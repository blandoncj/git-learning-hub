# Solución - Ejercicio 5: Stash y Tags

## Comandos Completos

### 1. Preparación

```bash
git checkout main
git status
```

**Salida esperada:**

```
On branch main
nothing to commit, working tree clean
```

---

### 2. Hacer cambios sin commitear

```bash
# Modificar index.html
sed -i '/<\/body>/i\    <p>Trabajo en progreso - nueva funcionalidad</p>' index.html

# Crear nuevo archivo
cat > temp.html << 'EOF'
<!DOCTYPE html>
<html>
  <body>
    <h1>Archivo temporal</h1>
  </body>
</html>
EOF

# Verificar estado
git status
```

**Salida esperada:**

```
On branch main
Changes not staged for commit:
  modified:   index.html

Untracked files:
  temp.html
```

---

### 3. Usar stash básico

```bash
# Guardar cambios temporalmente
git stash
# O con mensaje más descriptivo
git stash push -m "WIP: nueva funcionalidad"
```

**Salida esperada:**

```
Saved working directory and index state WIP on main: abc1234 feat: último commit
```

**Verificar:**

```bash
git status
# On branch main
# nothing to commit, working tree clean

ls  # temp.html NO aparece (no fue guardado)
cat index.html  # Cambios desaparecieron
```

**Respuesta:** Los cambios están guardados en el stash, el working directory volvió al estado del último commit. Los archivos untracked NO se guardaron.

---

### 4. Ver stash list

```bash
git stash list
```

**Salida esperada:**

```
stash@{0}: WIP on main: abc1234 feat: último commit
```

**Ver más detalles:**

```bash
# Ver estadísticas
git stash list --stat

# Ver con formato personalizado
git stash list --pretty=format:"%C(red)%h%C(reset) - %C(yellow)%s%C(reset) %C(green)(%cr)%C(reset)"
```

---

### 5. Aplicar stash

```bash
# Opción 1: Pop (aplica y elimina del stash)
git stash pop

# Opción 2: Apply (aplica pero mantiene en el stash)
git stash apply
```

**Salida esperada (pop):**

```
On branch main
Changes not staged for commit:
  modified:   index.html

Dropped refs/stash@{0} (def5678...)
```

**Respuesta a la pregunta:**

- **`pop`**: Aplica cambios Y elimina del stash
- **`apply`**: Aplica cambios pero MANTIENE en el stash (útil para aplicar en múltiples ramas)

**Cuándo usar cada uno:**

```bash
# pop: Caso normal, ya no necesitas el stash
git stash pop

# apply: Quieres aplicar mismo stash en varias ramas
git checkout feature-a
git stash apply
git checkout feature-b
git stash apply
```

---

### 6. Stash con mensaje

```bash
# Modificar about.html
echo "<p>Otro cambio temporal</p>" >> about.html

# Guardar con mensaje descriptivo
git stash push -m "cambios en página acerca de"
```

**Verificar:**

```bash
git stash list
# stash@{0}: On main: cambios en página acerca de
# stash@{1}: WIP on main: abc1234 (si quedó del apply)
```

---

### 7. Ver contenido del stash

```bash
# Ver diferencias del stash más reciente
git stash show

# Ver diff completo
git stash show -p
# O
git stash show -p stash@{0}

# Ver stash específico
git stash show -p stash@{1}
```

**Salida esperada:**

```diff
diff --git a/about.html b/about.html
index abc1234..def5678 100644
--- a/about.html
+++ b/about.html
@@ -6,5 +6,6 @@
   <body>
     <h1>Acerca de Mí</h1>
     <p>Soy un desarrollador aprendiendo Git</p>
+    <p>Otro cambio temporal</p>
   </body>
 </html>
```

---

### 8. Stash incluyendo archivos untracked

```bash
# Crear archivo no rastreado
echo "test" > nuevo.txt

# Guardar incluyendo untracked files
git stash push -u -m "con archivos nuevos"
# O
git stash push --include-untracked -m "con archivos nuevos"

# También hay opción para TODO (incluso ignored)
git stash push -a  # --all
```

**Verificar:**

```bash
ls  # nuevo.txt desapareció
git stash list
git stash show stash@{0}  # Verás nuevo.txt listado
```

---

### 9. Limpiar stashes

```bash
# Eliminar stash específico
git stash drop stash@{0}

# Eliminar el más reciente
git stash drop

# Eliminar TODOS los stashes (¡cuidado!)
git stash clear
```

**Comandos relacionados:**

```bash
# Ver lista antes de limpiar
git stash list

# Eliminar stash específico por índice
git stash drop stash@{2}

# Verificar que se eliminó
git stash list
```

---

### 10. Crear tu primer tag

```bash
# Hacer commit primero
git add .
git commit -m "feat: completar funcionalidades básicas"

# Crear tag anotado
git tag -a v1.0.0 -m "Primera versión estable"
```

**Alternativas:**

```bash
# Tag anotado con editor
git tag -a v1.0.0  # Se abre editor para mensaje

# Tag en commit específico
git tag -a v1.0.0 abc1234 -m "versión 1.0.0"
```

---

### 11. Listar tags

```bash
# Listar todos los tags
git tag

# Listar con patrón
git tag -l "v1.*"

# Listar con detalles
git tag -n
git tag -n99  # Mostrar más líneas del mensaje
```

**Salida esperada:**

```
v1.0.0
```

---

### 12. Ver información del tag

```bash
# Ver información completa del tag anotado
git show v1.0.0
```

**Salida esperada:**

```
tag v1.0.0
Tagger: Tu Nombre <tu@email.com>
Date:   Fri Oct 17 14:30:00 2025 -0500

Primera versión estable

commit abc1234def5678...
Author: Tu Nombre <tu@email.com>
Date:   Fri Oct 17 14:30:00 2025 -0500

    feat: completar funcionalidades básicas

diff --git a/index.html b/index.html
...
```

**Otros comandos:**

```bash
# Ver solo el commit
git show v1.0.0^{commit}

# Ver referencias del tag
git show-ref --tags
```

---

### 13. Crear tag ligero

```bash
# Hacer commit pequeño
echo "<!-- v1.0.1 -->" >> index.html
git add index.html
git commit -m "fix: corrección menor"

# Crear tag ligero (sin -a, sin mensaje)
git tag v1.0.1
```

**Verificar:**

```bash
git show v1.0.1
# Solo muestra el commit, no información del tag
```

**Respuesta a la pregunta:**

**Tag Anotado:**

- Tiene mensaje, autor, fecha
- Es un objeto completo en Git
- Recomendado para releases
- Más información y trazabilidad

```bash
git tag -a v1.0.0 -m "mensaje"
```

**Tag Ligero:**

- Solo un puntero a un commit
- Sin metadata adicional
- Útil para marcadores temporales
- Más simple y rápido

```bash
git tag v1.0.1  # Sin -a
```

**Comparación:**

```bash
# Ver diferencia
git cat-file -t v1.0.0  # Output: tag (objeto tag)
git cat-file -t v1.0.1  # Output: commit (solo puntero)
```

---

### 14. Push de tags

```bash
# Push de tag específico
git push origin v1.0.0

# Push de todos los tags
git push origin --tags

# Push de tags anotados solamente
git push origin --follow-tags
```

**Verificar en GitHub:**

- Ve a tu repositorio
- Click en "tags" o "releases"
- Deberías ver tus tags listados

**Crear release en GitHub (opcional):**

1. Ve a la pestaña "Releases"
2. Click "Draft a new release"
3. Selecciona el tag v1.0.0
4. Agrega título y descripción
5. Click "Publish release"

---

### 15. Checkout a un tag

```bash
# Ver estado actual
git log --oneline

# Crear rama desde un tag
git checkout -b version-1.0.0 v1.0.0

# Verificar
git branch
# * version-1.0.0
#   main

git log --oneline -1
```

**Alternativa (solo ver, no crear rama):**

```bash
# Checkout directo (detached HEAD)
git checkout v1.0.0
# Warning: you are in 'detached HEAD' state

# Ver el código en ese punto
ls
cat index.html

# Volver a main
git checkout main
```

---

## Verificación Final

```bash
# Ver stashes actuales
git stash list

# Ver tags
git tag
# v1.0.0
# v1.0.1

# Ver historial con tags
git log --oneline --decorate
# abc1234 (HEAD -> main, tag: v1.0.1, origin/main) fix: corrección menor
# def5678 (tag: v1.0.0) feat: completar funcionalidades básicas
# ...

# Ver tags remotos
git ls-remote --tags origin
```

---

## Respuestas a Preguntas de Reflexión

### 1. ¿Cuándo es útil usar stash en lugar de hacer un commit?

**Usa stash cuando:**

1. **Necesitas cambiar de rama urgentemente:**

```bash
# Estás en feature/login con cambios sin terminar
git stash
git checkout main
git checkout -b hotfix/urgent
# ... arreglar bug ...
git checkout feature/login
git stash pop
```

2. **Trabajo a medio terminar:**

```bash
# No quieres commitear código incompleto
git stash push -m "WIP: implementando validación"
# Trabajar en otra cosa
git stash pop  # Continuar más tarde
```

3. **Experimentos:**

```bash
# Probar algo sin commitear
git stash
# Experimentar
git stash pop  # Si funcionó
# o
git stash drop  # Si no funcionó
```

4. **Pull con cambios locales:**

```bash
# Tienes cambios y necesitas hacer pull
git stash
git pull
git stash pop
```

**NO uses stash cuando:**

- El trabajo está listo (usa commit)
- Necesitas compartir con otros (usa commit y push)
- Quieres guardar a largo plazo (usa commit)
- Es un punto importante en la historia (usa commit)

---

### 2. ¿Qué es el versionado semántico (SemVer)?

**Definición:**
Sistema de versionado con formato `MAJOR.MINOR.PATCH`

```
v1.2.3
│ │ └── PATCH: Bug fixes (compatibles)
│ └──── MINOR: Nuevas features (compatibles)
└────── MAJOR: Cambios incompatibles
```

**Reglas:**

1. **MAJOR (1.x.x):**

```bash
# Cambios que rompen compatibilidad
# Ejemplo: Remover función pública, cambiar API
v1.0.0 → v2.0.0
```

2. **MINOR (x.1.x):**

```bash
# Nuevas features compatibles
# Ejemplo: Agregar nueva función sin afectar existentes
v1.0.0 → v1.1.0
```

3. **PATCH (x.x.1):**

```bash
# Bug fixes compatibles
# Ejemplo: Corregir bug sin cambiar funcionalidad
v1.0.0 → v1.0.1
```

**Ejemplos:**

```
v0.1.0  →  Desarrollo inicial
v0.2.0  →  Más features en desarrollo
v1.0.0  →  Primera versión estable
v1.0.1  →  Bug fix
v1.1.0  →  Nueva feature
v2.0.0  →  Breaking changes
```

**Pre-releases:**

```
v1.0.0-alpha
v1.0.0-beta
v1.0.0-rc.1  (release candidate)
v1.0.0       (release final)
```

---

### 3. ¿Por qué usarías tags en lugar de solo recordar hashes de commits?

**Ventajas de tags:**

1. **Legibilidad humana:**

```bash
# ❌ Difícil de recordar
git checkout 7a8b9c0d1e2f3g4h5i6j

# ✅ Fácil de entender
git checkout v1.0.0
```

2. **Significado semántico:**

```bash
# Tags comunican qué contiene esa versión
v2.0.0  # Breaking changes
v1.5.2  # Bug fix de v1.5.1
```

3. **Permanencia:**

```bash
# Los hashes no se mueven, pero son crípticos
# Los tags son puntos de referencia claros
```

4. **Integración con herramientas:**

```bash
# GitHub Releases
# Sistemas de CI/CD
# Gestores de paquetes (npm, pip, etc.)
```

5. **Comunicación de equipo:**

```bash
# "Deployemos v2.1.0"  ✅ Claro
# "Deployemos 7a8b9c0"  ❌ Confuso
```

6. **Changelog y documentación:**

```
## v1.2.0 (2025-10-15)
### Features
- Agregar login social
### Bug Fixes
- Corregir validación de email
```

---

## Comandos Avanzados

### Stash avanzado

```bash
# Stash solo archivos staged
git stash push --staged

# Stash archivos específicos
git stash push -m "mensaje" archivo1.txt archivo2.txt

# Stash con patch interactivo
git stash push -p  # Seleccionar hunks interactivamente

# Crear rama desde stash
git stash branch nueva-rama stash@{0}

# Aplicar stash en otra rama
git checkout otra-rama
git stash apply stash@{1}
```

### Tags avanzados

```bash
# Tag en commit anterior
git tag -a v0.9.0 HEAD~5 -m "versión anterior"

# Mover tag existente
git tag -f v1.0.0 nuevo-commit-hash
git push origin v1.0.0 --force

# Eliminar tag local
git tag -d v1.0.0

# Eliminar tag remoto
git push origin --delete v1.0.0
# O
git push origin :refs/tags/v1.0.0

# Verificar firma de tag (si está firmado)
git tag -v v1.0.0

# Crear tag firmado con GPG
git tag -s v1.0.0 -m "versión firmada"
```

### Comparar tags

```bash
# Ver cambios entre tags
git log v1.0.0..v1.1.0
git log --oneline v1.0.0..v1.1.0

# Ver diferencias de código
git diff v1.0.0 v1.1.0

# Ver estadísticas
git diff --stat v1.0.0 v1.1.0

# Ver archivos cambiados
git diff --name-only v1.0.0 v1.1.0
```

---

## Workflows Prácticos

### Workflow de feature con stash

```bash
# 1. Trabajando en feature
git checkout -b feature/nueva
# ... codificando ...

# 2. Urgencia en main
git stash push -m "WIP: feature a medio hacer"

# 3. Arreglar urgencia
git checkout main
git checkout -b hotfix/urgente
# ... fix ...
git commit -m "fix: resolver problema urgente"
git checkout main
git merge hotfix/urgente

# 4. Volver a feature
git checkout feature/nueva
git stash pop
# ... continuar trabajando ...
```

### Workflow de releases con tags

```bash
# 1. Desarrollar en develop
git checkout develop
# ... commits ...

# 2. Crear release branch
git checkout -b release/v1.2.0

# 3. Preparar release
# ... ajustes finales ...
git commit -m "chore: preparar v1.2.0"

# 4. Merge a main y taggear
git checkout main
git merge release/v1.2.0
git tag -a v1.2.0 -m "Release 1.2.0"
git push origin main --tags

# 5. Merge de vuelta a develop
git checkout develop
git merge release/v1.2.0

# 6. Limpiar
git branch -d release/v1.2.0
```

---

## Errores Comunes y Soluciones

### Error: Conflicto al aplicar stash

```bash
# Al hacer stash pop hay conflictos
git stash pop
# CONFLICT: Merge conflict in index.html

# Resolver manualmente
# ... editar archivos ...
git add index.html

# NO necesitas git stash drop, el pop falló y el stash sigue ahí
# Puedes:
git stash drop  # Si ya no lo necesitas
# o
git stash  # Si quieres guardarlo de nuevo
```

### Error: Tag ya existe

```bash
# Intentar crear tag que ya existe
git tag v1.0.0
# fatal: tag 'v1.0.0' already exists

# Solución 1: Usar otro nombre
git tag v1.0.1

# Solución 2: Mover tag (forzar)
git tag -f v1.0.0
git push origin v1.0.0 --force
```

### Error: Push de tag rechazado

```bash
# Tag ya existe en remoto con diferente commit
git push origin v1.0.0
# error: remote contains work that you do not have

# Solución: Forzar (¡cuidado! Comunica al equipo)
git push origin v1.0.0 --force

# O eliminar y recrear
git push origin --delete v1.0.0
git tag -f v1.0.0
git push origin v1.0.0
```

---

## Desafío Extra - Soluciones

### 1. Script para crear tag incrementando versión

```bash
#!/bin/bash
# auto-tag.sh

# Obtener último tag
LAST_TAG=$(git describe --tags --abbrev=0 2>/dev/null)

if [ -z "$LAST_TAG" ]; then
    # No hay tags, crear v0.1.0
    NEW_TAG="v0.1.0"
else
    # Extraer MAJOR.MINOR.PATCH
    VERSION=${LAST_TAG#v}
    MAJOR=$(echo $VERSION | cut -d. -f1)
    MINOR=$(echo $VERSION | cut -d. -f2)
    PATCH=$(echo $VERSION | cut -d. -f3)

    # Preguntar qué incrementar
    echo "Último tag: $LAST_TAG"
    echo "1) MAJOR ($MAJOR → $((MAJOR+1)).0.0)"
    echo "2) MINOR ($MINOR → $MAJOR.$((MINOR+1)).0)"
    echo "3) PATCH ($PATCH → $MAJOR.$MINOR.$((PATCH+1)))"
    read -p "Selecciona (1/2/3): " choice

    case $choice in
        1) NEW_TAG="v$((MAJOR+1)).0.0" ;;
        2) NEW_TAG="v$MAJOR.$((MINOR+1)).0" ;;
        3) NEW_TAG="v$MAJOR.$MINOR.$((PATCH+1))" ;;
        *) echo "Opción inválida"; exit 1 ;;
    esac
fi

# Crear tag
read -p "Mensaje para $NEW_TAG: " MESSAGE
git tag -a $NEW_TAG -m "$MESSAGE"

echo "✅ Tag $NEW_TAG creado"
echo "Usa 'git push origin $NEW_TAG' para publicar"
```

**Uso:**

```bash
chmod +x auto-tag.sh
./auto-tag.sh
```

---

### 2. Investigar git stash branch

**¿Qué hace?**
Crea una nueva rama y aplica el stash en ella.

```bash
# Tienes cambios en stash
git stash list
# stash@{0}: WIP on main: abc1234 último commit

# Crear rama desde el commit donde se hizo el stash
git stash branch nueva-feature stash@{0}
```

**Resultado:**

1. Crea rama `nueva-feature` desde el commit donde se hizo stash
2. Aplica el stash en esa rama
3. Elimina el stash si aplica sin conflictos

**Cuándo usar:**

- Cuando el stash tiene muchos conflictos en la rama actual
- Para aislar experimentos en rama propia
- Cuando descubres que el trabajo merece su propia rama

**Ejemplo completo:**

```bash
# En main, experimentando
echo "experimento" >> test.txt
git stash push -m "experimento interesante"

# Más tarde decides que merece rama propia
git stash branch feature/experimento stash@{0}

# Ahora estás en nueva rama con los cambios
git status  # test.txt modificado
git add test.txt
git commit -m "feat: agregar experimento"
```

---

## Casos de Uso Reales

### Caso 1: Hotfix urgente mientras desarrollas

```bash
# Desarrollando feature
git checkout -b feature/payment
# ... 2 horas de trabajo ...

# ¡Bug crítico en producción!
git stash push -u -m "WIP: payment feature"

# Arreglar
git checkout main
git pull
git checkout -b hotfix/critical-bug
# ... fix ...
git commit -m "fix: resolver bug crítico"
git push origin hotfix/critical-bug

# Volver a tu trabajo
git checkout feature/payment
git stash pop

# Actualizar con el hotfix
git fetch origin
git rebase origin/main
```

---

### Caso 2: Múltiples experimentos con stash

```bash
# Experimento 1
echo "opción A" > feature.txt
git stash push -m "experimento: opción A"

# Experimento 2
echo "opción B" > feature.txt
git stash push -m "experimento: opción B"

# Experimento 3
echo "opción C" > feature.txt
git stash push -m "experimento: opción C"

# Probar cada uno
git stash apply stash@{2}  # Opción A
# ... probar ...
git restore feature.txt

git stash apply stash@{1}  # Opción B
# ... probar ...
git restore feature.txt

git stash apply stash@{0}  # Opción C
# ... probar ...

# Elegir la mejor y commitear
git add feature.txt
git commit -m "feat: implementar opción C"

# Limpiar stashes no usados
git stash clear
```

---

### Caso 3: Versionado de releases

```bash
# Release inicial
git tag -a v1.0.0 -m "Primera versión estable"

# Bug fixes
git tag -a v1.0.1 -m "Fix: corregir login"
git tag -a v1.0.2 -m "Fix: mejorar validación"

# Nueva feature compatible
git tag -a v1.1.0 -m "Feature: agregar dashboard"

# Otra feature
git tag -a v1.2.0 -m "Feature: agregar reportes"

# Breaking change
git tag -a v2.0.0 -m "Breaking: nueva API REST"

# Ver historial de releases
git tag -l -n1
# v1.0.0  Primera versión estable
# v1.0.1  Fix: corregir login
# v1.0.2  Fix: mejorar validación
# v1.1.0  Feature: agregar dashboard
# v1.2.0  Feature: agregar reportes
# v2.0.0  Breaking: nueva API REST
```

---

## Integración con CI/CD

### GitHub Actions con tags

```yaml
# .github/workflows/release.yml
name: Release

on:
  push:
    tags:
      - "v*"

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Get tag
        id: tag
        run: echo "::set-output name=tag::${GITHUB_REF#refs/tags/}"

      - name: Build
        run: npm run build

      - name: Create Release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ steps.tag.outputs.tag }}
          release_name: Release ${{ steps.tag.outputs.tag }}
```

**Uso:**

```bash
git tag -a v1.5.0 -m "Nueva versión"
git push origin v1.5.0
# GitHub Actions automáticamente crea release
```

---

## Aliases Útiles

```bash
# Stash
git config --global alias.save 'stash push -u'
git config --global alias.pop 'stash pop'
git config --global alias.stashes 'stash list'

# Tags
git config --global alias.tags 'tag -l -n1'
git config --global alias.lasttag 'describe --tags --abbrev=0'

# Uso
git save "WIP: feature"
git pop
git stashes
git tags
git lasttag
```

---

## Herramientas Complementarias

### Git Extensions para VS Code

```json
// settings.json
{
  "git.enableSmartCommit": true,
  "git.autofetch": true,
  "gitlens.advanced.messages": {
    "suppressCommitHasNoPreviousCommitWarning": false
  }
}
```

### Hooks para automatizar tags

```bash
# .git/hooks/post-commit
#!/bin/bash

# Auto-tag cada N commits
COMMIT_COUNT=$(git rev-list --count HEAD)
if [ $((COMMIT_COUNT % 10)) -eq 0 ]; then
    LAST_TAG=$(git describe --tags --abbrev=0 2>/dev/null || echo "v0.0.0")
    echo "¿Crear tag después de 10 commits? (actual: $LAST_TAG)"
fi
```

---

## Mejores Prácticas Finales

### Stash

✅ **Hacer:**

- Usar mensajes descriptivos
- Limpiar stashes regularmente
- Usar para trabajo temporal
- Incluir untracked files cuando sea necesario

❌ **Evitar:**

- Acumular muchos stashes
- Usarlo como backup permanente
- Olvidar stashes por semanas
- Depender del stash para compartir código

### Tags

✅ **Hacer:**

- Seguir SemVer consistentemente
- Usar tags anotados para releases
- Documentar cambios en mensaje del tag
- Pushear tags después de release
- Crear changelog basado en tags

❌ **Evitar:**

- Mover tags después de publicar
- Tags ambiguos (v1, final, latest)
- Olvidar pushear tags
- Tags sin mensaje descriptivo
- Eliminar tags públicos

---

## Checklist de Dominio

Al completar este ejercicio deberías poder:

- [ ] Usar `git stash` para guardar trabajo temporal
- [ ] Aplicar stashes con `pop` y `apply`
- [ ] Crear stashes con mensajes descriptivos
- [ ] Incluir archivos untracked en stash
- [ ] Crear tags anotados y ligeros
- [ ] Diferenciar entre tipos de tags
- [ ] Pushear tags al remoto
- [ ] Listar y ver información de tags
- [ ] Entender versionado semántico
- [ ] Crear releases desde tags
- [ ] Usar stash branch para experimentos
- [ ] Limpiar stashes no necesarios

---

## Recursos Adicionales

### Documentación

- [Git Stash Documentation](https://git-scm.com/docs/git-stash)
- [Git Tag Documentation](https://git-scm.com/docs/git-tag)
- [Semantic Versioning](https://semver.org/)

### Cheat Sheet Stash

```bash
git stash                    # Guardar cambios
git stash push -m "msg"      # Con mensaje
git stash push -u            # Incluir untracked
git stash list               # Ver lista
git stash show -p            # Ver contenido
git stash pop               # Aplicar y eliminar
git stash apply             # Aplicar y mantener
git stash drop              # Eliminar uno
git stash clear             # Eliminar todos
git stash branch <nombre>   # Crear rama desde stash
```

### Cheat Sheet Tags

```bash
git tag                      # Listar tags
git tag v1.0.0              # Tag ligero
git tag -a v1.0.0 -m "msg"  # Tag anotado
git show v1.0.0             # Ver info
git push origin v1.0.0      # Pushear tag
git push origin --tags      # Pushear todos
git tag -d v1.0.0           # Eliminar local
git push origin --delete v1.0.0  # Eliminar remoto
git checkout v1.0.0         # Checkout a tag
git describe --tags         # Tag más cercano
```

---

## Próximos Pasos

Has completado el Ejercicio 5. Ahora dominas:

- ✅ Uso de stash para trabajo temporal
- ✅ Creación y gestión de tags
- ✅ Versionado semántico
- ✅ Workflows de releases

**Continúa con:** [Ejercicio 6 - Rebase Interactivo →](../exercise-06.md)

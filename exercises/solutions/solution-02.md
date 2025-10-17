# Solución - Ejercicio 2: Trabajar con Ramas

## Comandos Completos

### 1. Ver ramas actuales

```bash
git branch
```

**Respuesta:**

```
* main
```

**Explicación:**

- Una rama (main o master)
- El asterisco (\*) indica la rama activa
- Color verde = rama actual

**Comandos relacionados:**

```bash
git branch -v          # Ver último commit de cada rama
git branch -a          # Ver todas las ramas (incluye remotas)
git branch -r          # Solo ramas remotas
```

---

### 2. Crear una nueva rama

```bash
git branch feature/pagina-acerca-de
```

**Verificar:**

```bash
git branch
# * main
#   feature/pagina-acerca-de
```

---

### 3. Cambiar a la nueva rama

```bash
# Opción 1: Git tradicional
git checkout feature/pagina-acerca-de

# Opción 2: Git moderno (2.23+)
git switch feature/pagina-acerca-de

# Opción 3: Crear y cambiar en un comando
git checkout -b feature/pagina-acerca-de  # Si no existe
git switch -c feature/pagina-acerca-de    # Alternativa moderna
```

**Verificar:**

```bash
git branch
#   main
# * feature/pagina-acerca-de
```

---

### 4. Crear archivo about.html

```bash
cat > about.html << 'EOF'
<!DOCTYPE html>
<html>
  <head>
    <title>Acerca de</title>
  </head>
  <body>
    <h1>Acerca de Mí</h1>
    <p>Soy un desarrollador aprendiendo Git</p>
  </body>
</html>
EOF
```

---

### 5. Hacer commit en la rama

```bash
git add about.html
git commit -m "feat: agregar página acerca de"
```

**Alternativa:**

```bash
git add about.html && git commit -m "feat: agregar página acerca de"
```

---

### 6. Volver a main

```bash
git checkout main
# o
git switch main
```

**Respuesta a la pregunta:**
El archivo `about.html` desaparece porque está solo en la rama `feature/pagina-acerca-de`.

**Verificar:**

```bash
ls
# Solo verás: index.html, styles.css (no about.html)

git checkout feature/pagina-acerca-de
ls
# Ahora sí verás: index.html, styles.css, about.html
```

**Explicación:** Los archivos no se borran, simplemente Git cambia el working directory al estado de cada rama.

---

### 7. Fusionar la rama

```bash
git checkout main
git merge feature/pagina-acerca-de
```

**Salida esperada:**

```
Updating abc1234..def5678
Fast-forward
 about.html | 9 +++++++++
 1 file changed, 9 insertions(+)
 create mode 100644 about.html
```

**Tipos de merge:**

- **Fast-forward:** Main no ha avanzado, solo mueve el puntero
- **Three-way merge:** Ambas ramas tienen commits nuevos, se crea commit de merge

---

### 8. Verificar la fusión

```bash
# Ver archivos
ls
# Ahora about.html está en main

# Ver historial
git log --oneline
# def5678 feat: agregar página acerca de
# abc1234 feat: agregar párrafo de bienvenida
# ...

# Ver gráfico
git log --oneline --graph --all
```

---

### 9. Eliminar la rama

```bash
git branch -d feature/pagina-acerca-de
```

**Salida:**

```
Deleted branch feature/pagina-acerca-de (was def5678).
```

**Opciones:**

```bash
git branch -d rama   # Eliminar (seguro, solo si está mergeada)
git branch -D rama   # Forzar eliminación (¡cuidado!)
```

---

### 10. Crear conflicto (simulación)

**Paso A: Modificar en main**

```bash
git checkout main

# Modificar index.html
sed -i 's/<h1>Hola Mundo<\/h1>/<h1>¡Hola Mundo desde Main!<\/h1>/' index.html

# Commitear
git add index.html
git commit -m "update: cambiar título en main"
```

**Paso B: Crear y cambiar a nueva rama**

```bash
git checkout -b feature/nuevo-titulo
```

**Paso C: Modificar el mismo h1**

```bash
sed -i 's/<h1>¡Hola Mundo desde Main!<\/h1>/<h1>Bienvenido a Mi Sitio<\/h1>/' index.html

git add index.html
git commit -m "update: cambiar a título de bienvenida"
```

**Paso D: Intentar fusionar**

```bash
git checkout main
git merge feature/nuevo-titulo
```

**Respuesta esperada:**

```
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

**Verificar conflicto:**

```bash
git status
# both modified:   index.html
```

---

### 11. Resolver el conflicto

**Ver el archivo con conflicto:**

```bash
cat index.html
```

**Contenido con marcadores de conflicto:**

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Mi Sitio Web</title>
  </head>
  <body>
    <<<<<<< HEAD
    <h1>¡Hola Mundo desde Main!</h1>
    =======
    <h1>Bienvenido a Mi Sitio</h1>
    >>>>>>> feature/nuevo-titulo
    <p>Bienvenido a mi sitio web</p>
  </body>
</html>
```

**Explicación de marcadores:**

- `<<<<<<< HEAD`: Inicio del contenido en la rama actual (main)
- `=======`: Separador entre versiones
- `>>>>>>> feature/nuevo-titulo`: Fin del contenido de la otra rama

**Resolver manualmente:**

```bash
# Editar el archivo eliminando marcadores y eligiendo la versión final
cat > index.html << 'EOF'
<!DOCTYPE html>
<html>
  <head>
    <title>Mi Sitio Web</title>
  </head>
  <body>
    <h1>Bienvenido a Mi Sitio Web</h1>
    <p>Bienvenido a mi sitio web</p>
  </body>
</html>
EOF
```

**Completar el merge:**

```bash
# Marcar como resuelto
git add index.html

# Verificar estado
git status
# All conflicts fixed but you are still merging.

# Completar merge
git commit -m "merge: resolver conflicto de título"
# o simplemente
git commit  # Usará mensaje por defecto
```

**Alternativas durante el merge:**

```bash
# Aceptar versión de main (nuestra)
git checkout --ours index.html

# Aceptar versión de la otra rama (suya)
git checkout --theirs index.html

# Abortar el merge completamente
git merge --abort
```

---

## Verificación Final

```bash
# Ver ramas
git branch
# * main
#   feature/nuevo-titulo

# Ver historial con gráfico
git log --oneline --graph --all
# *   ghi7890 merge: resolver conflicto de título
# |\
# | * jkl3456 update: cambiar a título de bienvenida
# * | mno1234 update: cambiar título en main
# |/
# * def5678 feat: agregar página acerca de
# * abc1234 feat: agregar párrafo de bienvenida

# Ver archivos
ls
# about.html  index.html  styles.css

# Estado limpio
git status
# On branch main
# nothing to commit, working tree clean
```

---

## Respuestas a Preguntas de Reflexión

### 1. ¿Cuándo es útil trabajar con ramas?

**Casos de uso:**

- **Desarrollar features nuevas:** Sin afectar código estable
- **Experimentar:** Probar ideas sin riesgo
- **Hotfixes:** Arreglar bugs urgentes mientras desarrollas features
- **Colaboración:** Varios desarrolladores trabajando simultáneamente
- **Code review:** Revisión antes de merge a main
- **Releases:** Preparar versiones para producción

**Ejemplo de workflow:**

```bash
main          ←  Código en producción
  ↓
develop       ←  Integración de features
  ↓
feature/*     ←  Desarrollo de funcionalidades
hotfix/*      ←  Correcciones urgentes
release/*     ←  Preparación de versiones
```

### 2. ¿Qué es un merge conflict y por qué ocurre?

**Definición:**
Un conflicto ocurre cuando Git no puede fusionar cambios automáticamente porque dos ramas modificaron las mismas líneas de código.

**Causas comunes:**

1. Dos personas editan las mismas líneas
2. Una persona elimina un archivo que otra modificó
3. Cambios en líneas adyacentes (a veces)

**No hay conflicto cuando:**

- Se modifican archivos diferentes
- Se modifican líneas diferentes del mismo archivo
- Se agregan líneas en lugares diferentes

### 3. ¿Cuál es la diferencia entre git merge y git rebase?

**Git Merge:**

```bash
# Crea un commit de merge
git merge feature

# Historial:
*   Merge branch 'feature'
|\
| * Commit en feature
* | Commit en main
|/
* Commit base
```

**Ventajas:**

- Preserva historia completa
- Seguro (no reescribe historia)
- Muestra puntos de integración

**Desventajas:**

- Historia con muchas ramas puede ser confusa
- Commits de merge adicionales

**Git Rebase:**

```bash
# Reaplica commits sobre otra base
git rebase main

# Historial:
* Commit en feature (reaplicado)
* Commit en main
* Commit base
```

**Ventajas:**

- Historia lineal y limpia
- Más fácil de leer
- Sin commits de merge

**Desventajas:**

- Reescribe historia (peligroso en ramas compartidas)
- Conflictos pueden ser más complejos
- Pierde información de cuándo se integraron cambios

**Regla de oro:**

```bash
# ✅ Rebase en ramas locales/personales
git rebase main

# ❌ NO rebase en ramas públicas/compartidas
# Nunca hagas rebase de commits que otros ya tienen
```

---

## Desafío Extra - Solución

### Crear rama con navegación

```bash
# Crear y cambiar a nueva rama
git checkout -b feature/navegacion

# Crear componente de navegación
cat > nav.html << 'EOF'
<nav>
  <ul>
    <li><a href="index.html">Inicio</a></li>
    <li><a href="about.html">Acerca de</a></li>
    <li><a href="contact.html">Contacto</a></li>
  </ul>
</nav>
EOF

# Agregar navegación a index.html
sed -i '/<body>/r nav.html' index.html

# Agregar navegación a about.html
sed -i '/<body>/r nav.html' about.html

# Commitear cambios
git add .
git commit -m "feat: agregar menú de navegación"

# Volver a main y fusionar
git checkout main
git merge feature/navegacion

# Limpiar
git branch -d feature/navegacion
```

---

## Comandos Útiles Adicionales

### Gestión de ramas

```bash
# Ver última actividad de cada rama
git branch -v

# Ver ramas fusionadas
git branch --merged

# Ver ramas no fusionadas
git branch --no-merged

# Renombrar rama actual
git branch -m nuevo-nombre

# Renombrar otra rama
git branch -m viejo-nombre nuevo-nombre

# Ver ramas remotas
git branch -r

# Ver todas las ramas
git branch -a
```

### Checkout avanzado

```bash
# Crear rama desde commit específico
git checkout -b nueva-rama abc1234

# Volver a rama anterior
git checkout -

# Checkout de archivo específico de otra rama
git checkout otra-rama -- archivo.txt
```

### Merge avanzado

```bash
# Merge sin fast-forward (siempre crear commit de merge)
git merge --no-ff feature

# Merge con estrategia específica
git merge -X ours feature      # Preferir nuestra versión en conflictos
git merge -X theirs feature    # Preferir su versión en conflictos

# Abortar merge en progreso
git merge --abort

# Ver archivos en conflicto
git diff --name-only --diff-filter=U
```

### Comparar ramas

```bash
# Ver commits en feature que no están en main
git log main..feature

# Ver commits en main que no están en feature
git log feature..main

# Ver diferencias entre ramas
git diff main..feature

# Ver archivos diferentes
git diff --name-only main..feature
```

---

## Errores Comunes y Soluciones

### Error: "Your local changes would be overwritten by checkout"

```bash
# Problema: Tienes cambios sin commitear

# Solución 1: Guardar cambios
git stash
git checkout otra-rama
git stash pop

# Solución 2: Commitear cambios
git add .
git commit -m "WIP: trabajo en progreso"
git checkout otra-rama

# Solución 3: Descartar cambios (¡cuidado!)
git checkout -- .
git checkout otra-rama
```

### Error: "Cannot delete branch - not fully merged"

```bash
# Problema: La rama tiene commits no fusionados

# Ver qué commits no están fusionados
git log main..rama-a-eliminar

# Solución 1: Fusionar primero
git checkout main
git merge rama-a-eliminar
git branch -d rama-a-eliminar

# Solución 2: Forzar eliminación (perderás commits)
git branch -D rama-a-eliminar
```

### Error: Conflictos complejos durante merge

```bash
# Ver herramientas de merge
git mergetool

# Usar editor visual (VS Code, etc)
code index.html

# Aceptar versión completa de un lado
git checkout --ours archivo.txt    # Nuestra versión
git checkout --theirs archivo.txt  # Su versión

# Si el conflicto es muy complejo, abortar
git merge --abort
```

### Error: Olvidé en qué rama estoy

```bash
# Ver rama actual
git branch
git status
git branch --show-current  # Solo el nombre

# Ver último commit
git log -1
```

---

## Visualización de Ramas

### Aliases útiles para ver historia

```bash
# Configurar alias bonito
git config --global alias.tree "log --graph --oneline --all --decorate"

# Usar
git tree

# Alias más detallado
git config --global alias.hist "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --all"

# Usar
git hist
```

### Herramientas visuales

```bash
# Git GUI incluido
gitk --all

# GitKraken (GUI externa)
# SourceTree (GUI externa)
# VS Code con extensión Git Graph
```

---

## Workflow de Ramas Recomendado

### Para proyectos pequeños/personales

```bash
main              # Código estable
feature/*         # Features en desarrollo
```

### Para equipos pequeños

```bash
main              # Producción
develop           # Integración
feature/*         # Features
hotfix/*          # Correcciones urgentes
```

### Git Flow completo (equipos grandes)

```bash
main              # Producción
develop           # Desarrollo
feature/*         # Features
release/*         # Preparación de releases
hotfix/*          # Hotfixes
```

---

## Buenas Prácticas

### Naming de ramas

```bash
# ✅ Bueno
feature/login-page
feature/user-authentication
bugfix/header-styling
hotfix/critical-security-issue

# ❌ Malo
new-feature
fix
temp
test
```

### Commits en ramas

```bash
# Hacer commits frecuentes
git commit -m "feat: agregar formulario de login"
git commit -m "style: aplicar estilos al formulario"
git commit -m "test: agregar tests unitarios"

# Antes de merge, limpiar historia con rebase interactivo
git rebase -i HEAD~3
```

### Antes de merge

```bash
# 1. Actualizar main
git checkout main
git pull

# 2. Actualizar tu feature con main
git checkout feature/mi-feature
git rebase main  # o git merge main

# 3. Resolver conflictos si hay

# 4. Hacer merge
git checkout main
git merge feature/mi-feature

# 5. Eliminar rama
git branch -d feature/mi-feature
```

---

## Testing de Resolución de Conflictos

### Práctica adicional

```bash
# Crear escenario de conflicto controlado

# En main
echo "Versión main" > conflict.txt
git add conflict.txt
git commit -m "agregar conflict.txt en main"

# En feature
git checkout -b test-conflict
echo "Versión feature" > conflict.txt
git add conflict.txt
git commit -m "modificar conflict.txt en feature"

# Intentar merge
git checkout main
git merge test-conflict
# Resolver conflicto manualmente
git add conflict.txt
git commit
```

---

## Próximos Pasos

Has completado el Ejercicio 2. Ahora dominas:

- ✅ Creación y gestión de ramas
- ✅ Merge de ramas
- ✅ Resolución de conflictos
- ✅ Cuándo usar merge vs rebase

**Continúa con:** [Ejercicio 3 - Git Remoto →](../exercise-03.md)

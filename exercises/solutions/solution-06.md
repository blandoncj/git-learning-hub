# Solución - Ejercicio 6: Rebase Interactivo y Historia Limpia

## Comandos Completos

### 1. Preparación

```bash
# Crear nueva rama para practicar
git checkout -b feature/rebase-practice
```

**Verificar:**

```bash
git branch
# * feature/rebase-practice
#   main
```

---

### 2. Crear varios commits "sucios"

```bash
echo "Línea 1" >> test.txt
git add test.txt
git commit -m "agregar test"

echo "Línea 2" >> test.txt
git add test.txt
git commit -m "fix typo"

echo "Línea 3" >> test.txt
git add test.txt
git commit -m "WIP"

echo "Línea 4" >> test.txt
git add test.txt
git commit -m "agregar mas cosas"

echo "Línea 5" >> test.txt
git add test.txt
git commit -m "fix: corregir bug"
```

**Ver el historial:**

```bash
git log --oneline
```

**Salida esperada:**

```
abc1234 fix: corregir bug
def5678 agregar mas cosas
ghi9012 WIP
jkl3456 fix typo
mno7890 agregar test
```

**Respuesta a la pregunta - Problemas con estos commits:**

- Mensajes poco descriptivos ("WIP", "fix typo")
- Commits demasiado granulares que deberían ser uno solo
- No siguen convenciones (algunos sin prefijo feat:/fix:)
- Historia sucia difícil de entender

---

### 3. Iniciar rebase interactivo

```bash
# Rebase de los últimos 5 commits
git rebase -i HEAD~5
```

**Se abrirá editor con:**

```
pick mno7890 agregar test
pick jkl3456 fix typo
pick ghi9012 WIP
pick def5678 agregar mas cosas
pick abc1234 fix: corregir bug

# Rebase xyz1234..abc1234 onto xyz1234 (5 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
```

---

### 4. Combinar commits (squash)

**Cambiar a:**

```
pick mno7890 agregar test
squash jkl3456 fix typo
squash ghi9012 WIP
squash def5678 agregar mas cosas
pick abc1234 fix: corregir bug
```

**Guardar y cerrar el editor.**

**Respuesta a la pregunta - ¿Qué hace squash?**
`squash` combina el commit con el commit anterior (`pick`), y te permite editar el mensaje combinado. Los cambios se fusionan pero mantienes la oportunidad de crear un mensaje descriptivo.

---

### 5. Editar mensaje del commit combinado

**Se abrirá otro editor con:**

```
# This is a combination of 4 commits.
# This is the 1st commit message:

agregar test

# This is the commit message #2:

fix typo

# This is the commit message #3:

WIP

# This is the commit message #4:

agregar mas cosas
```

**Reemplazar todo con:**

```
feat: agregar archivo de prueba con contenido inicial

- Crear archivo test.txt
- Agregar 4 líneas de contenido
- Implementar estructura básica
```

**Guardar y cerrar.**

**Salida esperada:**

```
[detached HEAD abc1234] feat: agregar archivo de prueba con contenido inicial
 Date: Fri Oct 17 10:00:00 2025 -0500
 1 file changed, 4 insertions(+)
 create mode 100644 test.txt
Successfully rebased and updated refs/heads/feature/rebase-practice.
```

---

### 6. Verificar resultado

```bash
git log --oneline
```

**Salida esperada:**

```
xyz9876 fix: corregir bug
abc1234 feat: agregar archivo de prueba con contenido inicial
def5678 (commit anterior en main)
```

**Respuesta:** Ahora tienes solo 2 commits en lugar de 5, con mensajes claros y descriptivos.

---

### 7. Reordenar commits

**Crear más commits:**

```bash
echo "Feature A" > feature-a.txt
git add feature-a.txt
git commit -m "feat: implementar feature A"

echo "Bugfix" > bugfix.txt
git add bugfix.txt
git commit -m "fix: arreglar bug crítico"

echo "Feature B" > feature-b.txt
git add feature-b.txt
git commit -m "feat: implementar feature B"
```

**Iniciar rebase:**

```bash
git rebase -i HEAD~3
```

**Reordenar poniendo el bugfix primero:**

```
pick [hash] fix: arreglar bug crítico
pick [hash] feat: implementar feature A
pick [hash] feat: implementar feature B
```

**Guardar y cerrar.**

**Verificar:**

```bash
git log --oneline
# Ahora el bugfix aparece primero
```

---

### 8. Editar un commit específico

**Iniciar rebase:**

```bash
git rebase -i HEAD~3
```

**Cambiar `pick` a `edit`:**

```
edit [hash] feat: implementar feature A
pick [hash] fix: arreglar bug crítico
pick [hash] feat: implementar feature B
```

**Guardar y cerrar.**

**Git se detendrá:**

```
Stopped at [hash]... feat: implementar feature A
You can amend the commit now, with

  git commit --amend

Once you are satisfied with your changes, run

  git rebase --continue
```

**Modificar el archivo:**

```bash
echo "Feature A - mejorada" > feature-a.txt
git add feature-a.txt
git commit --amend -m "feat: implementar feature A (mejorada)"
```

**Continuar el rebase:**

```bash
git rebase --continue
```

**Salida esperada:**

```
Successfully rebased and updated refs/heads/feature/rebase-practice.
```

---

### 9. Dividir un commit

**Crear un commit grande:**

```bash
echo "Parte 1" > multi.txt
echo "Parte 2" >> multi.txt
git add multi.txt
git commit -m "agregar multiples cambios"
```

**Iniciar rebase:**

```bash
git rebase -i HEAD~1
```

**Cambiar `pick` a `edit`:**

```
edit [hash] agregar multiples cambios
```

**Git se detiene. Deshacer el commit:**

```bash
# Reset mixto: deshace commit pero mantiene cambios
git reset HEAD~1
```

**Verificar:**

```bash
git status
# Changes not staged for commit:
#   modified:   multi.txt
```

**Hacer commits separados:**

```bash
# Primer commit - solo Parte 1
echo "Parte 1" > multi.txt
git add multi.txt
git commit -m "feat: agregar parte 1"

# Segundo commit - agregar Parte 2
echo "Parte 2" >> multi.txt
git add multi.txt
git commit -m "feat: agregar parte 2"
```

**Continuar rebase:**

```bash
git rebase --continue
```

**Verificar:**

```bash
git log --oneline -2
# Ahora tienes dos commits en lugar de uno
```

---

### 10. Usar fixup

**Crear un commit y luego un "arreglo":**

```bash
echo "Primera versión" > documento.txt
git add documento.txt
git commit -m "docs: agregar documentación"

echo "Corrección de typo" >> documento.txt
git add documento.txt
git commit -m "fix: typo en documentación"
```

**Rebase usando fixup:**

```bash
git rebase -i HEAD~2
```

**Cambiar el segundo commit a fixup:**

```
pick [hash] docs: agregar documentación
fixup [hash] fix: typo en documentación
```

**Guardar y cerrar.**

**Respuesta a la pregunta - Diferencia entre squash y fixup:**

| Comando  | Combina commits | Mensaje del commit                                     |
| -------- | --------------- | ------------------------------------------------------ |
| `squash` | ✅ Sí           | Abre editor para editar mensaje combinado              |
| `fixup`  | ✅ Sí           | Descarta mensaje del commit fixup, mantiene el primero |

**Cuándo usar cada uno:**

- **squash**: Cuando ambos commits son importantes y quieres mensaje descriptivo
- **fixup**: Para correcciones menores (typos, formato) donde el mensaje no importa

**Ejemplo práctico:**

```bash
# Squash - ambos mensajes importantes
pick abc1234 feat: implementar login
squash def5678 feat: agregar validación de login

# Fixup - corrección menor
pick abc1234 feat: implementar login
fixup def5678 fix: typo en variable
```

---

### 11. Rebase sobre main

**Volver a main y hacer un commit:**

```bash
git checkout main
echo "Cambio en main" >> index.html
git add index.html
git commit -m "feat: actualizar index en main"
```

**Volver a tu rama feature:**

```bash
git checkout feature/rebase-practice
```

**Actualizar tu rama con los cambios de main usando rebase:**

```bash
git rebase main
```

**Salida esperada:**

```
First, rewinding head to replay your work on top of it...
Applying: feat: agregar archivo de prueba con contenido inicial
Applying: fix: corregir bug
Applying: fix: arreglar bug crítico
Applying: feat: implementar feature A (mejorada)
Applying: feat: implementar feature B
Applying: docs: agregar documentación
```

**Respuesta a la pregunta - Diferencia entre merge y rebase:**

**Merge:**

```bash
git checkout feature
git merge main

# Historia resultante:
*   Merge branch 'main' into feature (commit de merge)
|\
| * Commit en main
* | Commits en feature
|/
* Commit base común
```

**Ventajas:**

- Preserva historia completa
- No reescribe commits
- Seguro para ramas compartidas
- Muestra puntos de integración

**Desventajas:**

- Historia con muchas ramas
- Commits de merge adicionales
- Más difícil de leer

**Rebase:**

```bash
git checkout feature
git rebase main

# Historia resultante:
* Commits en feature (reaplicados)
* Commit en main
* Commit base común
```

**Ventajas:**

- Historia lineal y limpia
- Fácil de leer
- Sin commits de merge
- Bisect más fácil

**Desventajas:**

- Reescribe historia (cambia hashes)
- Peligroso en ramas compartidas
- Conflictos pueden ser más complejos

**Regla de oro:**

```bash
# ✅ Rebase en ramas locales/privadas
git rebase main  # feature branch local

# ❌ NO rebase en ramas públicas/compartidas
git rebase main  # si otros tienen tus commits = PROBLEMA
```

---

### 12. Abortar un rebase

**Si algo sale mal durante un rebase:**

```bash
git rebase --abort
```

**Cuándo usar `--abort`:**

- Conflictos demasiado complicados
- Te das cuenta que no era el momento
- Cometiste error en las instrucciones
- Quieres intentar con otra estrategia

**Ejemplo:**

```bash
git rebase -i HEAD~10
# Oops, seleccioné commits equivocados
git rebase --abort  # Volver al estado anterior

# Intentar nuevamente
git rebase -i HEAD~5  # Correcto esta vez
```

**Otras opciones durante rebase:**

```bash
git rebase --continue  # Continuar después de resolver conflictos
git rebase --skip      # Saltar commit actual (si causa problemas)
git rebase --abort     # Cancelar completamente
```

---

## Verificación Final

```bash
# Ver historial limpio
git log --oneline --graph

# Debería verse lineal y organizado
# * abc1234 docs: agregar documentación
# * def5678 feat: implementar feature B
# * ghi9012 feat: implementar feature A (mejorada)
# * jkl3456 fix: arreglar bug crítico
# * mno7890 feat: agregar archivo de prueba con contenido inicial
# * pqr5678 feat: actualizar index en main

# Ver diferencias con main
git log main..HEAD --oneline

# Estado limpio
git status
```

---

## Respuestas a Preguntas de Reflexión

### 1. ¿Por qué es importante mantener una historia de commits limpia?

**Razones principales:**

1. **Facilita code review:**

```bash
# ✅ Historia limpia
feat: implementar sistema de usuarios
feat: agregar autenticación
fix: corregir validación de email

# ❌ Historia sucia
WIP
fix typo
oops forgot this
actually fix it now
final version (for real this time)
```

2. **Debugging más eficiente:**

```bash
# Con historia limpia puedes usar git bisect efectivamente
git bisect start
git bisect bad HEAD
git bisect good v1.0.0
# Git encuentra el commit problemático rápidamente
```

3. **Mejor documentación:**

```bash
# Historia limpia = documentación del proyecto
git log --oneline
# Puedes ver la evolución lógica del código
```

4. **Revertir cambios fácilmente:**

```bash
# Commits atómicos son fáciles de revertir
git revert abc1234  # Revertir feature específica

# vs commits mezclados donde no puedes revertir solo una parte
```

5. **Onboarding de nuevos desarrolladores:**

```bash
# Nuevos miembros pueden entender el código viendo la historia
git log --all --graph --oneline
```

6. **Genera changelogs automáticos:**

```bash
# Con conventional commits
git log --oneline v1.0.0..v2.0.0 | grep "feat:"
# Genera lista de nuevas features automáticamente
```

---

### 2. ¿Cuándo NO deberías usar rebase?

**Situaciones donde NO usar rebase:**

1. **En ramas públicas/compartidas:**

```bash
# ❌ MAL - Otros tienen estos commits
git checkout main
git rebase develop
git push --force  # Rompe el repositorio de todos

# ✅ BIEN - Usa merge
git checkout main
git merge develop
git push
```

2. **Después de push a repositorio compartido:**

```bash
# ❌ MAL
git push origin feature/login
# ... más tarde ...
git rebase main
git push --force origin feature/login  # Problemas para colaboradores

# ✅ BIEN
git push origin feature/login
# ... más tarde ...
git merge main  # O coordinar con equipo si necesitas rebase
```

3. **En commits que otros referenciaron:**

```bash
# Si alguien hizo cherry-pick o creó branch desde tus commits
# Rebase cambiará los hashes y causará confusión
```

4. **Cuando no entiendes bien el rebase:**

```bash
# Mejor hacer merge seguro que rebase mal hecho
# Practica rebase en ramas de prueba primero
```

5. **En merge commits complejos:**

```bash
# Rebase de merge commits puede ser muy complicado
# A veces es mejor dejar el merge commit
```

**Casos donde SÍ usar rebase:**

✅ Rama feature local antes de crear PR
✅ Actualizar rama feature con main
✅ Limpiar historia antes de merge
✅ Ramas de experimentación personal

---

### 3. ¿Qué significa "nunca hagas rebase de commits públicos"?

**Explicación:**

Cuando haces rebase, Git **reescribe la historia** cambiando los hashes de los commits. Si otros desarrolladores tienen esos commits, sus repositorios quedarán en estado inconsistente.

**Ejemplo del problema:**

```bash
# === Desarrollador A ===
git checkout -b feature/login
git commit -m "feat: add login"  # Hash: abc1234
git push origin feature/login

# === Desarrollador B ===
git checkout feature/login        # Tiene commit abc1234
git checkout -b feature/login-styles
# Trabaja basándose en abc1234

# === Desarrollador A (continúa) ===
git rebase main                   # Commit abc1234 → nuevo hash xyz9876
git push --force origin feature/login

# === Desarrollador B (intenta actualizar) ===
git pull
# ERROR: Historia divergente
# Git no sabe cómo reconciliar abc1234 con xyz9876
```

**Consecuencias:**

1. **Commits duplicados:**

```bash
# B ve ambos: el commit original y el rebaseado
git log --oneline
# xyz9876 feat: add login (nuevo)
# abc1234 feat: add login (viejo)
```

2. **Merge hell:**

```bash
# B intenta hacer merge y obtiene conflictos innecesarios
```

3. **Pérdida de trabajo:**

```bash
# Si B hizo commits basándose en abc1234, su trabajo puede perderse
```

**Solución correcta:**

```bash
# === Desarrollador A ===
# Si la rama es compartida, usa merge en lugar de rebase
git checkout feature/login
git merge main
git push origin feature/login  # Sin --force

# === O coordinar con el equipo ===
# 1. Avisar al equipo
# 2. Todos guardan trabajo: git push
# 3. A hace rebase y force push
# 4. Todos hacen: git fetch && git reset --hard origin/feature/login
```

**Analogía:**
Imagina que reescribes un libro después de que otros ya tienen copias. Sus copias no coincidirán con la nueva versión, causando confusión.

---

## Comandos Avanzados de Rebase

### Rebase con autosquash

```bash
# Hacer commit con prefijo fixup! o squash!
git commit -m "feat: agregar login"
git commit -m "fixup! feat: agregar login"  # Se combinará automáticamente

# Rebase con autosquash
git rebase -i --autosquash HEAD~5
# Los commits fixup! se ordenan y marcan automáticamente
```

**Configurar autosquash por defecto:**

```bash
git config --global rebase.autosquash true
```

---

### Rebase con estrategia específica

```bash
# Usar estrategia "ours" en conflictos
git rebase -X ours main

# Usar estrategia "theirs"
git rebase -X theirs main

# Mostrar más contexto en conflictos
git rebase -X patience main
```

---

### Rebase sin cambiar fecha de commits

```bash
# Preservar fecha de autor original
git rebase --committer-date-is-author-date main

# O configurar para siempre
git config --global rebase.instructionFormat '%s%nexec GIT_COMMITTER_DATE="%ai" git commit --amend --no-edit --date="%ai"'
```

---

### Rebase exec

```bash
# Ejecutar comando después de cada commit
git rebase -i HEAD~5 --exec "npm test"

# En el editor verás:
pick abc1234 commit 1
exec npm test
pick def5678 commit 2
exec npm test
# Si algún test falla, el rebase se detiene
```

---

## Workflows Prácticos

### Workflow 1: Limpiar historia antes de PR

```bash
# Tienes 15 commits sucios en feature branch
git log --oneline
# ... muchos WIP, fix typo, oops, etc ...

# Limpiar antes de crear PR
git rebase -i HEAD~15

# En el editor:
pick abc1234 feat: implement user login
squash def5678 add form
squash ghi9012 fix typo
squash jkl3456 WIP
pick mno7890 feat: add validation
fixup pqr1234 fix validation bug
pick stu5678 test: add login tests
squash vwx9012 add more tests

# Resultado: 3 commits limpios
# - feat: implement user login
# - feat: add validation
# - test: add login tests
```

---

### Workflow 2: Actualizar feature branch

```bash
# Main ha avanzado mientras trabajabas
git checkout main
git pull

git checkout feature/payment
git rebase main

# Si hay conflictos:
# ... resolver ...
git add .
git rebase --continue

# Actualizar en remoto
git push --force-with-lease origin feature/payment
```

**Nota:** `--force-with-lease` es más seguro que `--force`:

```bash
# Falla si alguien más pusheó cambios
git push --force-with-lease

# vs forzar sin importar qué
git push --force  # Peligroso
```

---

### Workflow 3: Separar feature en commits lógicos

```bash
# Tienes un commit gigante
git rebase -i HEAD~1
# Cambiar 'pick' a 'edit'

# Deshacer commit
git reset HEAD~1

# Agregar cambios por partes
git add -p  # Seleccionar interactivamente
git commit -m "feat: add user model"

git add -p
git commit -m "feat: add user controller"

git add -p
git commit -m "test: add user tests"

# Continuar
git rebase --continue
```

---

## Casos de Uso Reales

### Caso 1: Equipo usa Squash Merge en GitHub

```bash
# GitHub hará squash al mergear PR
# Pero quieres historia limpia localmente para desarrollo

# Limpiar commits antes de push
git rebase -i origin/main

# GitHub hará squash automáticamente al mergear
# Resultado: Un commit en main, historia limpia en tu rama
```

---

### Caso 2: Rebase largo con conflictos

```bash
git rebase main
# CONFLICT in file1.txt

# Resolver y continuar
git add file1.txt
git rebase --continue

# CONFLICT in file2.txt
# ... resolver ...
git rebase --continue

# Demasiados conflictos, mejor abortar
git rebase --abort

# Intentar con merge en su lugar
git merge main
```

---

### Caso 3: Reorganizar commits por tipo

```bash
# Tienes commits mezclados
git log --oneline
# abc1234 test: add tests
# def5678 feat: add feature
# ghi9012 docs: update readme
# jkl3456 feat: add another feature
# mno7890 test: more tests

# Reordenar agrupando por tipo
git rebase -i HEAD~5

# Reordenar en editor:
pick def5678 feat: add feature
pick jkl3456 feat: add another feature
pick abc1234 test: add tests
pick mno7890 test: more tests
pick ghi9012 docs: update readme

# Resultado: commits organizados por categoría
```

---

## Herramientas y Aliases

### Aliases útiles

```bash
# Rebase interactivo de N commits
git config --global alias.rb 'rebase -i HEAD~'

# Uso: git rb 5

# Rebase con autosquash
git config --global alias.rba 'rebase -i --autosquash'

# Continuar rebase
git config --global alias.rbc 'rebase --continue'

# Abortar rebase
git config --global alias.rba 'rebase --abort'

# Ver commits no en main
git config --global alias.new 'log main..HEAD --oneline'
```

---

### Script para limpiar rama automáticamente

```bash
#!/bin/bash
# clean-branch.sh

# Cuenta commits únicos en esta rama
COMMITS=$(git log main..HEAD --oneline | wc -l)

if [ $COMMITS -gt 0 ]; then
    echo "Encontrados $COMMITS commits para limpiar"

    # Rebase interactivo automático
    git rebase -i HEAD~$COMMITS
else
    echo "No hay commits para limpiar"
fi
```

---

## Errores Comunes y Soluciones

### Error: "Cannot rebase: You have unstaged changes"

```bash
# Problema: Tienes cambios sin commitear

# Solución 1: Stash
git stash
git rebase main
git stash pop

# Solución 2: Commit temporal
git add .
git commit -m "WIP: temp commit"
git rebase main
git reset HEAD~1  # Deshacer commit temporal

# Solución 3: Commit y luego squash
git add .
git commit -m "WIP"
git rebase main
git rebase -i HEAD~2  # Squash el WIP
```

---

### Error: "Cannot rebase: Multiple branches point to the same commit"

```bash
# Problema: Otras ramas apuntan al mismo commit

# Ver qué ramas
git branch --contains HEAD

# Solución: Mover o eliminar ramas conflictivas
git branch -d rama-vieja
# Luego rebase
```

---

### Error: Conflictos en cada commit durante rebase

```bash
# Problema: Rebase muy largo con muchos conflictos

# Solución 1: Abortar y usar merge
git rebase --abort
git merge main

# Solución 2: Rebase en pasos más pequeños
git rebase HEAD~5
# Resolver conflictos
git rebase HEAD~5  # Siguiente lote
# Etc.

# Solución 3: Usar rerere
git config --global rerere.enabled true
# Git recordará resoluciones de conflictos
```

---

### Error: Perdí commits después de rebase

```bash
# No te preocupes, están en reflog
git reflog
# Encuentra el hash antes del rebase
git reset --hard abc1234

# O crear rama desde ese punto
git branch backup-branch abc1234
```

---

## Mejores Prácticas

### ✅ Hacer

1. **Rebase antes de crear PR:**

```bash
git fetch origin
git rebase origin/main
git push --force-with-lease origin feature/branch
```

2. **Commits atómicos:**

```bash
# Un commit = una idea lógica
git commit -m "feat: add login form"
git commit -m "style: style login form"
git commit -m "test: add login tests"
```

3. **Mensajes descriptivos:**

```bash
# Después de squash, escribir mensaje completo
feat: implement user authentication

- Add login/logout functionality
- Implement JWT tokens
- Add password hashing
- Create auth middleware
```

4. **Probar después de rebase:**

```bash
git rebase main
npm test  # Verificar que todo funciona
git push --force-with-lease
```

### ❌ Evitar

1. **Rebase de main o ramas compartidas**
2. **Force push sin --force-with-lease**
3. **Rebase sin entender qué haces**
4. **Squash de commits que ya están en remoto compartido**

---

## Checklist de Dominio

Al completar este ejercicio deberías poder:

- [ ] Iniciar rebase interactivo
- [ ] Usar pick, squash, fixup, reword, edit, drop
- [ ] Reordenar commits
- [ ] Combinar commits relacionados
- [ ] Dividir commits grandes
- [ ] Editar commits específicos
- [ ] Resolver conflictos durante rebase
- [ ] Abortar rebase cuando sea necesario
- [ ] Rebase sobre otra rama
- [ ] Entender cuándo NO usar rebase
- [ ] Usar autosquash con fixup! commits
- [ ] Limpiar historia antes de PR

---

## Próximos Pasos

Has completado el Ejercicio 6. Ahora dominas:

- ✅ Rebase interactivo completo
- ✅ Limpieza de historia de commits
- ✅ Cuándo usar merge vs rebase
- ✅ Resolución de conflictos en rebase

**Continúa con:** [Ejercicio 7 - Cherry Pick y Workflows →](../exercise-07.md)

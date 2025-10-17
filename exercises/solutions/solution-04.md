# Solución - Ejercicio 4: Deshacer Cambios

## Comandos Completos

### 1. Preparación

```bash
git checkout main
git status
```

**Salida esperada:**

```
On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

---

### 2. Deshacer cambios en working directory

```bash
# Hacer cambio (NO agregarlo con git add)
echo "<h1>ESTO ES UN ERROR</h1>" >> index.html

# Ver estado
git status
# modified:   index.html

# Ver el cambio
git diff index.html

# Deshacer el cambio
git restore index.html
# O en versiones antiguas de Git:
git checkout -- index.html

# Verificar
git status  # Should be clean
cat index.html  # Error should be gone
```

**Explicación:**

- `git restore` recupera el archivo del último commit
- Los cambios se pierden permanentemente
- Solo funciona en archivos modificados, no en staged

---

### 3. Deshacer cambios en staging area

```bash
# Modificar archivo
echo "<p>Cambio temporal</p>" >> about.html

# Agregar a staging
git add about.html

# Ver estado
git status
# Changes to be committed:
#   modified:   about.html

# Quitar del staging (mantener cambios en working directory)
git restore --staged about.html
# O en versiones antiguas:
git reset HEAD about.html

# Verificar
git status
# Changes not staged for commit:
#   modified:   about.html

# El archivo aún tiene los cambios
cat about.html
```

**Respuesta:** Comandos usados:

- `git restore --staged <archivo>`
- `git reset HEAD <archivo>` (versión antigua)

---

### 4. Hacer commits para practicar

```bash
echo "<!-- Commit 1 -->" >> index.html
git add . && git commit -m "commit 1"

echo "<!-- Commit 2 -->" >> index.html
git add . && git commit -m "commit 2"

echo "<!-- Commit 3 -->" >> index.html
git add . && git commit -m "commit 3"
```

---

### 5. Ver historial

```bash
git log --oneline
```

**Salida esperada:**

```
e3f4a5b commit 3
d2c3b4a commit 2
c1b2a3c commit 1
a0b1c2d feat: agregar footer
...
```

---

### 6. Reset suave

```bash
# Deshaz último commit, mantén cambios en staging
git reset --soft HEAD~1

# Verificar estado
git status
# Changes to be committed:
#   modified:   index.html

# Ver historial
git log --oneline  # Ya no existe "commit 3"
```

**Respuesta:** Los cambios están en el staging area, listos para commit.

**Diagrama:**

```
                    Working    Staging    Repository
                    Directory  Area
reset --soft        ✗          ✓          ✓ (mueve HEAD)
```

---

### 7. Reset mixto (default)

```bash
# Hacer commit nuevamente
git commit -m "commit 3"

# Reset mixto (default)
git reset HEAD~1
# Equivale a: git reset --mixed HEAD~1

# Verificar
git status
# Changes not staged for commit:
#   modified:   index.html
```

**Respuesta - Diferencia con --soft:**

- `--soft`: Cambios en staging area
- `--mixed` (default): Cambios en working directory
- Ambos mantienen los cambios, solo difieren en dónde

**Diagrama:**

```
                    Working    Staging    Repository
                    Directory  Area
reset --mixed       ✓          ✗          ✓ (mueve HEAD)
```

---

### 8. Reset duro (CUIDADO)

```bash
# Hacer un commit nuevo
echo "Temporal" >> index.html
git add index.html
git commit -m "commit temporal"

# ⚠️ ADVERTENCIA: Esto BORRA cambios permanentemente
git reset --hard HEAD~1

# Verificar
git status  # Clean
git log --oneline  # Commit desapareció
cat index.html  # Cambios desaparecieron
```

**Diagrama:**

```
                    Working    Staging    Repository
                    Directory  Area
reset --hard        ✗          ✗          ✓ (mueve HEAD)
                    (BORRA!)   (BORRA!)
```

---

### 9. Revertir un commit (método seguro)

```bash
# Crear commit con error
echo "<h1>ERROR</h1>" > error.html
git add error.html
git commit -m "agregar archivo con error"

# Ver historial
git log --oneline
# abc1234 agregar archivo con error
# def5678 ...

# Revertir creando nuevo commit
git revert HEAD
# Se abrirá editor con mensaje por defecto
# Guardar y cerrar

# O con mensaje directo
git revert HEAD --no-edit

# Verificar
git log --oneline
# xyz9876 Revert "agregar archivo con error"
# abc1234 agregar archivo con error
# def5678 ...

ls  # error.html ya no existe
```

**Diferencia con reset:**

- `reset`: Mueve HEAD hacia atrás (reescribe historia)
- `revert`: Crea nuevo commit que deshace cambios (mantiene historia)

**Cuándo usar cada uno:**

- `revert`: En commits ya pusheados (públicos)
- `reset`: En commits locales no compartidos

---

### 10. Recuperar commits perdidos

```bash
# Ver el reflog (registro de todos los movimientos de HEAD)
git reflog
```

**Salida esperada:**

```
abc1234 HEAD@{0}: revert: Revert "agregar archivo con error"
def5678 HEAD@{1}: commit: agregar archivo con error
ghi9012 HEAD@{2}: reset: moving to HEAD~1
jkl3456 HEAD@{3}: commit: commit temporal
...
```

**Recuperar commit perdido:**

```bash
# Encontrar el hash del commit que quieres recuperar
git reflog | grep "commit temporal"
# jkl3456 HEAD@{3}: commit: commit temporal

# Recuperar ese commit
git reset --hard jkl3456
# O usar la notación HEAD@{n}
git reset --hard HEAD@{3}

# Verificar
git log --oneline  # El commit está de vuelta
```

**Nota:** Reflog guarda entradas por ~90 días (configurable).

---

## Verificación Final

```bash
# Ver historial completo
git log --oneline --all

# Ver reflog
git reflog

# Estado limpio
git status
```

---

## Respuestas a Preguntas de Reflexión

### 1. ¿Cuándo usarías git reset vs git revert?

**Git Reset:**

```bash
# ✅ Usar cuando:
- Commits son locales (no pusheados)
- Quieres limpiar historia antes de push
- Estás solo en la rama
- Cometiste error en commits recientes

# Ejemplo:
git reset --soft HEAD~3  # Combinar últimos 3 commits
# ... editar ...
git commit -m "feat: feature completa"
```

**Git Revert:**

```bash
# ✅ Usar cuando:
- Commits ya están en remoto (pusheados)
- Otros desarrolladores tienen esos commits
- Quieres mantener historia completa
- Necesitas auditoría de cambios

# Ejemplo:
git revert abc1234  # Deshacer commit específico
git push  # Seguro compartirlo
```

**Comparación:**

```
Situación                          Reset    Revert
─────────────────────────────────────────────────
Commits locales                    ✅       ✅
Commits pusheados                  ❌       ✅
Trabajo en equipo                  ❌       ✅
Reescribir historia                ✅       ❌
Mantener auditoría completa        ❌       ✅
```

---

### 2. ¿Por qué git reset --hard es peligroso?

**Razones:**

1. **Pérdida permanente de datos:**

```bash
# Cambios no commiteados se pierden para siempre
echo "Trabajo importante" > file.txt
git reset --hard HEAD
# file.txt desaparece - NO hay forma de recuperarlo
```

2. **No hay "deshacer" para working directory:**

```bash
# Commits se pueden recuperar con reflog
git reflog  # ✅ Commits recuperables

# Cambios no commiteados NO están en reflog
# ❌ Perdidos permanentemente
```

3. **Puede afectar a colaboradores:**

```bash
# Si haces reset de commits pusheados
git reset --hard HEAD~5
git push --force  # ⚠️ Otros desarrolladores tendrán problemas
```

**Alternativas más seguras:**

```bash
# En lugar de reset --hard, usa:
git stash              # Guardar cambios temporalmente
git restore <file>     # Deshacer archivo específico
git revert <commit>    # Deshacer commit de forma segura
```

---

### 3. ¿Qué es reflog y para qué sirve?

**Definición:**
Reflog (reference log) es un registro local de todos los movimientos de HEAD y ramas. Es tu "red de seguridad" en Git.

**Qué registra:**

- Commits
- Checkouts
- Merges
- Resets
- Rebases
- Cualquier operación que mueva HEAD

**Comandos útiles:**

```bash
# Ver reflog de HEAD
git reflog

# Ver reflog de rama específica
git reflog show main

# Ver reflog con más detalle
git reflog show --all

# Filtrar por operación
git reflog | grep "commit"
git reflog | grep "reset"

# Ver últimas N entradas
git reflog -n 10
```

**Casos de uso:**

1. **Recuperar commits perdidos:**

```bash
git reset --hard HEAD~5  # Oops, perdí commits
git reflog  # Encontrar hash perdido
git reset --hard abc1234  # Recuperar
```

2. **Recuperar después de rebase malo:**

```bash
git rebase -i HEAD~10  # Algo salió mal
git reflog  # Encontrar estado antes del rebase
git reset --hard HEAD@{5}  # Volver atrás
```

3. **Ver historial de trabajo:**

```bash
git reflog  # Ver qué hiciste hoy
```

**Limitaciones:**

- Solo es local (no se pushea)
- Expira después de ~90 días
- No guarda cambios no commiteados

---

## Comandos Adicionales

### Deshacer el último commit (mantener cambios)

```bash
# Método 1: Reset soft
git reset --soft HEAD~1

# Método 2: Reset mixed (default)
git reset HEAD~1

# Ambos mantienen cambios, difieren en staging
```

---

### Deshacer múltiples commits

```bash
# Últimos 3 commits
git reset --soft HEAD~3

# Hasta commit específico
git reset --soft abc1234

# Ver qué commits se van a deshacer
git log HEAD~3..HEAD
```

---

### Modificar el último commit

```bash
# Cambiar mensaje del último commit
git commit --amend -m "nuevo mensaje"

# Agregar archivos olvidados al último commit
git add archivo-olvidado.txt
git commit --amend --no-edit

# Cambiar mensaje y archivos
git add otros-archivos.txt
git commit --amend -m "mensaje actualizado"
```

---

### Revertir múltiples commits

```bash
# Revertir últimos 3 commits (crea 3 commits de revert)
git revert HEAD~2..HEAD

# Revertir sin crear commits inmediatamente
git revert --no-commit HEAD~2..HEAD
git commit -m "revert: deshacer últimos 3 commits"

# Revertir merge commit
git revert -m 1 <merge-commit-hash>
```

---

### Deshacer cambios en archivos específicos

```bash
# Restaurar archivo del último commit
git restore archivo.txt

# Restaurar de commit específico
git restore --source=abc1234 archivo.txt

# Restaurar todos los archivos
git restore .

# Sacar archivo del staging
git restore --staged archivo.txt
```

---

### Interactive reset

```bash
# Deshacer commits pero mantener algunos cambios
git reset HEAD~5  # Deshace 5 commits

# Seleccionar qué cambios volver a commitear
git add -p  # Agregar interactivamente
git commit -m "commit limpio"
```

---

## Tabla Resumen de Comandos

| Comando            | Working Dir | Staging  | Repository      | Uso                      |
| ------------------ | ----------- | -------- | --------------- | ------------------------ |
| `restore <file>`   | Restaura    | -        | -               | Deshacer cambios locales |
| `restore --staged` | -           | Quita    | -               | Quitar de staging        |
| `reset --soft`     | Mantiene    | Mantiene | Mueve HEAD      | Rehacer commits          |
| `reset --mixed`    | Mantiene    | Limpia   | Mueve HEAD      | Deshacer add             |
| `reset --hard`     | BORRA       | BORRA    | Mueve HEAD      | ⚠️ Peligroso             |
| `revert`           | -           | -        | Nuevo commit    | Deshacer públicamente    |
| `commit --amend`   | -           | Incluye  | Modifica último | Corregir último commit   |

---

## Escenarios Comunes

### Scenario 1: Cometí en rama equivocada

```bash
# Estás en main pero querías commitear en feature
git log -1  # Copiar hash del commit: abc1234

# Mover commit a la rama correcta
git checkout feature
git cherry-pick abc1234

# Volver a main y quitar el commit
git checkout main
git reset --hard HEAD~1
```

---

### Scenario 2: Quiero combinar últimos 3 commits

```bash
# Opción 1: Reset soft
git reset --soft HEAD~3
git commit -m "feat: feature completa"

# Opción 2: Rebase interactivo (más control)
git rebase -i HEAD~3
# Cambiar 'pick' a 'squash' para combinar
```

---

### Scenario 3: Commitee información sensible

```bash
# ⚠️ SI NO HAS PUSHEADO:
git reset --hard HEAD~1
# Recrear commit sin información sensible

# ⚠️ SI YA PUSHEASTE:
# 1. Remover archivo sensible
git rm archivo-sensible.txt
git commit -m "remove: eliminar archivo sensible"
git push

# 2. Considerar usar git-filter-repo o BFG
# 3. Notificar al equipo
# 4. Rotar credenciales expuestas
```

---

### Scenario 4: Merge salió mal

```bash
# Durante el merge
git merge --abort  # Abortar y volver al estado anterior

# Después del merge
git reset --hard HEAD~1  # Si no has pusheado
git revert -m 1 HEAD     # Si ya pusheaste
```

---

## Buenas Prácticas

### Antes de reset --hard

```bash
# Crear respaldo temporal
git branch backup-$(date +%Y%m%d-%H%M%S)
git reset --hard HEAD~5
# Si algo sale mal: git checkout backup-20251017-143000
```

### Usar reflog regularmente

```bash
# Ver qué hiciste hoy
git reflog | head -20

# Crear alias
git config --global alias.undo '!git reflog show'
```

### Commits atómicos

```bash
# Hacer commits pequeños y frecuentes
# Es más fácil revertir cambios específicos

# ✅ Bueno:
git commit -m "feat: agregar login form"
git commit -m "style: aplicar estilos al form"
git commit -m "test: agregar tests del form"

# ❌ Malo:
git commit -m "completar todo el login"
```

---

## Próximos Pasos

Has completado el Ejercicio 4. Ahora dominas:

- ✅ Diferentes formas de deshacer cambios
- ✅ Cuándo usar reset vs revert
- ✅ Cómo recuperar commits perdidos
- ✅ Usar reflog como red de seguridad

**Continúa con:** [Ejercicio 5 - Stash y Tags →](../exercise-05.md)

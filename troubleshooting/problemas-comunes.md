# Problemas Comunes de Git

Soluciones a los problemas más frecuentes al trabajar con Git.

## Tabla de Contenidos

1. [Problemas de Configuración](#problemas-de-configuración)
2. [Problemas con Ramas](#problemas-con-ramas)
3. [Problemas con Commits](#problemas-con-commits)
4. [Problemas con Push/Pull](#problemas-con-pushpull)
5. [Problemas de Permisos](#problemas-de-permisos)
6. [Problemas de Historial](#problemas-de-historial)

---

## Problemas de Configuración

### "fatal: not a git repository"

**Síntoma:** Git no reconoce el directorio como repositorio.

**Causa:** No estás en un repositorio Git o no hay `.git/` en el directorio.

**Solución:**

```bash
# Ver si estás en un repositorio
ls -la .git

# Si no existe, inicializar
git init

# O clonar uno existente
git clone https://github.com/usuario/repo.git
```

---

### "Author identity unknown"

**Síntoma:** Git no sabe quién eres al hacer commit.

```
*** Please tell me who you are.
Run
  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
```

**Solución:**

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu.email@ejemplo.com"

# Verificar
git config --global --list
```

---

### "warning: LF will be replaced by CRLF"

**Síntoma:** Advertencia sobre finales de línea en Windows.

**Causa:** Diferencias entre Windows (CRLF) y Unix (LF).

**Solución (Windows):**

```bash
# Configurar para convertir automáticamente
git config --global core.autocrlf true
```

**Solución (macOS/Linux):**

```bash
git config --global core.autocrlf input
```

---

## Problemas con Ramas

### "error: pathspec 'rama' did not match any file(s)"

**Síntoma:** La rama que intentas usar no existe.

**Solución:**

```bash
# Ver ramas disponibles
git branch -a

# Crear la rama si no existe
git checkout -b nombre-rama

# O traer rama remota
git fetch origin
git checkout nombre-rama
```

---

### "error: cannot delete branch 'main'"

**Síntoma:** No puedes eliminar la rama en la que estás.

**Solución:**

```bash
# Cambiar a otra rama primero
git checkout otra-rama

# Luego eliminar
git branch -d main
```

---

### "Your branch and 'origin/main' have diverged"

**Síntoma:** Tu rama local y la remota tienen historiales diferentes.

```
Your branch and 'origin/main' have diverged,
and have 3 and 2 different commits each, respectively.
```

**Solución 1: Merge (preserva historial)**

```bash
git pull origin main
# Resolver conflictos si hay
git push
```

**Solución 2: Rebase (historial limpio)**

```bash
git pull --rebase origin main
# Resolver conflictos si hay
git push
```

---

### "fatal: refusing to merge unrelated histories"

**Síntoma:** Intentas fusionar repositorios sin historial común.

**Solución:**

```bash
# Forzar merge
git pull origin main --allow-unrelated-histories

# O al clonar
git merge origin/main --allow-unrelated-histories
```

---

## Problemas con Commits

### "nothing to commit, working tree clean"

**Síntoma:** No hay cambios para commitear.

**Causa:** Ya hiciste commit de todos los cambios o no hay cambios.

**Verificación:**

```bash
# Ver estado
git status

# Ver diferencias
git diff
```

---

### "Please enter the commit message"

**Síntoma:** Se abre un editor de texto pidiendo mensaje.

**Causa:** Hiciste `git commit` sin `-m`.

**Solución 1: Escribir mensaje**

- Escribe el mensaje en el editor
- Guarda y cierra (en nano: Ctrl+X, Y, Enter; en vim: ESC, :wq)

**Solución 2: Cancelar**

```bash
# En nano: Ctrl+X, n
# En vim: ESC, :q!
```

**Prevención:**

```bash
# Siempre usa -m
git commit -m "Tu mensaje"
```

---

### "Changes not staged for commit"

**Síntoma:** Hiciste cambios pero no los agregaste al staging area.

**Solución:**

```bash
# Agregar archivos específicos
git add archivo.js

# Agregar todos
git add .

# Luego hacer commit
git commit -m "Mensaje"
```

---

### "Aborting commit due to empty commit message"

**Síntoma:** Commit cancelado por mensaje vacío.

**Solución:**

```bash
# Asegurar que escribes mensaje
git commit -m "Descripción del cambio"

# O permitir commit vacío (para testing)
git commit --allow-empty-message -m ""
```

---

## Problemas con Push/Pull

### "Updates were rejected because the remote contains work"

**Síntoma:** El remoto tiene commits que no tienes localmente.

```
! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/user/repo.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
```

**Solución:**

```bash
# 1. Traer cambios
git pull origin main

# 2. Resolver conflictos si hay
git add .
git commit -m "Resolver conflictos"

# 3. Hacer push
git push origin main
```

---

### "fatal: refusing to merge unrelated histories"

Ver sección [Problemas con Ramas](#fatal-refusing-to-merge-unrelated-histories).

---

### "! [remote rejected] (pre-receive hook declined)"

**Síntoma:** El servidor rechaza tu push por políticas del repositorio.

**Causas comunes:**

- No pasaste tests automáticos
- No tienes permisos
- Rama protegida requiere Pull Request

**Solución:**

```bash
# Verificar que tus tests pasen
npm test  # o el comando de tu proyecto

# Usar Pull Request en lugar de push directo
# (crear PR en GitHub/GitLab/Bitbucket)
```

---

### "error: failed to push some refs"

**Síntoma:** Push general falló.

**Causas comunes:**

1. No tienes permisos
2. Rama protegida
3. Necesitas hacer pull primero

**Solución:**

```bash
# Hacer pull primero
git pull --rebase origin main

# Luego push
git push origin main
```

---

## Problemas de Permisos

### "Permission denied (publickey)"

**Síntoma:** No puedes conectarte vía SSH.

```
Permission denied (publickey).
fatal: Could not read from remote repository.
```

**Solución:**

```bash
# 1. Verificar que tienes clave SSH
ls -la ~/.ssh/

# 2. Si no existe, generar
ssh-keygen -t ed25519 -C "tu.email@ejemplo.com"

# 3. Agregar al SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# 4. Copiar clave pública
cat ~/.ssh/id_ed25519.pub

# 5. Agregar a GitHub/GitLab/Bitbucket
# (en configuración de la plataforma)

# 6. Probar conexión
ssh -T git@github.com
```

**Más información:** [SSH Troubleshooting](/cheat-sheets/ssh-gpg-keys.md#ssh---troubleshooting)

---

### "fatal: Authentication failed"

**Síntoma:** Autenticación HTTPS falló.

**Causas comunes:**

- Contraseña incorrecta
- Token expirado
- 2FA activado sin token

**Solución:**

```bash
# Opción 1: Usar token en lugar de contraseña
# Generar token en GitHub/GitLab/Bitbucket
# Usar token como contraseña

# Opción 2: Cambiar a SSH
git remote set-url origin git@github.com:usuario/repo.git

# Opción 3: Configurar credentials helper
git config --global credential.helper manager
```

---

### "fatal: could not read Username"

**Síntoma:** Git no puede obtener credenciales.

**Solución:**

```bash
# Configurar credentials helper
git config --global credential.helper manager

# O proporcionar credenciales en URL (no recomendado)
git clone https://usuario:token@github.com/usuario/repo.git
```

---

## Problemas de Historial

### "detached HEAD state"

**Síntoma:** Estás en un commit específico, no en una rama.

```
You are in 'detached HEAD' state.
```

**Solución:**

```bash
# Si quieres volver a la rama
git checkout main

# Si quieres crear rama desde aquí
git checkout -b nombre-rama

# Si solo querías ver el commit
git checkout -
```

---

### "error: Your local changes would be overwritten"

**Síntoma:** Cambios locales se perderían al cambiar de rama.

```
error: Your local changes to the following files would be overwritten by checkout:
  archivo.js
Please commit your changes or stash them before you switch branches.
```

**Solución 1: Guardar cambios**

```bash
git add .
git commit -m "Cambios en progreso"
git checkout otra-rama
```

**Solución 2: Guardar temporalmente**

```bash
git stash
git checkout otra-rama
# Cuando vuelvas
git stash pop
```

**Solución 3: Descartar cambios**

```bash
git checkout -- .
git checkout otra-rama
```

---

### "warning: refname 'HEAD' is ambiguous"

**Síntoma:** Git está confundido sobre qué es HEAD.

**Causa:** Archivo o rama llamado HEAD.

**Solución:**

```bash
# Especificar claramente
git checkout main

# Ver referencias
git show-ref
```

---

## Tips Generales de Troubleshooting

### Ver mensajes de error completos

```bash
# Modo verbose
git --verbose comando

# O
GIT_TRACE=1 git comando
```

### Ver configuración actual

```bash
# Global
git config --global --list

# Local
git config --local --list

# Todo
git config --list --show-origin
```

### Verificar integridad del repositorio

```bash
# Verificar objetos
git fsck

# Limpiar y optimizar
git gc
```

### Deshacer casi cualquier cosa

```bash
# Ver historial de cambios de HEAD
git reflog

# Volver a un punto específico
git checkout abc123def
```

---

## ¿Aún tienes problemas?

- 📖 **Errores de Git:** [git-errors.md](git-errors.es.md)
- 🔐 **Problemas SSH:** [ssh-problems.md](ssh-problems.es.md)
- 🔀 **Conflictos de Merge:** [merge-conflicts.md](merge-conflicts.es.md)
- 📚 **Documentación:** [/docs/es](/docs/es)

---

**¿Encontraste la solución?** Comparte tu experiencia o reporta problemas nuevos.

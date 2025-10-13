# Git Básico - Cheat Sheet

Referencia rápida de los comandos más usados en Git para principiantes.

## Configuración Inicial

```bash
# Configurar nombre
git config --global user.name "Tu Nombre"

# Configurar email
git config --global user.email "tu.email@ejemplo.com"

# Ver configuración
git config --global --list
```

---

## Crear y Clonar Repositorios

```bash
# Crear nuevo repositorio local
git init

# Clonar repositorio remoto
git clone https://github.com/usuario/repo.git

# Clonar en carpeta específica
git clone https://github.com/usuario/repo.git nombre-carpeta
```

---

## Ver Estado y Cambios

```bash
# Ver estado actual
git status

# Ver estado corto
git status -s

# Ver cambios sin hacer commit
git diff

# Ver cambios en staging area
git diff --staged
```

---

## Agregar y Confirmar Cambios

```bash
# Agregar archivo específico
git add archivo.js

# Agregar todos los cambios
git add .

# Agregar cambios de un tipo específico
git add *.js

# Remover archivo del staging area
git restore --staged archivo.js

# Hacer commit
git commit -m "Descripción del cambio"

# Commit y agregar archivos modificados
git commit -a -m "Descripción"

# Enmendar último commit
git commit --amend -m "Nuevo mensaje"
```

---

## Ramas

```bash
# Ver ramas locales
git branch

# Ver todas las ramas
git branch -a

# Crear rama
git branch nombre-rama

# Crear y cambiar a rama
git checkout -b nombre-rama

# O versión más moderna
git switch -c nombre-rama

# Cambiar a rama
git checkout nombre-rama

# O versión moderna
git switch nombre-rama

# Cambiar a rama anterior
git checkout -

# Eliminar rama local
git branch -d nombre-rama

# Forzar eliminación
git branch -D nombre-rama

# Eliminar rama remota
git push origin --delete nombre-rama
```

---

## Guardar Cambios Temporalmente

```bash
# Guardar cambios sin hacer commit
git stash

# Ver lista de stashes
git stash list

# Recuperar últimos cambios guardados
git stash pop

# Recuperar cambios específicos
git stash pop stash@{0}

# Descartar stash
git stash drop
```

---

## Historial

```bash
# Ver historial completo
git log

# Ver últimos N commits
git log -n 5

# Ver en una línea
git log --oneline

# Ver con gráfico de ramas
git log --oneline --graph --all

# Ver commits de un archivo
git log -- archivo.js

# Ver commits de un autor
git log --author="nombre"

# Ver commits de últimas 24 horas
git log --since="24 hours ago"
```

---

## Push y Pull

```bash
# Enviar cambios al remoto
git push

# Enviar rama específica
git push origin nombre-rama

# Enviar todas las ramas
git push --all

# Traer cambios del remoto
git pull

# Traer cambios sin fusionar
git fetch

# Traer cambios de rama específica
git pull origin nombre-rama
```

---

## Merge (Fusionar)

```bash
# Fusionar rama en rama actual
git merge nombre-rama

# Ver ramas fusionadas
git branch --merged

# Ver ramas sin fusionar
git branch --no-merged

# Deshacer merge
git merge --abort
```

---

## Deshacer Cambios

```bash
# Deshacer cambios en archivo (antes de add)
git checkout -- archivo.js

# O versión moderna
git restore archivo.js

# Deshacer cambios en archivo (después de add)
git restore --staged archivo.js

# Volver a un commit anterior
git checkout abc123def

# Crear rama desde commit anterior
git checkout -b nombre-rama abc123def

# Revertir commit manteniendo historial
git revert abc123def

# Eliminar commits (cuidado)
git reset --hard abc123def
```

---

## Ver Información

```bash
# Ver detalles de commit
git show abc123def

# Ver solo mensaje del commit
git show --format=fuller abc123def

# Ver quién cambió cada línea
git blame archivo.js

# Ver remotes configurados
git remote -v

# Ver información del remoto
git remote show origin
```

---

## Alias Útiles

```bash
# Ver último commit
git config --global alias.last "log -1 HEAD"

# Ver log en una línea
git config --global alias.lo "log --oneline --graph --all"

# Ver estado corto
git config --global alias.st "status -s"

# Ver diferencias
git config --global alias.d "diff"

# Ver ramas
git config --global alias.br "branch -a"

# Agregar cambios
git config --global alias.ad "add"

# Commit
git config --global alias.cm "commit -m"

# Uso: git last, git lo, git st, etc.
```

---

## Combinaciones Útiles

```bash
# Ver qué va a cambiar antes de push
git diff main origin/main

# Ver cambios de tu rama vs main
git diff main..

# Actualizar rama con main sin cambiar commits
git pull --rebase origin main

# Ver log con mejor formato
git log --oneline --decorate --graph --all

# Hacer commit vacío (para testing)
git commit --allow-empty -m "Commit vacío"

# Ver commits que no están en main
git log main..
```

---

## Emergencias

```bash
# Recuperar commit eliminado (ver reflog)
git reflog

# Ir a commit específico
git checkout abc123def

# Descartar todos los cambios
git reset --hard

# Limpiar archivos sin rastrear
git clean -fd

# Ver cambios pendientes antes de hacer reset
git diff HEAD
```

---

## Tips Rápidos

✅ **Siempre actualiza antes de trabajar:**

```bash
git pull origin main
```

✅ **Revisa qué vas a enviar:**

```bash
git diff main..
```

✅ **Haz commits pequeños y frecuentes**

✅ **Escribe mensajes claros**

✅ **Nunca hagas push --force en ramas públicas**

✅ **Si algo sale mal, git reflog te salva**

---

**Para más información:** [Documentación Completa](/docs/es) | [Guías Detalladas](/guides/es)

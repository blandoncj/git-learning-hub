# Git Avanzado - Cheat Sheet

Referencia rápida de comandos avanzados para usuarios experimentados.

## Rebase Interactivo

```bash
# Rebase últimos 3 commits
git rebase -i HEAD~3

# Rebase sobre rama
git rebase -i main

# Opciones en editor:
# pick = usar commit
# reword = editar mensaje
# squash = fusionar con anterior
# fixup = como squash sin mensaje
# drop = eliminar
```

---

## Cherry-pick

```bash
# Aplicar commit específico a rama actual
git cherry-pick abc123def

# Cherry-pick con edición
git cherry-pick -e abc123def

# Cherry-pick múltiples commits
git cherry-pick abc123def ghi789jkl

# Cherry-pick rango
git cherry-pick abc123def..mno456pqr
```

---

## Reflog y Recuperación

```bash
# Ver historial de HEAD
git reflog

# Ver historial de rama específica
git reflog show nombre-rama

# Ir a punto específico en reflog
git checkout @{2}

# Recuperar rama eliminada
git checkout -b nombre-rama abc123def
```

---

## Stash Avanzado

```bash
# Guardar con mensaje
git stash save "Descripción"

# Guardar solo archivos rastreados
git stash push -m "Mensaje"

# Guardar cambios de archivo específico
git stash push archivo.js

# Aplicar stash sin eliminar
git stash apply

# Ver contenido de stash
git stash show -p stash@{0}

# Crear rama desde stash
git stash branch nombre-rama

# Limpiar todos los stashes
git stash clear
```

---

## Rebase

```bash
# Rebase sobre rama
git rebase main

# Rebase continuando después de conflicto
git rebase --continue

# Abortar rebase
git rebase --abort

# Rebase y fusionar con fast-forward
git rebase main && git merge --ff-only

# Rebase interactivo
git rebase -i main
```

---

## Reset y Restore

```bash
# Reset soft (mantiene cambios)
git reset --soft abc123def

# Reset mixed (por defecto)
git reset --mixed abc123def

# Reset hard (descarta todo)
git reset --hard abc123def

# Reset de archivo específico
git reset abc123def -- archivo.js

# Restore archivo
git restore archivo.js

# Restore desde commit específico
git restore -s abc123def archivo.js
```

---

## Clean

```bash
# Ver qué se limpiaría
git clean -fd --dry-run

# Limpiar archivos sin rastrear
git clean -fd

# Limpiar incluyendo directorios
git clean -fdx

# Limpiar interactivo
git clean -fdi
```

---

## Bisect - Encuentra el Bug

```bash
# Iniciar bisect
git bisect start

# Marcar commit actual como malo
git bisect bad

# Marcar commit antiguo como bueno
git bisect good abc123def

# Git te mostrará commits para probar
# Marca como good o bad
git bisect good  # o git bisect bad

# Cuando encuentres el bug
git bisect reset
```

---

## Filter-branch

```bash
# Cambiar nombre de autor globalmente
git filter-branch --env-filter '
if [ "$GIT_COMMITTER_NAME" = "Nombre Viejo" ]
then
  export GIT_COMMITTER_NAME="Nombre Nuevo"
  export GIT_AUTHOR_NAME="Nombre Nuevo"
fi' -- --all

# Remover archivo de historial
git filter-branch --tree-filter 'rm -f archivo.txt' HEAD
```

---

## Merge Avanzado

```bash
# Merge sin fast-forward
git merge --no-ff nombre-rama

# Squash merge
git merge --squash nombre-rama
git commit

# Merge con estrategia específica
git merge -s recursive -X theirs nombre-rama

# Merge con resolución automática
git merge -X ours nombre-rama
```

---

## Log Avanzado

```bash
# Ver cambios línea por línea
git log -p

# Ver estadísticas
git log --stat

# Ver solo archivos cambiados
git log --name-only

# Ver en formato personalizado
git log --pretty=format:"%h %an %ar %s"

# Ver commits que tocaron líneas específicas
git log -L 10,20:archivo.js

# Ver commits que agregaron/quitaron texto
git log -S "texto_específico"

# Ver commits con búsqueda regex
git log -G "pattern"

# Ver con árbol decorado
git log --decorate --graph --oneline
```

---

## Diff Avanzado

```bash
# Diff entre commits
git diff abc123d def456g

# Diff entre ramas
git diff main feature/rama

# Diff solo nombres de archivos
git diff --name-only

# Diff solo archivos cambiados
git diff --name-status

# Diff ignorando espacios
git diff -w

# Diff con contexto extendido
git diff -U10

# Diff de palabra por palabra
git diff --word-diff

# Diff de código específico
git diff -p archivo.js

# Diff con estadísticas
git diff --stat
```

---

## Blame Avanzado

```bash
# Blame ignorando espacios
git blame -w archivo.js

# Blame rango de líneas
git blame -L 10,50 archivo.js

# Blame con email
git blame -e archivo.js

# Blame detectando movimientos de líneas
git blame -M archivo.js

# Blame ignorando cambios de formato
git blame --ignore-rev abc123def
```

---

## Worktree

```bash
# Crear worktree en nueva rama
git worktree add ../path/a/worktree -b nombre-rama

# Listar worktrees
git worktree list

# Remover worktree
git worktree remove path/a/worktree

# Crear worktree en rama existente
git worktree add ../path/a/worktree nombre-rama-existente
```

---

## Submodules

```bash
# Agregar submodule
git submodule add https://github.com/user/repo.git path

# Clonar con submodules
git clone --recurse-submodules url

# Actualizar submodules
git submodule update --remote

# Inicializar submodules
git submodule init

# Remover submodule
git rm --cached path && rm -rf path
```

---

## Tags

```bash
# Crear tag ligero
git tag v1.0.0

# Crear tag anotado
git tag -a v1.0.0 -m "Versión 1.0.0"

# Listar tags
git tag -l

# Ver tag específico
git show v1.0.0

# Eliminar tag local
git tag -d v1.0.0

# Eliminar tag remoto
git push origin --delete v1.0.0

# Empujar tags
git push origin v1.0.0

# Empujar todos los tags
git push origin --tags
```

---

## Sparse Checkout

```bash
# Clonar solo parte del repo
git clone --sparse url

# Definir qué carpetas clonar
git sparse-checkout init
git sparse-checkout set carpeta1 carpeta2

# Ver configuración
git sparse-checkout list

# Desactivar sparse checkout
git sparse-checkout disable
```

---

## Patch

```bash
# Crear patch
git format-patch -1 abc123def

# Aplicar patch
git apply archivo.patch

# Aplicar patch con am (mantiene autor)
git am archivo.patch

# Ver patch antes de aplicar
git apply --check archivo.patch
```

---

## Shortcuts Avanzados

```bash
# Ver cambios no staged
git diff

# Ver cambios staged
git diff --staged

# Ver todas las diferencias
git diff HEAD

# Ver qué cambia entre ramas
git diff main...feature

# Ver commits en una rama pero no en otra
git log main..feature

# Ver commits comunes
git merge-base main feature

# Ver divergencia
git log --oneline --graph main feature

# Próximos cambios a syncear
git log @{u}..
```

---

## Performance

```bash
# Verificar integridad del repo
git fsck

# Limpiar y optimizar repo
git gc

# Limpiar agresivo
git gc --aggressive

# Ver tamaño del repo
du -sh .git

# Ver objetos grandes
git rev-list --all --objects | \
  sed -n $(git rev-list --objects --all | \
  cut -f1 -d' ' | \
  git cat-file --batch-check | \
  grep blob | \
  sort -k3 -n | \
  tail -10 | \
  while read hash type size; do \
    echo -n "-e s/$hash/$size/p "; \
  done) | \
  sort -k1 -n -r | \
  head -10
```

---

## Alias Avanzados

```bash
# Ver commits no fusionados
git config --global alias.unmerged \
  "branch --no-merged"

# Ver árbol completo
git config --global alias.tree \
  "log --oneline --graph --all --decorate"

# Estadísticas del repo
git config --global alias.stats \
  "shortlog -s -n"

# Ver cambios de un archivo
git config --global alias.changes \
  "log -p"

# Diff entre ramas
git config --global alias.diffb \
  "diff main..."

# Mostrar nombre de rama actual
git config --global alias.branch-name \
  "rev-parse --abbrev-ref HEAD"

# Push de rama actual
git config --global alias.pushc \
  "push -u origin HEAD"

# Sync automático
git config --global alias.sync \
  "pull --rebase && push"
```

---

**Para más información:** [Guías Avanzadas](/guides/es) | [Documentación Completa](/docs/es)

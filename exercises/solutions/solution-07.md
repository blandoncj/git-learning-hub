# Solución - Ejercicio 7: Cherry Pick y Git Workflows

## Comandos Completos

### 1. Preparación - Crear escenario

```bash
# Asegurarse de estar en main
git checkout main

# Crear rama de login
git checkout -b feature/login

# Volver a main
git checkout main

# Crear rama de dashboard
git checkout -b feature/dashboard
```

**Verificar:**

```bash
git branch
# * feature/dashboard
#   feature/login
#   main
```

---

### 2. Hacer commits en feature/login

```bash
# Cambiar a rama login
git checkout feature/login

# Commit 1: Formulario de login
echo "Login form" > login.html
git add login.html
git commit -m "feat: crear formulario de login"

# Commit 2: Validación
echo "Validation" >> login.html
git add login.html
git commit -m "feat: agregar validación de login"

# Commit 3: Servicio de autenticación
echo "Auth service" > auth.js
git add auth.js
git commit -m "feat: agregar servicio de autenticación"
```

**Ver el historial:**

```bash
git log --oneline
```

**Salida esperada:**

```
abc1234 feat: agregar servicio de autenticación
def5678 feat: agregar validación de login
ghi9012 feat: crear formulario de login
jkl3456 (main) último commit en main
```

---

### 3. Cherry-pick un commit específico

**Copiar el hash del commit "agregar servicio de autenticación":**

```bash
git log --oneline | grep "servicio de autenticación"
# abc1234 feat: agregar servicio de autenticación
```

**Cambiar a feature/dashboard:**

```bash
git checkout feature/dashboard
```

**Aplicar solo ese commit:**

```bash
git cherry-pick abc1234
```

**Salida esperada:**

```
[feature/dashboard xyz9876] feat: agregar servicio de autenticación
 Date: Fri Oct 17 10:00:00 2025 -0500
 1 file changed, 1 insertion(+)
 create mode 100644 auth.js
```

**Verificar:**

```bash
ls
# auth.js  (otros archivos de dashboard si los hubiera)

git log --oneline
# xyz9876 feat: agregar servicio de autenticación
# jkl3456 último commit en main
```

**Respuesta a la pregunta:**
Solo tienes `auth.js` en feature/dashboard. Los otros archivos (`login.html`) NO están porque solo aplicaste un commit específico, no todos los commits de la rama login.

---

### 4. Cherry-pick múltiples commits

**Volver a feature/login y crear más commits:**

```bash
git checkout feature/login

# Commit 4: Logout
echo "Logout" > logout.js
git add logout.js
git commit -m "feat: agregar logout"

# Commit 5: Session manager
echo "Session manager" > session.js
git add session.js
git commit -m "feat: agregar gestor de sesiones"
```

**Ver commits recientes:**

```bash
git log --oneline -2
# mno7890 feat: agregar gestor de sesiones
# pqr1234 feat: agregar logout
```

**Cherry-pick un rango de commits a dashboard:**

```bash
git checkout feature/dashboard

# Opción 1: Rango (no incluye el primero)
git cherry-pick pqr1234..mno7890

# Opción 2: Lista explícita (recomendado)
git cherry-pick pqr1234 mno7890
```

**Salida esperada:**

```
[feature/dashboard abc2345] feat: agregar logout
 1 file changed, 1 insertion(+)
 create mode 100644 logout.js
[feature/dashboard def6789] feat: agregar gestor de sesiones
 1 file changed, 1 insertion(+)
 create mode 100644 session.js
```

**Verificar:**

```bash
ls
# auth.js  logout.js  session.js

git log --oneline -3
# def6789 feat: agregar gestor de sesiones
# abc2345 feat: agregar logout
# xyz9876 feat: agregar servicio de autenticación
```

---

### 5. Resolver conflictos en cherry-pick

**Crear un conflicto:**

```bash
# En dashboard, modificar auth.js
git checkout feature/dashboard
echo "Dashboard version" > auth.js
git add auth.js
git commit -m "feat: modificar auth para dashboard"

# En login, modificar el mismo auth.js
git checkout feature/login
echo "Login version" > auth.js
git add auth.js
git commit -m "feat: modificar auth para login"
```

**Copiar hash del último commit en login:**

```bash
HASH=$(git log --oneline -1 | awk '{print $1}')
echo $HASH  # Por ejemplo: stu5678
```

**Intentar cherry-pick en dashboard:**

```bash
git checkout feature/dashboard
git cherry-pick stu5678
```

**Salida esperada (conflicto):**

```
error: could not apply stu5678... feat: modificar auth para login
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'
```

**Ver el conflicto:**

```bash
git status
# both modified:   auth.js

cat auth.js
```

**Contenido con marcadores:**

```javascript
<<<<<<< HEAD
Dashboard version
=======
Login version
>>>>>>> stu5678... feat: modificar auth para login
```

**Resolver manualmente:**

```bash
# Editar auth.js para combinar ambas versiones
cat > auth.js << 'EOF'
// Merged version
Dashboard version
Login version
EOF

# O elegir una versión:
# git checkout --ours auth.js    # Mantener dashboard
# git checkout --theirs auth.js  # Mantener login
```

**Completar el cherry-pick:**

```bash
git add auth.js
git cherry-pick --continue
```

**Se abrirá editor para mensaje de commit. Guardar y cerrar.**

**Salida:**

```
[feature/dashboard vwx9012] feat: modificar auth para login
 Date: Fri Oct 17 10:30:00 2025 -0500
 1 file changed, 2 insertions(+)
```

---

### 6. Abortar cherry-pick

**Si el conflicto es muy complicado:**

```bash
# Durante un cherry-pick con conflictos
git cherry-pick --abort
```

**Resultado:**

```
# Vuelve al estado antes del cherry-pick
# Sin cambios aplicados
```

**Cuándo usar:**

- Conflictos demasiado complejos
- Te das cuenta que no era el commit correcto
- Quieres intentar otra estrategia

---

### 7. Implementar Git Flow - Crear rama develop

```bash
# Volver a main limpio
git checkout main

# Crear rama develop desde main
git checkout -b develop
```

**Pushear develop (opcional):**

```bash
git push -u origin develop
```

**Estructura resultante:**

```
main      ← Producción
develop   ← Integración
```

---

### 8. Git Flow - Feature branches

```bash
# Asegurarse de estar en develop
git checkout develop

# Crear feature desde develop
git checkout -b feature/user-profile

# Desarrollo de la feature
echo "Profile page" > profile.html
git add profile.html
git commit -m "feat: agregar página de perfil"

echo "Edit profile" >> profile.html
git add profile.html
git commit -m "feat: permitir edición de perfil"

# Agregar más funcionalidad
echo "Avatar upload" >> profile.html
git add profile.html
git commit -m "feat: agregar subida de avatar"
```

**Ver progreso:**

```bash
git log --oneline -3
# abc1234 feat: agregar subida de avatar
# def5678 feat: permitir edición de perfil
# ghi9012 feat: agregar página de perfil
```

---

### 9. Git Flow - Finalizar feature

**Merge la feature en develop:**

```bash
# Volver a develop
git checkout develop

# Merge sin fast-forward
git merge --no-ff feature/user-profile
```

**Se abrirá editor con mensaje por defecto:**

```
Merge branch 'feature/user-profile' into develop
```

**Guardar y cerrar.**

**Salida esperada:**

```
Merge made by the 'recursive' strategy.
 profile.html | 3 +++
 1 file changed, 3 insertions(+)
 create mode 100644 profile.html
```

**Respuesta a la pregunta - ¿Qué hace `--no-ff`?**

`--no-ff` (no fast-forward) **siempre crea un commit de merge**, incluso cuando Git podría hacer fast-forward.

**Sin --no-ff (fast-forward):**

```
develop ─── A ─── B ─── C
                   ↑
              (feature commits se agregan linealmente)
```

**Con --no-ff:**

```
develop ─── A ─────────── M (merge commit)
             \           /
              B ─── C ─── (feature)
```

**Ventajas de --no-ff:**

- Preserva información de que existió una rama
- Agrupa commits relacionados
- Más fácil revertir feature completa: `git revert M`
- Historia más descriptiva

**Cuándo usar:**

```bash
# Features en develop
git merge --no-ff feature/nueva

# Hotfixes en main
git merge --no-ff hotfix/critico

# Releases
git merge --no-ff release/v1.0.0
```

**Eliminar la rama feature:**

```bash
git branch -d feature/user-profile
```

**Salida:**

```
Deleted branch feature/user-profile (was abc1234).
```

---

### 10. Git Flow - Crear release branch

```bash
# Desde develop, crear release
git checkout develop
git checkout -b release/1.0.0

# Hacer ajustes finales (version bumps, changelog, etc.)
echo "Version 1.0.0" > VERSION
git add VERSION
git commit -m "chore: preparar release 1.0.0"

# Actualizar changelog
cat > CHANGELOG.md << 'EOF'
# Changelog

## v1.0.0 (2025-10-17)

### Features
- Página de perfil de usuario
- Edición de perfil
- Subida de avatar

### Bug Fixes
- Ninguno

### Breaking Changes
- Ninguno
EOF

git add CHANGELOG.md
git commit -m "docs: actualizar changelog para v1.0.0"
```

---

### 11. Git Flow - Finalizar release

**Merge a main:**

```bash
git checkout main
git merge --no-ff release/1.0.0
```

**Mensaje de commit:**

```
Merge branch 'release/1.0.0'

Release version 1.0.0
```

**Crear tag:**

```bash
git tag -a v1.0.0 -m "Release version 1.0.0

Features:
- User profile page
- Profile editing
- Avatar upload"
```

**Merge de vuelta a develop:**

```bash
git checkout develop
git merge --no-ff release/1.0.0
```

**Mensaje:**

```
Merge branch 'release/1.0.0' into develop
```

**Eliminar rama release:**

```bash
git branch -d release/1.0.0
```

**Pushear todo:**

```bash
git push origin main develop --tags
```

---

### 12. Git Flow - Hotfix

**Crear hotfix desde main:**

```bash
git checkout main
git checkout -b hotfix/1.0.1

# Hacer fix crítico
echo "Critical fix" > hotfix.txt
git add hotfix.txt
git commit -m "fix: corregir bug crítico en producción"

# Actualizar VERSION
echo "Version 1.0.1" > VERSION
git add VERSION
git commit -m "chore: bump version to 1.0.1"
```

**Merge a main:**

```bash
git checkout main
git merge --no-ff hotfix/1.0.1
```

**Crear tag:**

```bash
git tag -a v1.0.1 -m "Hotfix 1.0.1

Fix:
- Corregir bug crítico en producción"
```

**Merge a develop:**

```bash
git checkout develop
git merge --no-ff hotfix/1.0.1
```

**Eliminar hotfix:**

```bash
git branch -d hotfix/1.0.1
```

**Push:**

```bash
git push origin main develop --tags
```

---

### 13. Visualizar el workflow

```bash
git log --all --graph --oneline --decorate
```

**Salida esperada (simplificada):**

```
*   Merge branch 'hotfix/1.0.1' into develop
|\
| *   Merge branch 'hotfix/1.0.1' (tag: v1.0.1)
| |\
| | * chore: bump version to 1.0.1
| | * fix: corregir bug crítico
| |/
* |   Merge branch 'release/1.0.0' into develop
|\|
| *   Merge branch 'release/1.0.0' (tag: v1.0.0)
| |\
| | * docs: actualizar changelog
| | * chore: preparar release 1.0.0
| |/
* |   Merge branch 'feature/user-profile' into develop
|\|
| * feat: agregar subida de avatar
| * feat: permitir edición de perfil
| * feat: agregar página de perfil
|/
* commit inicial
```

---

### 14. GitHub Flow (alternativa simple)

**Implementación de GitHub Flow:**

```bash
# 1. Todo parte de main
git checkout main
git pull origin main

# 2. Crear rama descriptiva para cualquier cambio
git checkout -b add-search-feature

# 3. Hacer commits
echo "Search feature" > search.js
git add search.js
git commit -m "feat: agregar funcionalidad de búsqueda"

echo "Search UI" > search.html
git add search.html
git commit -m "feat: agregar interfaz de búsqueda"

# 4. Push regularmente
git push -u origin add-search-feature

# 5. Abrir Pull Request en GitHub cuando esté listo
# (interfaz web)

# 6. Discusión y code review en el PR

# 7. Tests automáticos (CI/CD)
# (GitHub Actions, CircleCI, etc.)

# 8. Deploy a staging automáticamente desde PR
# (opcional)

# 9. Después de aprobación, merge a main
git checkout main
git merge add-search-feature
git push origin main

# 10. Deploy automático a producción
# (desde main)

# 11. Eliminar rama
git branch -d add-search-feature
git push origin --delete add-search-feature
```

**Respuesta a la pregunta - ¿Cuándo usar GitHub Flow vs Git Flow?**

**Usa GitHub Flow cuando:**

- Deployeas frecuentemente (varias veces al día)
- Tienes CI/CD robusto
- Equipo pequeño/mediano
- Productos web/SaaS
- Quieres simplicidad
- Solo hay una versión en producción

**Ejemplos:** Startups, aplicaciones web modernas, microservicios

**Usa Git Flow cuando:**

- Releases programados (mensual, trimestral)
- Múltiples versiones en producción
- Productos enterprise con soporte de versiones antiguas
- Aplicaciones móviles (App Store review)
- Software empaquetado
- Necesitas hotfixes para versiones antiguas

**Ejemplos:** Software empresarial, aplicaciones móviles, bibliotecas/frameworks

**Comparación:**

| Aspecto              | GitHub Flow  | Git Flow               |
| -------------------- | ------------ | ---------------------- |
| Ramas principales    | 1 (main)     | 2 (main, develop)      |
| Complejidad          | Simple       | Compleja               |
| Deploys              | Continuos    | Programados            |
| Ideal para           | Web apps     | Enterprise             |
| Curva de aprendizaje | Baja         | Alta                   |
| Hotfixes             | Rama de main | Rama hotfix específica |

---

## Verificación Final

```bash
# Ver todas las ramas
git branch -a
# * develop
#   main
#   remotes/origin/develop
#   remotes/origin/main

# Ver tags
git tag
# v1.0.0
# v1.0.1

# Ver historial completo
git log --all --graph --oneline --decorate -20

# Ver commits en develop pero no en main
git log main..develop --oneline

# Ver archivos en cada rama
git checkout main && ls
git checkout develop && ls
```

---

## Respuestas a Preguntas de Reflexión

### 1. ¿Cuándo es apropiado usar cherry-pick?

**Casos apropiados:**

1. **Hotfix urgente necesita ir a múltiples ramas:**

```bash
# Fix en release/1.0
git checkout release/1.0
git commit -m "fix: seguridad crítica"

# Aplicar mismo fix en release/2.0
git checkout release/2.0
git cherry-pick abc1234

# Y en develop
git checkout develop
git cherry-pick abc1234
```

2. **Recuperar commit de rama que se va a eliminar:**

```bash
# Rama experimental tiene 1 commit útil
git checkout experimental
# ... 10 commits malos, 1 bueno ...

git checkout main
git cherry-pick <hash-del-bueno>
git branch -D experimental
```

3. **Backporting de features a versiones antiguas:**

```bash
# Feature en v2.0, cliente paga por tenerlo en v1.5
git checkout release/v1.5
git cherry-pick <hash-feature>
```

4. **Commit en rama equivocada:**

```bash
# Commiteaste en main por error
git checkout feature/correcta
git cherry-pick abc1234

git checkout main
git reset --hard HEAD~1
```

**Cuándo NO usar cherry-pick:**

❌ Para sincronizar ramas regularmente (usa merge/rebase)
❌ Para múltiples commits relacionados (usa merge)
❌ Cuando puede causar conflictos complejos
❌ Como reemplazo de workflow apropiado

---

### 2. ¿Qué ventajas y desventajas tiene Git Flow?

**Ventajas:**

1. **Estructura clara:**

```
main ─── producción (siempre deployable)
develop ─── integración
feature/* ─── desarrollo
release/* ─── preparación
hotfix/* ─── emergencias
```

2. **Soporte de múltiples versiones:**

```bash
# Mantener v1.0, v2.0, v3.0 simultáneamente
git checkout release/v1.0
git cherry-pick <hotfix>
```

3. **Releases predecibles:**

```bash
# Proceso claro de release
develop → release/X.X.X → main (tag)
```

4. **Aislamiento de trabajo:**

- Features no afectan a develop hasta merge
- Develop no afecta a main hasta release

5. **Hotfixes limpios:**

```bash
# Proceso claro para emergencias
main → hotfix/X.X.X → main + develop
```

**Desventajas:**

1. **Complejidad:**

- Muchas ramas para gestionar
- Curva de aprendizaje alta
- Fácil cometer errores

2. **Overhead:**

```bash
# Muchos pasos para feature simple
develop → feature → develop → release → main
# vs GitHub Flow:
main → feature → main
```

3. **Merge hell:**

- Merges frecuentes entre ramas
- Conflictos complicados
- Historia compleja

4. **No apto para CD:**

- Deploy continuo es difícil
- Bottleneck en rama develop

5. **Requiere disciplina:**

- Equipo debe seguir proceso estrictamente
- Errores en workflow causan problemas grandes

---

### 3. ¿Por qué algunos equipos prefieren GitHub Flow?

**Razones principales:**

1. **Simplicidad extrema:**

```bash
# GitHub Flow
main → feature → main

# vs Git Flow
develop → feature → develop → release → main
```

2. **Deploy continuo:**

```bash
# Merge a main = deploy automático
git merge feature/nueva
# → GitHub Actions → Deploy a producción
```

3. **Feedback rápido:**

```bash
# Cambios en producción en minutos/horas
# No días/semanas esperando release
```

4. **Menos errores:**

```bash
# Menos ramas = menos confusión
# Solo main + feature branches
```

5. **Pull Requests centrados:**

```bash
# Todo el proceso en el PR:
# - Code review
# - Tests automáticos
# - Deploy a staging
# - Aprobación
# - Merge y deploy
```

6. **Mejor para SaaS:**

```bash
# Una versión en producción
# Rollback si hay problemas
# No necesitas mantener v1.0, v2.0, etc.
```

**Equipos que lo prefieren:**

- Startups (velocidad)
- Productos web (deploy continuo)
- Equipos pequeños (menos overhead)
- Cultura DevOps (automatización)

---

### 4. ¿Qué es trunk-based development?

**Definición:**
Todos los desarrolladores trabajan en una sola rama (trunk/main) con commits frecuentes y pequeños.

**Características:**

1. **Una rama principal:**

```bash
main
  ├── commit developer A
  ├── commit developer B
  ├── commit developer A
  ├── commit developer C
```

2. **Branches de vida corta (< 1 día):**

```bash
main → feature (2-4 horas) → main
```

3. **Feature flags:**

```javascript
if (featureFlags.newSearch) {
  return <NewSearch />;
} else {
  return <OldSearch />;
}
```

4. **Integración continua estricta:**

```bash
# Cada commit dispara:
# - Tests unitarios
# - Tests de integración
# - Tests E2E
# - Deploy a staging
```

**Ventajas:**

- Integración continua real
- Menos merge conflicts
- Feedback inmediato
- Simplicidad máxima

**Desventajas:**

- Requiere tests excelentes
- Requiere feature flags
- No apto para todo tipo de proyecto
- Necesita CI/CD robusto

**Comparación:**

| Aspecto                | Trunk-Based | Git Flow     | GitHub Flow         |
| ---------------------- | ----------- | ------------ | ------------------- |
| Ramas                  | 1 (trunk)   | 5 tipos      | 2 (main + features) |
| Vida de feature branch | Horas       | Días/Semanas | Días                |
| Integración            | Continua    | Por release  | Por PR              |
| Complejidad            | Baja        | Alta         | Media               |

---

## Comandos Avanzados

### Cherry-pick avanzado

```bash
# Cherry-pick sin hacer commit
git cherry-pick -n abc1234
# Editar antes de commitear
git commit

# Cherry-pick de merge commit
git cherry-pick -m 1 <merge-commit>

# Cherry-pick múltiples commits
git cherry-pick abc1234 def5678 ghi9012

# Cherry-pick conservando autor original
git cherry-pick -x abc1234
# Agrega "(cherry picked from commit abc1234)" al mensaje
```

---

### Git Flow helpers

```bash
# Instalar git-flow extensions
# Mac: brew install git-flow
# Linux: apt-get install git-flow

# Inicializar git-flow
git flow init

# Feature
git flow feature start nombre
git flow feature finish nombre

# Release
git flow release start 1.0.0
git flow release finish 1.0.0

# Hotfix
git flow hotfix start 1.0.1
git flow hotfix finish 1.0.1
```

---

## Workflows Híbridos

### GitHub Flow + Release Branches

```bash
# Normal: GitHub Flow
main → feature → main (deploy continuo)

# Para mobile releases
main → release/v2.0 → App Store
         ↓
      hotfix → release/v2.0 → main
```

---

### Git Flow Simplificado

```bash
# Solo usar:
main      # Producción
develop   # Integración
feature/* # Features

# Omitir release branches para equipos pequeños
# Hotfixes directos a main
```

---

## Casos de Uso Reales

### Caso 1: Empresa con múltiples clientes

```bash
# Cada cliente en su rama
main → client-A (v1.5 customizado)
    → client-B (v2.0 customizado)
    → client-C (v1.8 customizado)

# Cherry-pick fixes a todos
git checkout client-A
git cherry-pick <security-fix>

git checkout client-B
git cherry-pick <security-fix>
```

---

### Caso 2: Feature flags en GitHub Flow

```javascript
// config.js
export const features = {
  newCheckout: process.env.ENABLE_NEW_CHECKOUT === "true",
  darkMode: process.env.ENABLE_DARK_MODE === "true",
};

// Component.jsx
import { features } from "./config";

if (features.newCheckout) {
  return <NewCheckout />;
}
return <OldCheckout />;
```

```bash
# Deploy a producción con feature desactivada
ENABLE_NEW_CHECKOUT=false

# Cuando esté lista, activar
ENABLE_NEW_CHECKOUT=true
```

---

### Caso 3: Hotfix en múltiples versiones (Git Flow)

```bash
# Bug crítico en producción
git checkout main
git checkout -b hotfix/security-patch

# Fix
git commit -m "fix: CVE-2025-1234 security patch"

# Aplicar a main
git checkout main
git merge hotfix/security-patch
git tag v2.1.5

# Aplicar a versión anterior aún soportada
git checkout release/v2.0
git cherry-pick <hash-del-fix>
git tag v2.0.10

# Aplicar a develop
git checkout develop
git merge hotfix/security-patch

# Limpiar
git branch -d hotfix/security-patch
```

---

## Herramientas y Automatización

### GitHub Actions para Git Flow

```yaml
# .github/workflows/gitflow.yml
name: Git Flow

on:
  push:
    branches:
      - "release/**"

jobs:
  tag-release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Extract version
        id: version
        run: echo "::set-output name=version::${GITHUB_REF#refs/heads/release/}"

      - name: Create tag
        run: |
          git tag -a v${{ steps.version.outputs.version }} -m "Release ${{ steps.version.outputs.version }}"
          git push origin v${{ steps.version.outputs.version }}
```

---

### Script para automatizar Git Flow

```bash
#!/bin/bash
# gitflow-helper.sh

case $1 in
  "feature")
    git checkout develop
    git checkout -b feature/$2
    ;;

  "finish-feature")
    BRANCH=$(git branch --show-current)
    git checkout develop
    git merge --no-ff $BRANCH
    git branch -d $BRANCH
    ;;

  "release")
    git checkout develop
    git checkout -b release/$2
    echo $2 > VERSION
    git add VERSION
    git commit -m "chore: bump version to $2"
    ;;

  "finish-release")
    BRANCH=$(git branch --show-current)
    VERSION=${BRANCH#release/}

    git checkout main
    git merge --no-ff $BRANCH
    git tag -a v$VERSION -m "Release $VERSION"

    git checkout develop
    git merge --no-ff $BRANCH
    git branch -d $BRANCH
    ;;

  *)
    echo "Usage: $0 {feature|finish-feature|release|finish-release} [name|version]"
    ;;
esac
```

**Uso:**

```bash
chmod +x gitflow-helper.sh

./gitflow-helper.sh feature nueva-feature
./gitflow-helper.sh finish-feature

./gitflow-helper.sh release 1.2.0
./gitflow-helper.sh finish-release
```

---

## Mejores Prácticas

### Cherry-pick

✅ **Hacer:**

- Usar para fixes específicos en múltiples ramas
- Verificar que el commit aplique limpiamente
- Usar `-x` para referencia al commit original
- Testear después del cherry-pick

❌ **Evitar:**

- Cherry-pick de muchos commits relacionados
- Usar como reemplazo de merge
- Cherry-pick sin entender el contexto
- Aplicar commits con dependencias faltantes

### Git Flow

✅ **Hacer:**

- Documentar el proceso del equipo
- Automatizar lo posible
- Mantener develop actualizado
- Tags significativos en releases

❌ **Evitar:**

- Seguirlo si no lo necesitas
- Commits directos a main/develop
- Feature branches de larga duración
- Olvidar mergear hotfix a develop

### GitHub Flow

✅ **Hacer:**

- Branch names descriptivos
- PRs pequeños y frecuentes
- Deploy desde main automáticamente
- Tests robustos antes de merge

❌ **Evitar:**

- PRs gigantes
- Commits directos a main
- Mergear sin code review
- Deploy sin tests

---

## Checklist de Dominio

Al completar este ejercicio deberías poder:

- [ ] Usar cherry-pick para commits específicos
- [ ] Cherry-pick rangos de commits
- [ ] Resolver conflictos en cherry-pick
- [ ] Implementar Git Flow completo
- [ ] Crear y finalizar features
- [ ] Gestionar releases con Git Flow
- [ ] Crear hotfixes urgentes
- [ ] Implementar GitHub Flow
- [ ] Decidir qué workflow usar
- [ ] Entender trunk-based development
- [ ] Automatizar workflows con scripts

---

## Recursos Adicionales

- [Git Flow Cheat Sheet](https://danielkummer.github.io/git-flow-cheatsheet/)
- [GitHub Flow Guide](https://guides.github.com/introduction/flow/)
- [Trunk Based Development](https://trunkbaseddevelopment.com/)
- [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)

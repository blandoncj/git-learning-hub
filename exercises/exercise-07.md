# Ejercicio 7: Cherry Pick y Git Workflows

**Nivel:** Avanzado  
**Tiempo estimado:** 25 minutos  
**Objetivos:**

- Usar cherry-pick para commits específicos
- Entender diferentes workflows de Git
- Implementar Git Flow básico

---

## Contexto

Aprenderás a seleccionar commits específicos de una rama y aplicarlos en otra, además de implementar un workflow profesional.

---

## Tareas

### 1. Preparación - Crear escenario

Crea dos ramas de desarrollo:

```bash
git checkout main
git checkout -b feature/login
git checkout main
git checkout -b feature/dashboard
```

---

### 2. Hacer commits en feature/login

```bash
git checkout feature/login

echo "Login form" > login.html
git add login.html
git commit -m "feat: crear formulario de login"

echo "Validation" >> login.html
git add login.html
git commit -m "feat: agregar validación de login"

echo "Auth service" > auth.js
git add auth.js
git commit -m "feat: agregar servicio de autenticación"
```

Ver el historial:

```bash
git log --oneline
```

---

### 3. Cherry-pick un commit específico

Imagina que necesitas el servicio de autenticación en la rama dashboard.

Copia el hash del commit "agregar servicio de autenticación":

```bash
git log --oneline
# Copia el hash, ejemplo: abc1234
```

Cambia a feature/dashboard:

```bash
git checkout feature/dashboard
```

Aplica solo ese commit:

```bash
# Tu código aquí
```

**Pista:** `git cherry-pick <hash>`

Verifica:

```bash
git log --oneline
ls
```

**Pregunta:** ¿Qué archivos tienes ahora en feature/dashboard?

---

### 4. Cherry-pick múltiples commits

Vuelve a feature/login y crea más commits:

```bash
git checkout feature/login

echo "Logout" > logout.js
git add logout.js
git commit -m "feat: agregar logout"

echo "Session manager" > session.js
git add session.js
git commit -m "feat: agregar gestor de sesiones"
```

Cherry-pick un rango de commits a dashboard:

```bash
git checkout feature/dashboard
# Tu código aquí
```

**Pista:** `git cherry-pick <hash1>..<hash2>` o `git cherry-pick <hash1> <hash2>`

---

### 5. Resolver conflictos en cherry-pick

Crea un conflicto:

```bash
git checkout feature/dashboard
echo "Dashboard version" > auth.js
git add auth.js
git commit -m "feat: modificar auth para dashboard"

git checkout feature/login
echo "Login version" > auth.js
git add auth.js
git commit -m "feat: modificar auth para login"

# Intenta cherry-pick este último commit en dashboard
git checkout feature/dashboard
git cherry-pick <hash-del-commit-login>
```

Resuelve el conflicto manualmente, luego:

```bash
git add auth.js
git cherry-pick --continue
```

---

### 6. Abortar cherry-pick

Si el conflicto es muy complicado:

```bash
git cherry-pick --abort
```

---

### 7. Implementar Git Flow - Crear rama develop

```bash
git checkout main
git checkout -b develop
```

---

### 8. Git Flow - Feature branches

Crea una feature desde develop:

```bash
git checkout develop
git checkout -b feature/user-profile

echo "Profile page" > profile.html
git add profile.html
git commit -m "feat: agregar página de perfil"

echo "Edit profile" >> profile.html
git add profile.html
git commit -m "feat: permitir edición de perfil"
```

---

### 9. Git Flow - Finalizar feature

Merge la feature en develop:

```bash
git checkout develop
git merge --no-ff feature/user-profile
```

**Pregunta:** ¿Qué hace el flag `--no-ff`?

Elimina la rama feature:

```bash
git branch -d feature/user-profile
```

---

### 10. Git Flow - Crear release branch

```bash
git checkout develop
git checkout -b release/1.0.0

# Hacer ajustes finales
echo "Version 1.0.0" > VERSION
git add VERSION
git commit -m "chore: preparar release 1.0.0"
```

---

### 11. Git Flow - Finalizar release

Merge a main:

```bash
git checkout main
git merge --no-ff release/1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"
```

Merge de vuelta a develop:

```bash
git checkout develop
git merge --no-ff release/1.0.0
```

Eliminar rama release:

```bash
git branch -d release/1.0.0
```

---

### 12. Git Flow - Hotfix

Crear hotfix desde main:

```bash
git checkout main
git checkout -b hotfix/1.0.1

echo "Critical fix" > hotfix.txt
git add hotfix.txt
git commit -m "fix: corregir bug crítico"
```

Merge a main:

```bash
git checkout main
git merge --no-ff hotfix/1.0.1
git tag -a v1.0.1 -m "Hotfix 1.0.1"
```

Merge a develop:

```bash
git checkout develop
git merge --no-ff hotfix/1.0.1
```

Eliminar hotfix:

```bash
git branch -d hotfix/1.0.1
```

---

### 13. Visualizar el workflow

```bash
git log --all --graph --oneline --decorate
```

---

### 14. GitHub Flow (alternativa simple)

Crea y prueba GitHub Flow:

```bash
git checkout main

# 1. Crear rama desde main
git checkout -b feature/new-feature

# 2. Hacer commits
echo "New feature" > feature.txt
git add feature.txt
git commit -m "feat: agregar nueva funcionalidad"

# 3. Push y crear PR
git push origin feature/new-feature

# 4. Después de revisión, merge a main
git checkout main
git merge feature/new-feature

# 5. Deploy (simulado)
echo "Deployed!"
```

**Pregunta:** ¿Cuándo usarías GitHub Flow en lugar de Git Flow?

---

## Verificación

Al finalizar deberías tener:

- ✅ Experiencia con cherry-pick
- ✅ Comprensión de Git Flow completo
- ✅ Ramas main, develop y varias features
- ✅ Tags de versiones
- ✅ Historial organizado

Verifica con:

```bash
git log --all --graph --oneline --decorate
git tag
git branch -a
```

---

## Preguntas de Reflexión

1. ¿Cuándo es apropiado usar cherry-pick?
2. ¿Qué ventajas y desventajas tiene Git Flow?
3. ¿Por qué algunos equipos prefieren GitHub Flow?
4. ¿Qué es trunk-based development?

---

## Desafío Extra

1. Implementa un script que automatice la creación de release branches
2. Investiga GitLab Flow y compáralo con Git Flow
3. Configura branch protection rules en GitHub

---

**Anterior:** [← Ejercicio 6](exercise-06.md)  
**Solución:** [Ver solución →](solutions/solution-07.md)

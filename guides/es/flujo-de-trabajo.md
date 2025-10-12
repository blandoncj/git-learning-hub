# Flujo de Trabajo

Guía completa sobre cómo trabajar eficientemente con Git en equipos. Aprenderás mejores prácticas, convenciones de commits, flujo de Pull Requests y resolución de conflictos.

## Tabla de Contenidos

1. [Flujo Básico](#flujo-básico)
2. [Convenciones de Commits](#convenciones-de-commits)
3. [Pull Requests](#pull-requests)
4. [Code Review](#code-review)
5. [Merge y Conflictos](#merge-y-conflictos)
6. [Rebase vs Merge](#rebase-vs-merge)
7. [Squash de Commits](#squash-de-commits)
8. [Mejores Prácticas](#mejores-prácticas)

---

## Flujo Básico

### Workflow Completo Paso a Paso

#### Paso 1: Clonar el Repositorio

```bash
git clone https://github.com/usuario/proyecto.git
cd proyecto
```

#### Paso 2: Crear una Rama para tu Característica

```bash
# Asegúrate de estar en la rama principal
git checkout main
git pull origin main

# Crear rama de característica
git checkout -b feature/nueva-funcionalidad
```

#### Paso 3: Hacer Cambios

```bash
# Editar archivos
nano archivo.txt

# Ver cambios
git status
```

#### Paso 4: Preparar Cambios (Staging)

```bash
# Agregar archivo específico
git add archivo.txt

# O agregar todos los cambios
git add .
```

#### Paso 5: Hacer Commit

```bash
git commit -m "Descripción clara del cambio"
```

#### Paso 6: Hacer Push

```bash
# Primera vez (establece upstream)
git push -u origin feature/nueva-funcionalidad

# Próximas veces
git push
```

#### Paso 7: Crear Pull Request

1. Ve a GitHub/GitLab/Bitbucket
2. Haz clic en "Create Pull Request" o "New Merge Request"
3. Selecciona la rama base (main o develop)
4. Agrega descripción
5. Asigna revisores
6. Haz clic en "Create"

#### Paso 8: Code Review

- Los revisores examinan tu código
- Pueden pedir cambios
- Tú haces ajustes y haces push nuevamente

#### Paso 9: Fusionar (Merge)

Cuando el PR es aprobado:

1. Un mantenedor hace click en "Merge"
2. Elige estrategia de merge (merge commit, squash, rebase)
3. Confirma el merge

#### Paso 10: Limpiar

```bash
# Eliminar rama local
git branch -d feature/nueva-funcionalidad

# Actualizar main local
git checkout main
git pull origin main
```

---

## Convenciones de Commits

### Formato Conventional Commits

```
<tipo>(<alcance>): <asunto>

<cuerpo>

<pie de página>
```

### Tipos de Commit

| Tipo     | Uso                             | Ejemplo                                |
| -------- | ------------------------------- | -------------------------------------- |
| feat     | Nueva característica            | feat(auth): agregar login con Google   |
| fix      | Corrección de bug               | fix(checkout): resolver error de pago  |
| docs     | Cambios en documentación        | docs(readme): actualizar instrucciones |
| style    | Cambios de formato (sin lógica) | style: ajustar indentación             |
| refactor | Reorganización de código        | refactor(api): simplificar endpoints   |
| test     | Agregar/modificar tests         | test(login): aumentar cobertura        |
| perf     | Mejoras de performance          | perf(db): optimizar queries            |
| ci       | Cambios en CI/CD                | ci: agregar GitHub Actions             |
| chore    | Tareas menores                  | chore: actualizar dependencias         |

### Ejemplos de Buenos Commits

```
✓ feat(carrito): agregar funcionalidad de cantidad
✓ fix(validacion): corregir error de email vacío
✓ docs(instalacion): agregar pasos para Windows
✓ refactor(componentes): extraer lógica común
✓ test(autenticacion): agregar tests de JWT
```

### Ejemplos de Malos Commits

```
✗ cambios
✗ fix bug
✗ Updates
✗ WIP
✗ asdfghjkl
✗ TODO: arreglar después
```

### Guía para Escribir Mensajes

**Asunto:**

- Máximo 50 caracteres
- Primera letra mayúscula
- Imperativo ("agregar", no "agregado")
- Sin punto al final
- Claro y descriptivo

**Cuerpo (opcional):**

- Máximo 72 caracteres por línea
- Explica QUÉ y POR QUÉ, no CÓMO
- Separa del asunto con línea en blanco

**Ejemplo completo:**

```
feat(carrito): agregar validación de stock

Cuando un usuario intenta agregar un producto,
ahora se verifica que el stock sea suficiente.
Si no hay stock, muestra un mensaje de error.

Soluciona el issue #123
```

---

## Pull Requests

### Crear un Pull Request

#### En GitHub

1. Haz push a tu rama
2. Entra al repositorio
3. Verás "Compare & pull request"
4. Haz clic
5. Rellena el título y descripción
6. Haz clic en "Create pull request"

#### En GitLab

1. Haz push a tu rama
2. Entra al repositorio
3. Ve a "Merge requests"
4. Haz clic en "New merge request"
5. Selecciona ramas (source → target)
6. Rellena descripción
7. Haz clic en "Create merge request"

#### En Bitbucket

1. Haz push a tu rama
2. Entra al repositorio
3. Ve a "Pull requests"
4. Haz clic en "Create pull request"
5. Selecciona ramas
6. Rellena descripción
7. Haz clic en "Create"

### Descripción de Pull Request

```markdown
## Descripción

Breve descripción de qué hace este PR.

## Tipo de cambio

- [ ] Nueva característica
- [ ] Corrección de bug
- [ ] Cambio que rompe compatibilidad

## Cambios realizados

- Cambio 1
- Cambio 2
- Cambio 3

## Testing

Describe cómo probaste los cambios.

## Checklist

- [ ] Mi código sigue las convenciones del proyecto
- [ ] He actualizado la documentación
- [ ] He agregado tests
- [ ] Todos los tests pasan
- [ ] No hay conflictos con la rama base

## Screenshots (si aplica)

Agrega screenshots de cambios visuales.

## Issues relacionados

Cierra #123
```

### Gestionar Feedback

**Cuando el revisor pide cambios:**

1. Haz los cambios localmente
2. Haz commit y push (no necesitas crear nuevo PR)
3. El PR se actualiza automáticamente
4. Responde a los comentarios

**Nunca fuerces un push después de crear un PR:**

```bash
# MAL - No hagas esto
git push --force

# BIEN - Haz esto
git push
```

---

## Code Review

### Para Desarrolladores

**Antes de crear un PR:**

- ✅ Tu código funciona y está probado
- ✅ Seguiste las convenciones del proyecto
- ✅ No hay conflictos con la rama base
- ✅ Escribiste tests
- ✅ Documentaste cambios importantes

**Durante el review:**

- 📝 Responde a todos los comentarios
- 🔄 Haz cambios si es necesario
- 💬 Explica tus decisiones
- 🙏 Agradece por el feedback

### Para Revisores

**Qué revisar:**

✅ **Funcionalidad:** ¿Hace lo que promete?
✅ **Legibilidad:** ¿Se entiende el código?
✅ **Tests:** ¿Hay suficiente cobertura?
✅ **Performance:** ¿Hay optimizaciones obvias?
✅ **Seguridad:** ¿Hay vulnerabilidades?
✅ **Convenciones:** ¿Sigue las normas del proyecto?

**Cómo comentar:**

```
Malo:
"Esto está mal"

Bueno:
"Aquí podríamos usar .map() en lugar de un for loop para mayor claridad"

Mejor:
"Aquí podríamos usar .map() en lugar de un for loop para mayor claridad.
Ejemplo:
const resultado = datos.map(item => item.valor);
Esto es más funcional y fácil de leer."
```

---

## Merge y Conflictos

### Tipos de Merge

#### Merge Commit

```bash
git checkout main
git pull origin main
git merge feature/nueva-funcionalidad
```

**Resultado:** Historial con rama visible
**Uso:** Cuando quieres preservar la historia de la rama

#### Squash and Merge

```bash
git merge --squash feature/nueva-funcionalidad
git commit
```

**Resultado:** Un solo commit en main
**Uso:** Cuando quieres un historial limpio

#### Rebase and Merge

```bash
git rebase main
git checkout main
git merge --ff-only feature/nueva-funcionalidad
```

**Resultado:** Historial lineal
**Uso:** Cuando quieres un historial limpio y lineal

### Resolver Conflictos

#### Detectar Conflicto

```bash
git merge otra-rama
# CONFLICT (content merge conflict) in archivo.txt
```

#### Ver Conflicto

El archivo tendrá marcadores:

```
<<<<<<< HEAD
Tu versión del código
=======
Versión de la otra rama
>>>>>>> rama-nombre
```

#### Resolver Manualmente

1. Abre el archivo
2. Decide qué código mantener
3. Elimina los marcadores
4. Guarda el archivo

**Ejemplo:**

```
// ANTES (con conflicto)
<<<<<<< HEAD
function sumar(a, b) {
  return a + b;
}
=======
function sumar(a, b) {
  return a + b + 1;
}
>>>>>>> bug-fix

// DESPUÉS (resuelto)
function sumar(a, b) {
  return a + b;
}
```

#### Completar Merge

```bash
git add archivo-resuelto.js
git commit -m "Resolver conflicto de merge"
git push
```

#### Usar Herramientas de Merge

```bash
# Visual Studio Code
git mergetool

# O herramienta específica
git config merge.tool vscode
git mergetool
```

#### Abortar Merge

Si quieres cancelar:

```bash
git merge --abort
```

---

## Rebase vs Merge

### Merge

**Cómo funciona:**

```
Antes:
feature: A - B
           \
main:   C - D

Después (merge):
feature: A - B
           \ |
main:   C - D - M (merge commit)
```

**Ventajas:**
✅ Preserva el historial completo
✅ Seguro para ramas públicas
✅ Fácil de entender

**Desventajas:**
❌ Historial más complejo
❌ Muchos merge commits

**Cuándo usar:**

- En ramas compartidas
- Cuando historial importa
- En repositorios públicos

### Rebase

**Cómo funciona:**

```
Antes:
feature: A - B
           \
main:   C - D

Después (rebase):
feature:       A' - B'
               /
main:       C - D
```

**Ventajas:**
✅ Historial lineal y limpio
✅ Fácil de seguir
✅ Menos commits

**Desventajas:**
❌ Reescribe historial
❌ Peligroso en ramas públicas
❌ Puede ser confuso

**Cuándo usar:**

- En ramas locales
- Antes de hacer merge a main
- Para limpiar historial personal

### Rebase Interactivo

```bash
# Rebase sobre los últimos 3 commits
git rebase -i HEAD~3
```

Se abre un editor con opciones:

```
pick a1b2c3d Commit 1
pick d4e5f6g Commit 2
pick h7i8j9k Commit 3

# Opciones:
# p, pick = usar commit
# r, reword = usar pero editar mensaje
# s, squash = usar pero fusionar con anterior
# f, fixup = como squash pero sin mensaje
# d, drop = eliminar commit
```

---

## Squash de Commits

### ¿Qué es Squash?

Combina múltiples commits en uno solo.

**Antes:**

```
a1b2c3d Agregar función login
d4e5f6g Corregir bug en login
h7i8j9k Actualizar tests de login
```

**Después (squash):**

```
a1b2c3d Agregar función login con bugfixes
```

### Squash Interactivo

```bash
# Ver últimos 3 commits
git log --oneline -3

# Squashear
git rebase -i HEAD~3
```

Cambia `pick` a `squash` (o `s`):

```
pick a1b2c3d Agregar función login
squash d4e5f6g Corregir bug en login
squash h7i8j9k Actualizar tests de login
```

Guarda, y edita el mensaje final.

### Squash al Fusionar

En GitHub/GitLab/Bitbucket, muchas plataformas permiten "Squash and merge" directamente.

---

## Mejores Prácticas

### ✅ Lo Que DEBES Hacer

**1. Commits Atómicos**

```bash
# Bien: Cada commit hace una cosa
git commit -m "feat: agregar campo de email"
git commit -m "test: agregar test para email"

# Mal: Un commit hace muchas cosas
git commit -m "feat: agregar campo de email, actualizar tests, corregir bug"
```

**2. Rebase Antes de Push**

```bash
# Actualizar rama con main
git pull --rebase origin main

# Esto evita merge commits innecesarios
```

**3. Revisar Antes de Push**

```bash
# Ver qué vas a empujar
git diff main origin/main
```

**4. Nombra Ramas Descriptivas**

```
✓ feature/agregar-carrito-compras
✗ feature/cambios
```

### ❌ Lo Que NO DEBES Hacer

**1. Force Push en Ramas Compartidas**

```bash
# NUNCA hagas esto en ramas públicas
git push --force
```

**2. Commits con Mensajes Genéricos**

```bash
# Mal
git commit -m "fix"
git commit -m "updates"

# Bien
git commit -m "fix(login): corregir validación de contraseña"
```

**3. Cambios No Relacionados en Un Commit**

```bash
# Mal: Mezclar cambios
git add .
git commit -m "Cambios varios"

# Bien: Commits separados
git add archivo1.js
git commit -m "feat: agregar función"
git add archivo2.js
git commit -m "fix: corregir bug"
```

**4. Olvidar Actualizar con Main**

```bash
# Siempre actualiza antes de crear PR
git pull origin main
```

**5. Commits Incompletos**

```bash
# No hagas commit de código roto
git add .
git commit -m "WIP - no funciona aún"
```

### Workflow Recomendado Diario

```bash
# 1. Mañana: Actualizar
git pull origin main

# 2. Crear rama
git checkout -b feature/mi-trabajo

# 3. Durante el día: Commits frecuentes
git add archivo1.js
git commit -m "feat: parte 1"
git add archivo2.js
git commit -m "feat: parte 2"

# 4. Antes de push: Limpiar
git rebase -i origin/main  # Squash si es necesario

# 5. Push
git push -u origin feature/mi-trabajo

# 6. Crear PR
# (en GitHub/GitLab/Bitbucket)

# 7. Después de merge: Limpiar
git checkout main
git pull
git branch -d feature/mi-trabajo
```

---

## Próximos Pasos

1. **[Rastreo de Cambios](rastreo-cambios.md)** - Auditar y seguimiento
2. **[Gestión de Ramas](gestion-ramas.md)** - Repaso de ramas
3. **[Autenticación](autenticacion-ssh-https.md)** - Repaso de seguridad

---

**Anterior:** [Gestión de Ramas ←](gestion-ramas.md) | **Siguiente:** [Rastreo de Cambios →](rastreo-cambios.md)

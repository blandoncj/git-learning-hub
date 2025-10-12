# Gestión de Ramas

Guía completa sobre cómo crear, cambiar, eliminar y gestionar ramas (branches) en Git. Aprenderás estrategias de ramificación, nomenclatura, y mejores prácticas.

## Tabla de Contenidos

1. [¿Qué es una Rama?](#qué-es-una-rama)
2. [Ramas Especiales](#ramas-especiales)
3. [Crear Ramas](#crear-ramas)
4. [Cambiar entre Ramas](#cambiar-entre-ramas)
5. [Ver Ramas](#ver-ramas)
6. [Eliminar Ramas](#eliminar-ramas)
7. [Nomenclatura de Ramas](#nomenclatura-de-ramas)
8. [Estrategias de Ramificación](#estrategias-de-ramificación)
9. [Sincronizar Ramas](#sincronizar-ramas)
10. [Troubleshooting](#troubleshooting)

---

## ¿Qué es una Rama?

### Definición

Una **rama** (branch) es una línea independiente de desarrollo. Permite trabajar en nuevas características, correcciones o experimentos sin afectar el código principal.

### Cómo Funciona

Cada rama tiene su propio historial de commits. Puedes:

- Trabajar en múltiples características simultáneamente
- Aislar cambios experimentales
- Colaborar sin conflictos
- Fusionar cambios cuando estén listos

### Analogía

Imagina un árbol:

- **Tronco:** La rama principal (main/master)
- **Ramas:** Líneas de trabajo independientes
- **Hojas:** Commits en cada rama
- **Injertos:** Merges que unen ramas

---

## Ramas Especiales

### Main (o Master)

**Antes:** Se llamaba `master`
**Ahora:** Se llama `main` (por inclusividad)

**Características:**

- ✅ La rama principal del proyecto
- ✅ Código estable y listo para producción
- ✅ Generalmente protegida (requiere revisión)
- ✅ Cada commit es una versión estable

**Protección típica:**

- Requiere Pull Request para fusionar
- Requiere revisión de código
- Debe pasar todos los tests
- No se fuerza push (force push)

### Develop (o Development)

**Características:**

- ✅ Rama de desarrollo del proyecto
- ✅ Integra características antes de main
- ✅ Puede ser menos estable que main
- ✅ Punto de integración para features

**Uso típico:**

- Las features se ramifican de develop
- Se prueban en develop
- Cuando están listas, se fusionan a main

### Feature Branches

**Características:**

- ✅ Ramas temporales para características
- ✅ Se crean desde develop
- ✅ Se eliminan después del merge
- ✅ Aisladas del código principal

### Hotfix Branches

**Características:**

- ✅ Para correcciones urgentes en producción
- ✅ Se crean desde main
- ✅ Se fusionan inmediatamente a main y develop
- ✅ Se eliminan después del merge

---

## Crear Ramas

### Opción 1: Crear Rama Localmente

```bash
# Crear rama
git branch nombre-rama

# Verificar que se creó
git branch
```

**Nota:** Solo crea la rama localmente, no cambia a ella.

### Opción 2: Crear y Cambiar en Un Paso

```bash
# Crear y cambiar a la rama (recomendado)
git checkout -b nombre-rama

# O con git switch (más moderno)
git switch -c nombre-rama
```

### Opción 3: Crear Rama desde Otra Rama

```bash
# Estar en la rama desde donde quieres crear
git checkout feature/otra-rama

# Crear nueva rama (rama secundaria)
git checkout -b feature/nueva-rama
```

### Opción 4: Crear Rama desde un Commit Específico

```bash
# Crear rama en un commit anterior
git checkout -b nombre-rama abc123def

# abc123def es el hash del commit
```

### Opción 5: Crear Rama Remota

```bash
# Crear localmente
git checkout -b nombre-rama

# Hacer push (crea en remoto)
git push -u origin nombre-rama

# -u establece la rama remota como upstream
```

**Verificar:**

```bash
git branch -a  # Ver todas (locales y remotas)
```

---

## Cambiar entre Ramas

### Opción 1: git checkout

```bash
# Cambiar a otra rama
git checkout nombre-rama

# Cambiar a main
git checkout main
```

**Nota:** Debe no haber cambios sin commit. De lo contrario, Git te avisará.

### Opción 2: git switch (Más Moderno)

```bash
# Cambiar a otra rama
git switch nombre-rama

# Cambiar a main
git switch main
```

**Ventaja:** Sintaxis más clara que checkout.

### Opción 3: git switch - (Rama Anterior)

```bash
# Cambiar a la rama anterior (como cd -)
git switch -

# O con checkout
git checkout -
```

### Verificar Rama Actual

```bash
# Ver rama actual
git branch

# La rama actual tendrá un asterisco (*)
# Ejemplo:
#   main
# * feature/nueva-rama
#   develop
```

### Cambiar y Crear si No Existe

```bash
# Si no existe, la crea
git switch -c nombre-rama

# O con checkout
git checkout -b nombre-rama
```

---

## Ver Ramas

### Ver Ramas Locales

```bash
git branch
```

Verás:

```
  develop
* main
  feature/login
```

El asterisco (\*) indica la rama actual.

### Ver Ramas Remotas

```bash
git branch -r
```

Verás:

```
  origin/main
  origin/develop
  origin/feature/login
```

### Ver Todas las Ramas

```bash
git branch -a
```

Verás:

```
  develop
* main
  feature/login
  remotes/origin/main
  remotes/origin/develop
  remotes/origin/feature/login
```

### Ver Ramas con Más Información

```bash
# Última commit de cada rama
git branch -v

# Verás:
#   develop    abc123d Merge branch 'feature/logout'
# * main       def456e Add production build script
#   feature/l  ghi789f Add login form
```

### Ver Ramas Fusionadas

```bash
# Ramas que ya fueron fusionadas a main
git branch --merged

# Ramas que NO han sido fusionadas
git branch --no-merged
```

### Ver Ramas por Fecha

```bash
# Ramas ordenadas por última actividad
git branch -v --sort=-committerdate
```

---

## Eliminar Ramas

### Eliminar Rama Local

```bash
# Eliminar rama (seguro - verifica si fue fusionada)
git branch -d nombre-rama

# Forzar eliminación (sin verificar)
git branch -D nombre-rama
```

**Diferencia:**

- `-d`: Seguro. No elimina si no está fusionada.
- `-D`: Fuerza. Elimina sin importar el estado.

**Ejemplo:**

```bash
# Desarrollaste feature/login y lo fusionaste
git branch -d feature/login
# ✓ Eliminado

# Si lo quieres forzar
git branch -D feature/login
```

### Eliminar Rama Remota

```bash
# Eliminar en origin
git push origin --delete nombre-rama

# O sintaxis alternativa
git push origin -d nombre-rama

# O versión antigua
git push origin :nombre-rama
```

**Ejemplo:**

```bash
git push origin --delete feature/logout
```

### Limpiar Ramas Remotas Eliminadas

Si alguien elimina una rama remota, tu local aún la ve:

```bash
# Sincronizar ramas remotas
git fetch --prune

# O especificar remoto
git fetch --prune origin
```

---

## Nomenclatura de Ramas

### Convención Recomendada

```
tipo/descripcion-en-minusculas
```

### Tipos Comunes

| Tipo     | Propósito                | Ejemplo                  |
| -------- | ------------------------ | ------------------------ |
| feature  | Nueva característica     | feature/agregar-carrito  |
| fix      | Corrección de bug        | fix/validacion-email     |
| hotfix   | Corrección urgente       | hotfix/crash-pagina-pago |
| refactor | Reorganización de código | refactor/mejorar-api     |
| docs     | Cambios en documentación | docs/actualizar-readme   |
| test     | Agregar/mejorar tests    | test/cobertura-login     |
| perf     | Mejoras de performance   | perf/optimizar-bd        |
| ci       | Cambios en CI/CD         | ci/github-actions-setup  |
| chore    | Tareas menores           | chore/actualizar-deps    |

### Ejemplos Buenos

```
✓ feature/agregar-autenticacion-oauth
✓ fix/error-404-pagina-producto
✓ hotfix/crash-al-pagar
✓ refactor/simplificar-componente-modal
✓ docs/guia-instalacion
```

### Ejemplos Malos

```
✗ nueva-rama
✗ cambios
✗ temporal
✗ test123
✗ FEATURE/AGREGAR-CARRITO (usar minúsculas)
```

### Reglas de Nombres

✅ **Usa minúsculas**
✅ **Separa con guiones (-)**
✅ **Sin espacios**
✅ **Sin caracteres especiales**
✅ **Descriptivo pero conciso**
✅ **Máximo 50 caracteres**

---

## Estrategias de Ramificación

### Git Flow

**Estructura:**

```
main (releases)
 └─ develop (integration)
     ├─ feature/... (development)
     ├─ release/... (release prep)
     └─ hotfix/... (urgent fixes)
```

**Flujo:**

1. Creas feature desde develop
2. Trabajas en feature
3. Haces Pull Request a develop
4. Se revisa y fusiona
5. Cuando está listo, release va a main
6. Si hay emergencia, hotfix va a main y develop

**Ventajas:**
✅ Estructura clara
✅ Bueno para releases planificados
✅ Separación clara entre desarrollo y producción

**Desventajas:**
❌ Más complejo
❌ Mucho merge
❌ No ideal para CI/CD frecuente

### GitHub Flow (Recomendado para Equipos Ágiles)

**Estructura:**

```
main (always deployable)
 └─ feature/... (work in progress)
```

**Flujo:**

1. Creas feature desde main
2. Trabajas en feature
3. Haces Pull Request a main
4. Se revisa y fusiona
5. Se despliega a producción inmediatamente

**Ventajas:**
✅ Simple y directo
✅ Despliegue continuo
✅ Menos conflictos
✅ Ideal para CI/CD

**Desventajas:**
❌ Main siempre debe ser estable
❌ Menos control sobre versiones

### Trunk-Based Development

**Estructura:**

```
main (todos hacen commit aquí)
```

**Flujo:**

1. Todos trabajan en main
2. Commits pequeños y frecuentes
3. Feature flags para controlar nueva funcionalidad
4. Tests automáticos en cada commit

**Ventajas:**
✅ Muy simple
✅ Menos conflictos
✅ Integración continua máxima

**Desventajas:**
❌ Requiere tests muy buenos
❌ Requiere disciplina del equipo
❌ Difícil para equipos grandes

### Recomendación por Equipo

- **Equipos pequeños:** GitHub Flow
- **Equipos medianos:** GitHub Flow o Git Flow
- **Equipos grandes:** Git Flow
- **Equipos muy ágiles:** Trunk-Based

---

## Sincronizar Ramas

### Actualizar Rama Local desde Remoto

```bash
# Traer cambios del remoto
git fetch origin nombre-rama

# Actualizar rama local
git pull origin nombre-rama

# O en un paso
git pull
```

### Actualizar Feature desde Develop

```bash
# Estar en la feature branch
git checkout feature/login

# Traer cambios de develop
git pull origin develop

# Si hay conflictos, resolverlos y hacer commit
git add .
git commit -m "Resolver conflictos con develop"
git push
```

### Sincronizar todas las Ramas

```bash
# Obtener todas las actualizaciones
git fetch --all

# Ver todas (verás cambios remotos)
git branch -a
```

---

## Troubleshooting

### "error: pathspec 'nombre-rama' did not match any file"

**Causa:** La rama no existe.

**Solución:**

```bash
# Ver ramas disponibles
git branch -a

# Crear la rama primero
git checkout -b nombre-rama
```

### "fatal: Cannot switch to a branch with uncommitted changes"

**Causa:** Tienes cambios sin guardar.

**Solución:**

```bash
# Opción 1: Hacer commit
git add .
git commit -m "Cambios intermedios"
git checkout otra-rama

# Opción 2: Guardar temporalmente (stash)
git stash
git checkout otra-rama
git stash pop

# Opción 3: Descartar cambios
git checkout -- .
git checkout otra-rama
```

### "fatal: A branch named 'nombre' already exists"

**Causa:** La rama ya existe.

**Solución:**

```bash
# Ver ramas
git branch -a

# Usar nombre diferente
git checkout -b nombre-diferente

# O cambiar a rama existente
git checkout nombre-rama
```

### "error: src refspec main does not match any"

**Causa:** Rama no existe en remoto o Git está confundido.

**Solución:**

```bash
# Crear commit en main primero
git commit --allow-empty -m "Commit inicial"

# O hacer push de rama actual
git push -u origin nombre-rama
```

### Rama remota eliminada pero aún visible localmente

```bash
# Sincronizar
git fetch --prune

# O específico
git fetch --prune origin
```

---

## Resumen de Comandos

```bash
# CREAR
git checkout -b nombre-rama          # Crear y cambiar
git branch nombre-rama                # Solo crear

# VER
git branch                           # Locales
git branch -a                        # Todas
git branch -v                        # Con info

# CAMBIAR
git checkout nombre-rama             # Cambiar (antiguo)
git switch nombre-rama               # Cambiar (moderno)

# ELIMINAR
git branch -d nombre-rama            # Seguro
git branch -D nombre-rama            # Forzar
git push origin --delete nombre-rama # Remoto

# SINCRONIZAR
git fetch origin                     # Traer cambios
git pull origin nombre-rama          # Traer y fusionar
git push -u origin nombre-rama       # Enviar

# INFO
git branch --merged                  # Fusionadas
git branch --no-merged              # Sin fusionar
```

---

## Próximos Pasos

1. **[Flujo de Trabajo](flujo-de-trabajo.md)** - Workflows en equipo
2. **[Rastreo de Cambios](rastreo-cambios.md)** - Auditar código
3. **[Autenticación SSH/HTTPS](autenticacion-ssh-https.md)** - Repaso

---

**Anterior:** [Configuración GPG](configuracion-gpg.md) | **Siguiente:** [Flujo de Trabajo](flujo-de-trabajo.md)

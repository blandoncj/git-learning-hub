# Rastreo de Cambios

Guía completa sobre cómo auditar, rastrear y analizar cambios en tu código. Aprenderás a usar comandos de Git y herramientas visuales para entender quién hizo qué y cuándo.

## Tabla de Contenidos

1. [Ver el Historial](#ver-el-historial)
2. [Blame - Quién Cambió Qué](#blame---quién-cambió-qué)
3. [Diff - Ver Diferencias](#diff---ver-diferencias)
4. [Show - Detalles de Commit](#show---detalles-de-commit)
5. [GitLens en VSCode](#gitlens-en-vscode)
6. [SourceTree](#sourcetree)
7. [Búsqueda en Historial](#búsqueda-en-historial)
8. [Análisis Avanzado](#análisis-avanzado)

---

## Ver el Historial

### git log - Comando Básico

```bash
# Ver historial completo
git log

# Ver últimos N commits
git log -n 5

# Ver últimos 5 commits
git log -5
```

**Salida:**

```
commit abc123def456 (HEAD -> main)
Author: Juan Pérez <juan@ejemplo.com>
Date:   Mon Jan 15 10:30:45 2024 -0500

    feat: agregar carrito de compras

commit ghi789jkl012
Author: María López <maria@ejemplo.com>
Date:   Sun Jan 14 15:20:30 2024 -0500

    fix: corregir validación de email
```

### git log - Formato en Una Línea

```bash
# Ver de forma compacta
git log --oneline

# Ver últimos 10
git log --oneline -10
```

**Salida:**

```
abc123d (HEAD -> main) feat: agregar carrito
ghi789j fix: corregir validación
klm456n docs: actualizar readme
```

### git log - Formato Personalizado

```bash
# Formato personalizado
git log --pretty=format:"%h %an %ar %s"

# Donde:
# %h = hash corto
# %an = nombre del autor
# %ar = fecha relativa (2 days ago)
# %s = asunto del commit
```

**Salida:**

```
abc123d Juan Pérez 2 days ago feat: agregar carrito
ghi789j María López 3 days ago fix: corregir validación
```

### git log - Con Gráfico de Ramas

```bash
# Ver todas las ramas visualmente
git log --oneline --graph --all

# Versión más detallada
git log --oneline --graph --all --decorate
```

**Salida:**

```
* abc123d (main) feat: agregar carrito
|\
| * def456g (feature/login) feat: agregar login
|/
* ghi789j fix: corregir validación
* jkl012m docs: actualizar readme
```

### git log - Filtrar por Autor

```bash
# Ver commits de un autor específico
git log --author="Juan Pérez"

# O con email
git log --author="juan@ejemplo.com"
```

### git log - Filtrar por Fecha

```bash
# Últimas 24 horas
git log --since="24 hours ago"

# Desde una fecha específica
git log --since="2024-01-15" --until="2024-01-20"

# Últimas 2 semanas
git log --since="2 weeks ago"
```

### git log - Filtrar por Archivo

```bash
# Ver cambios a un archivo específico
git log -- archivo.js

# Ver cambios a una carpeta
git log -- carpeta/

# Con oneline
git log --oneline -- archivo.js
```

---

## Blame - Quién Cambió Qué

### git blame - Básico

El comando `blame` muestra quién modificó cada línea de un archivo.

```bash
# Ver quién cambió cada línea
git blame archivo.js
```

**Salida:**

```
abc123d (Juan Pérez 2024-01-15 10:30) function sumar(a, b) {
abc123d (Juan Pérez 2024-01-15 10:30)   return a + b;
def456g (María López 2024-01-14 15:20) }
```

### git blame - Con Más Contexto

```bash
# Ver número de línea
git blame -n archivo.js

# Ver fecha y hora
git blame -t archivo.js

# Ver email del autor
git blame -e archivo.js
```

### git blame - Ignorar Espacios

```bash
# Ignorar cambios de espacios en blanco
git blame -w archivo.js

# Ignorar comentarios vacíos
git blame --ignore-all-space archivo.js
```

### git blame - Rango de Líneas

```bash
# Ver solo líneas 10-50
git blame -L 10,50 archivo.js

# Ver líneas de la 10 en adelante
git blame -L 10 archivo.js
```

### Interpretar Blame

```
abc123d (Juan Pérez 2024-01-15 10:30:45 -0500) function sumar(a, b) {
         │           │      │       │        │        │
         │           │      │       │        │        └── Código
         │           │      │       │        └── Hora exacta
         │           │      │       └── Fecha
         │           │      └── Nombre del autor
         │           └── Email del autor
         └── Hash del commit
```

---

## Diff - Ver Diferencias

### git diff - Cambios Sin Hacer Commit

```bash
# Ver cambios en archivo actual
git diff

# Ver cambios de un archivo específico
git diff archivo.js

# Ver cambios de una carpeta
git diff carpeta/
```

**Salida:**

```
diff --git a/archivo.js b/archivo.js
index abc123d..def456g 100644
--- a/archivo.js
+++ b/archivo.js
@@ -1,5 +1,6 @@
 function sumar(a, b) {
   return a + b;
+  // Nuevo comentario
 }
```

### git diff - Cambios en Staging Area

```bash
# Ver cambios que van a ser commiteados
git diff --staged

# O versión corta
git diff --cached
```

### git diff - Comparar Commits

```bash
# Diferencia entre dos commits
git commit abc123d def456g

# Diferencia entre HEAD y hace 2 commits
git diff HEAD~2

# Diferencia entre main y feature
git diff main feature/nueva-rama
```

### git diff - Estadísticas

```bash
# Ver resumen de cambios
git diff --stat

# Ver más detalles
git diff --stat --summary
```

**Salida:**

```
archivo.js      | 3 +-
funcion.js      | 15 +++++-
README.md       | 2 ++
---------
3 files changed, 19 insertions(+), 1 deletion(-)
```

### git diff - Formato Unificado

```bash
# Vista previa antes/después
git diff -U3  # 3 líneas de contexto

# Mucha más contexto
git diff -U10
```

---

## Show - Detalles de Commit

### git show - Ver Commit Completo

```bash
# Ver un commit específico
git show abc123d

# Ver HEAD (último commit)
git show HEAD

# Ver hace 2 commits
git show HEAD~2
```

**Salida:**

```
commit abc123def456
Author: Juan Pérez <juan@ejemplo.com>
Date:   Mon Jan 15 10:30:45 2024 -0500

    feat: agregar carrito de compras

    - Agrega funcionalidad de carrito
    - Persiste en localStorage
    - Muestra total

diff --git a/carrito.js b/carrito.js
new file mode 100644
index 0000000..abc123d
--- /dev/null
+++ b/carrito.js
@@ -0,0 +1,50 @@
+function crearCarrito() {
...
```

### git show - Solo el Mensaje

```bash
# Ver solo el mensaje del commit
git show --format=fuller abc123d

# Ver solo el diff
git show --no-patch abc123d
```

### git show - Archivo Específico en Commit

```bash
# Ver cómo estaba un archivo en un commit
git show abc123d:archivo.js

# Ver archivo hace 2 commits
git show HEAD~2:archivo.js
```

---

## GitLens en VSCode

GitLens es una extensión poderosa que integra información de Git directamente en VSCode.

### Instalación

1. Abre VSCode
2. Ve a Extensiones (Ctrl+Shift+X)
3. Busca "GitLens"
4. Haz clic en "Install"

### Características Principales

#### Inline Blame

Muestra información del commit al lado de cada línea:

```
│ Juan Pérez  3 days ago │ function sumar(a, b) {
│                        │   return a + b;
│ María López 1 day ago  │ }
```

**Cómo usar:**

- Hover sobre una línea
- Verás quién la cambió y cuándo
- Haz clic para ver el commit completo

#### Code Lens

Muestra información sobre commits por encima de funciones:

```
│ 2 commits │
│ function sumar(a, b) {
```

Haz clic para ver historial de esa función.

#### Command Palette

Presiona `Ctrl+Shift+P` y busca "GitLens":

- `GitLens: Show Commit` - Ver detalles del commit
- `GitLens: Show File History` - Ver historial del archivo
- `GitLens: Compare with Branch` - Comparar ramas
- `GitLens: Blame` - Ver blame inline

#### Side Panel

La barra lateral de GitLens muestra:

- **Repository** - Información del repo
- **File History** - Historial del archivo actual
- **Line History** - Historial de línea seleccionada
- **Commits** - Todos los commits

---

## SourceTree

SourceTree es una herramienta gráfica para Git muy popular.

### Instalación

Descarga desde [sourcetreeapp.com](https://www.sourcetreeapp.com/)

Disponible para Windows y macOS.

### Características Principales

#### Log Graph

Visualización gráfica de commits:

- Muestra todas las ramas
- Conexiones entre commits
- Fácil de ver historial

#### Blame View

Haz clic derecho en un archivo → "Show in Blame View":

- Muestra quién cambió cada línea
- Haz clic para ver el commit
- Colorizado por autor

#### Search

Búsqueda de commits:

- Por autor
- Por mensaje
- Por contenido

#### Compare

Compara archivos entre commits:

- Ver antes/después
- Diferencias destacadas
- Cambios línea por línea

---

## Búsqueda en Historial

### git log -S - Buscar Contenido

```bash
# Buscar commits que agregaron/quitaron una palabra
git log -S "variable_nombre"

# Ver qué cambió
git log -S "variable_nombre" -p
```

### git log -G - Búsqueda Regex

```bash
# Buscar con expresión regular
git log -G "function.*suma"

# Con cambios
git log -G "function.*suma" -p
```

### git log --grep - Buscar en Mensajes

```bash
# Buscar en mensajes de commit
git log --grep="bug"

# Case insensitive
git log --grep="bug" -i

# Múltiples términos (cualquiera)
git log --grep="bug\|error" -E
```

### git log --all --grep - Buscar en Todas las Ramas

```bash
# Buscar en todo el historial
git log --all --grep="feature"

# Con gráfico
git log --all --grep="feature" --oneline --graph
```

---

## Análisis Avanzado

### Estadísticas por Autor

```bash
# Ver commits por autor
git shortlog -s -n

# Ver commits con detalle
git shortlog
```

**Salida:**

```
    15 Juan Pérez
    12 María López
     8 Carlos Rodríguez
```

### Líneas de Código Cambió

```bash
# Ver líneas agregadas/removidas por archivo
git diff --stat HEAD~10

# Ver líneas por autor
git log --format="%an" -n 1000 | sort | uniq -c | sort -rn
```

### Ver Cambios en Ramas

```bash
# Commits en main que no están en feature
git log feature..main

# Commits en feature que no están en main
git log main..feature

# Commits en cualquiera de las dos
git log main...feature
```

### Reflog - Historial de Cambios de HEAD

```bash
# Ver todos los movimientos de HEAD
git reflog

# Útil para recuperar commits eliminados
git reflog show
```

**Uso:** Si accidentalmente eliminaste un commit, puedes recuperarlo con:

```bash
git checkout abc123d  # ID del commit en reflog
```

---

## Workflow de Auditoría Típico

```bash
# 1. ¿Quién cambió esta línea?
git blame archivo.js

# 2. ¿Cuándo se hizo ese cambio?
git show abc123d

# 3. ¿Qué otros cambios hizo esa persona ese día?
git log --author="Juan Pérez" --since="2024-01-15" --until="2024-01-16"

# 4. ¿Qué cambios hizo el commit abc123d?
git show abc123d --stat

# 5. ¿Cómo era el archivo antes?
git show HEAD~1:archivo.js

# 6. Ver en GitLens
# (Abre archivo en VSCode y usa GitLens)
```

---

## Próximos Pasos

1. **[Flujo de Trabajo](flujo-de-trabajo.md)** - Repaso de workflow
2. **[Gestión de Ramas](gestion-ramas.md)** - Repaso de ramas
3. **[Configuración GPG](configuracion-gpg.md)** - Seguridad

---

**Anterior:** [Flujo de Trabajo](flujo-de-trabajo.md)

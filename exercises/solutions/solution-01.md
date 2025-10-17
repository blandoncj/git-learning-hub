# Solución: Ejercicio 1 - Crear tu Primer Repositorio

## Solución Paso a Paso

### 1. Crear el repositorio

```bash
# Crear carpeta
mkdir mi-sitio-web
cd mi-sitio-web

# Inicializar repositorio
git init
```

**Salida esperada:**

```
Initialized empty Git repository in /ruta/mi-sitio-web/.git/
```

---

### 2. Crear un archivo

```bash
# Crear archivo index.html
nano index.html
# o
code index.html
```

Contenido:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Mi Sitio Web</title>
  </head>
  <body>
    <h1>Hola Mundo</h1>
  </body>
</html>
```

---

### 3. Ver el estado

```bash
git status
```

**Salida esperada:**

```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	index.html

nothing added to commit but untracked files present (use "git add" to track)
```

**Explicación:** El archivo está "untracked" (sin rastrear), Git lo ve pero no lo está siguiendo aún.

---

### 4. Agregar al staging area

```bash
git add index.html

# O agregar todo
git add .
```

---

### 5. Hacer tu primer commit

```bash
git commit -m "feat: agregar página de inicio"
```

**Salida esperada:**

```
[main (root-commit) abc123d] feat: agregar página de inicio
 1 file changed, 9 insertions(+)
 create mode 100644 index.html
```

---

### 6. Ver el historial

```bash
git log
```

**Salida esperada:**

```
commit abc123def456... (HEAD -> main)
Author: Tu Nombre <tu.email@ejemplo.com>
Date:   Mon Jan 15 10:30:45 2024 -0500

    feat: agregar página de inicio
```

**Información visible:**

- **commit abc123...:** Hash único del commit
- **Author:** Quién hizo el commit
- **Date:** Cuándo se hizo
- **Message:** Descripción del cambio

---

### 7. Modificar el archivo

```bash
# Editar index.html y agregar
nano index.html
```

Agregar dentro de `<body>`:

```html
<p>Bienvenido a mi sitio web</p>
```

Archivo completo:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Mi Sitio Web</title>
  </head>
  <body>
    <h1>Hola Mundo</h1>
    <p>Bienvenido a mi sitio web</p>
  </body>
</html>
```

---

### 8. Ver diferencias

```bash
git diff
```

**Salida esperada:**

```diff
diff --git a/index.html b/index.html
index abc123d..def456g 100644
--- a/index.html
+++ b/index.html
@@ -6,5 +6,6 @@
 <body>
     <h1>Hola Mundo</h1>
+    <p>Bienvenido a mi sitio web</p>
 </body>
 </html>
```

**Explicación:**

- `---` líneas viejas (en rojo en terminal)
- `+++` líneas nuevas (en verde en terminal)
- `+` línea agregada

---

### 9. Hacer segundo commit

```bash
# Agregar cambios
git add index.html

# Hacer commit
git commit -m "feat: agregar párrafo de bienvenida"
```

**Salida esperada:**

```
[main def456g] feat: agregar párrafo de bienvenida
 1 file changed, 1 insertion(+)
```

---

### 10. Ver historial completo

```bash
git log --oneline
```

**Salida esperada:**

```
def456g (HEAD -> main) feat: agregar párrafo de bienvenida
abc123d feat: agregar página de inicio
```

---

## Verificación Final

```bash
# Ver estado (debe estar limpio)
git status
```

**Salida esperada:**

```
On branch main
nothing to commit, working tree clean
```

```bash
# Ver historial con gráfico
git log --oneline --graph
```

**Salida esperada:**

```
* def456g (HEAD -> main) feat: agregar párrafo de bienvenida
* abc123d feat: agregar página de inicio
```

---

## Respuestas a Preguntas de Reflexión

### 1. ¿Qué diferencia hay entre working directory y staging area?

- **Working Directory:** Donde editas tus archivos, los cambios aún no están marcados para commit
- **Staging Area:** Área intermedia donde preparas los cambios antes de commitear. Usas `git add` para mover cambios aquí

**Analogía:**

- Working directory = escribir un email
- Staging area = poner en bandeja de salida
- Commit = enviar el email

### 2. ¿Por qué es importante escribir buenos mensajes de commit?

- Ayudan a entender QUÉ cambió y POR QUÉ
- Facilitan encontrar bugs en el historial
- Mejoran la colaboración en equipo
- Sirven como documentación del proyecto
- Permiten generar changelogs automáticamente

### 3. ¿Qué pasaría si no hicieras `git add` antes de `git commit`?

Git daría error porque no hay nada en el staging area para commitear:

```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)

no changes added to commit (use "git add" and/or "git commit -a")
```

**Excepción:** Si usas `

# Ejercicio 1: Crear tu Primer Repositorio

**Nivel:** Básico  
**Tiempo estimado:** 15 minutos  
**Objetivos:**

- Crear un repositorio local
- Hacer tu primer commit
- Ver el historial

---

## Contexto

Vas a crear un repositorio para un proyecto personal llamado "mi-sitio-web" donde guardarás archivos HTML básicos.

---

## Tareas

### 1. Crear el repositorio

Crea una carpeta y conviértela en un repositorio Git.

```bash
# Tu código aquí
```

**Pista:** Usa `git init`

---

### 2. Crear un archivo

Crea un archivo `index.html` con el siguiente contenido:

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

Verifica el estado del repositorio.

```bash
# Tu código aquí
```

**Pregunta:** ¿Qué mensaje ves? ¿Qué significa?

---

### 4. Agregar al staging area

Agrega el archivo al staging area.

```bash
# Tu código aquí
```

---

### 5. Hacer tu primer commit

Haz un commit con el mensaje "feat: agregar página de inicio".

```bash
# Tu código aquí
```

---

### 6. Ver el historial

Muestra el historial de commits.

```bash
# Tu código aquí
```

**Pregunta:** ¿Qué información ves en el log?

---

### 7. Modificar el archivo

Agrega un párrafo dentro del `<body>`:

```html
<p>Bienvenido a mi sitio web</p>
```

---

### 8. Ver diferencias

Ver qué cambió en el archivo.

```bash
# Tu código aquí
```

---

### 9. Hacer segundo commit

Agrega el cambio y haz un commit con mensaje "feat: agregar párrafo de bienvenida".

```bash
# Tus comandos aquí
```

---

### 10. Ver historial completo

Muestra el historial de commits en una línea.

```bash
# Tu código aquí
```

---

## Verificación

Al finalizar deberías tener:

- ✅ Un repositorio Git inicializado
- ✅ Un archivo `index.html`
- ✅ Dos commits en el historial
- ✅ Mensajes de commit claros y descriptivos

Verifica con:

```bash
git log --oneline
```

Deberías ver algo como:

```
abc123d feat: agregar párrafo de bienvenida
def456g feat: agregar página de inicio
```

---

## Preguntas de Reflexión

1. ¿Qué diferencia hay entre working directory y staging area?
2. ¿Por qué es importante escribir buenos mensajes de commit?
3. ¿Qué pasaría si no hicieras `git add` antes de `git commit`?

---

## Desafío Extra

Agrega un archivo `styles.css` con:

```css
body {
  font-family: Arial, sans-serif;
  margin: 20px;
}
```

Haz un commit con mensaje apropiado.

---

**Siguiente:** [Ejercicio 2 - Trabajar con Ramas →](exercise-02.md)  
**Solución:** [Ver solución →](solutions/solution-01.md)

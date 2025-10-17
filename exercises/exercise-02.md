# Ejercicio 2: Trabajar con Ramas

**Nivel:** Básico-Intermedio  
**Tiempo estimado:** 20 minutos  
**Objetivos:**

- Crear y cambiar entre ramas
- Fusionar ramas
- Resolver conflictos básicos

---

## Contexto

Vas a agregar una nueva funcionalidad a tu sitio web: una página "Acerca de". Usarás una rama para desarrollar esta feature sin afectar la rama principal.

---

## Tareas

### 1. Ver ramas actuales

Lista todas las ramas en tu repositorio.

```bash
# Tu código aquí
```

**Pregunta:** ¿Cuántas ramas tienes? ¿Cuál está activa?

---

### 2. Crear una nueva rama

Crea una rama llamada `feature/pagina-acerca-de`.

```bash
# Tu código aquí
```

---

### 3. Cambiar a la nueva rama

Cambia a la rama que acabas de crear.

```bash
# Tu código aquí
```

**Pista:** Usa `git checkout` o `git switch`

---

### 4. Crear archivo about.html

Crea un archivo `about.html` con:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Acerca de</title>
  </head>
  <body>
    <h1>Acerca de Mí</h1>
    <p>Soy un desarrollador aprendiendo Git</p>
  </body>
</html>
```

---

### 5. Hacer commit en la rama

Agrega y commitea el nuevo archivo.

```bash
# Tus comandos aquí
```

**Mensaje sugerido:** "feat: agregar página acerca de"

---

### 6. Volver a main

Cambia de vuelta a la rama main.

```bash
# Tu código aquí
```

**Pregunta:** ¿Qué pasó con el archivo about.html?

---

### 7. Fusionar la rama

Fusiona la rama `feature/pagina-acerca-de` en main.

```bash
# Tu código aquí
```

---

### 8. Verificar la fusión

Lista los archivos y verifica el historial.

```bash
# Tus comandos aquí
```

---

### 9. Eliminar la rama

Elimina la rama `feature/pagina-acerca-de` ya que fue fusionada.

```bash
# Tu código aquí
```

---

### 10. Crear conflicto (simulación)

Vamos a crear un conflicto intencionalmente:

**Paso A:** Modifica `index.html` en main, cambia el h1 a:

```html
<h1>¡Hola Mundo desde Main!</h1>
```

Haz commit.

**Paso B:** Crea rama `feature/nuevo-titulo`, cámbiate a ella.

**Paso C:** Modifica el mismo h1 a:

```html
<h1>Bienvenido a Mi Sitio</h1>
```

Haz commit.

**Paso D:** Vuelve a main e intenta fusionar.

```bash
# Tus comandos aquí
```

**Pregunta:** ¿Qué mensaje recibes?

---

### 11. Resolver el conflicto

Abre `index.html` y verás algo como:

```html
<<<<<<< HEAD
<h1>¡Hola Mundo desde Main!</h1>
=======
<h1>Bienvenido a Mi Sitio</h1>
>>>>>>> feature/nuevo-titulo
```

Edita el archivo para quedarte con:

```html
<h1>Bienvenido a Mi Sitio Web</h1>
```

Luego:

```bash
# Marca como resuelto y completa el merge
```

---

## Verificación

Al finalizar deberías tener:

- ✅ Varias ramas creadas y fusionadas
- ✅ Un conflicto resuelto
- ✅ Archivos index.html y about.html en main
- ✅ Historial limpio

Verifica con:

```bash
git log --oneline --graph --all
```

---

## Preguntas de Reflexión

1. ¿Cuándo es útil trabajar con ramas?
2. ¿Qué es un merge conflict y por qué ocurre?
3. ¿Cuál es la diferencia entre `git merge` y `git rebase`?

---

## Desafío Extra

Crea una rama `feature/navegacion`, agrega un menú de navegación en ambas páginas HTML y fusiona los cambios.

---

**Anterior:** [← Ejercicio 1](exercise-01.md)  
**Siguiente:** [Ejercicio 3 - Git Remoto →](exercise-03.md)  
**Solución:** [Ver solución →](solutions/solution-02.md)

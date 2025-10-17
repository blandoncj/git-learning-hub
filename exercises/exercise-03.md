# Ejercicio 3: Trabajar con Repositorios Remotos

**Nivel:** Intermedio  
**Tiempo estimado:** 25 minutos  
**Objetivos:**

- Conectar con GitHub/GitLab
- Push y pull de cambios
- Clonar repositorios

---

## Contexto

Vas a conectar tu repositorio local con GitHub para colaborar y respaldar tu código.

---

## Tareas

### 1. Crear repositorio en GitHub

Ve a GitHub y crea un nuevo repositorio llamado "mi-sitio-web" (sin inicializar con README).

**Pregunta:** ¿Por qué no inicializamos con README?

---

### 2. Conectar repositorio local

Agrega el remoto a tu repositorio local.

```bash
# Tu código aquí
```

**Pista:** Usa `git remote add origin <URL>`

---

### 3. Verificar remotos

Lista los remotos configurados.

```bash
# Tu código aquí
```

---

### 4. Enviar cambios al remoto

Envía tu rama main al repositorio remoto.

```bash
# Tu código aquí
```

**Pista:** Usa `git push -u origin main`

---

### 5. Verificar en GitHub

Abre tu navegador y verifica que los archivos estén en GitHub.

---

### 6. Hacer cambio remoto

En GitHub, edita directamente `index.html` y agrega:

```html
<footer>© 2025 Mi Sitio Web</footer>
```

Haz commit directamente en GitHub.

---

### 7. Traer cambios remotos

En tu terminal, trae los cambios del remoto.

```bash
# Tu código aquí
```

**Pista:** Usa `git pull`

---

### 8. Crear nueva rama y push

Crea una rama `feature/contacto`, agrega un archivo `contact.html`:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Contacto</title>
  </head>
  <body>
    <h1>Contáctame</h1>
    <p>Email: ejemplo@email.com</p>
  </body>
</html>
```

Haz commit y push de esta rama.

```bash
# Tus comandos aquí
```

---

### 9. Crear Pull Request

Ve a GitHub y crea un Pull Request de `feature/contacto` a `main`.

**Pregunta:** ¿Qué ventajas tiene usar Pull Requests?

---

### 10. Simular colaboración

**Escenario:** Un colaborador clona tu repositorio.

En otra carpeta, clona tu repositorio:

```bash
# Tu código aquí
```

Haz un cambio, commit y push. Luego vuelve a tu carpeta original y haz pull.

---

## Verificación

Al finalizar deberías tener:

- ✅ Repositorio en GitHub
- ✅ Conexión entre local y remoto
- ✅ Varios push y pull exitosos
- ✅ Al menos un Pull Request

Verifica con:

```bash
git remote -v
git log --oneline --graph --all
```

---

## Preguntas de Reflexión

1. ¿Qué diferencia hay entre `git fetch` y `git pull`?
2. ¿Por qué es importante hacer pull antes de push?
3. ¿Qué es un Pull Request y para qué sirve?

---

## Desafío Extra

Configura GitHub Actions para hacer un deploy automático a GitHub Pages.

---

**Anterior:** [← Ejercicio 2](exercise-02.md)  
**Siguiente:** [Ejercicio 4 - Deshacer Cambios →](exercise-04.md)  
**Solución:** [Ver solución →](solutions/solution-03.md)

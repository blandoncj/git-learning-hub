# Ejercicio 5: Stash y Tags

**Nivel:** Intermedio  
**Tiempo estimado:** 20 minutos  
**Objetivos:**

- Guardar trabajo temporal con stash
- Crear y gestionar tags
- Entender versionado semántico

---

## Contexto

Aprenderás a guardar cambios temporales cuando necesites cambiar de contexto rápidamente y a etiquetar versiones importantes de tu proyecto.

---

## Tareas

### 1. Preparación

Asegúrate de estar en main con un working directory limpio.

```bash
git status
```

---

### 2. Hacer cambios sin commitear

Modifica `index.html`:

```html
<p>Trabajo en progreso - nueva funcionalidad</p>
```

Crea un archivo nuevo `temp.html`:

```html
<!DOCTYPE html>
<html>
  <body>
    <h1>Archivo temporal</h1>
  </body>
</html>
```

Verifica el estado:

```bash
git status
```

---

### 3. Usar stash básico

Imagina que necesitas cambiar urgentemente a otra rama. Guarda tus cambios temporalmente:

```bash
# Tu código aquí
```

**Pista:** Usa `git stash` o `git stash push`

Verifica que el working directory esté limpio:

```bash
git status
```

**Pregunta:** ¿Qué pasó con tus cambios?

---

### 4. Ver stash list

Lista todos los stashes guardados:

```bash
# Tu código aquí
```

---

### 5. Aplicar stash

Recupera tus cambios guardados:

```bash
# Tu código aquí
```

**Pista:** Usa `git stash pop` o `git stash apply`

**Pregunta:** ¿Cuál es la diferencia entre pop y apply?

---

### 6. Stash con mensaje

Haz otros cambios en `about.html`:

```html
<p>Otro cambio temporal</p>
```

Guarda el stash con un mensaje descriptivo:

```bash
# Tu código aquí
```

**Pista:** `git stash push -m "descripción"`

---

### 7. Ver contenido del stash

Mira qué hay en un stash sin aplicarlo:

```bash
# Tu código aquí
```

**Pista:** `git stash show -p stash@{0}`

---

### 8. Stash incluyendo archivos untracked

Crea un archivo nuevo sin agregar al staging:

```bash
echo "test" > nuevo.txt
```

Guarda el stash incluyendo archivos no rastreados:

```bash
# Tu código aquí
```

**Pista:** Usa flag `-u` o `--include-untracked`

---

### 9. Limpiar stashes

Elimina un stash específico:

```bash
# Tu código aquí
```

**Pista:** `git stash drop stash@{n}`

Elimina todos los stashes:

```bash
# Tu código aquí (NO lo ejecutes aún)
```

---

### 10. Crear tu primer tag

Haz commit de tus cambios actuales:

```bash
git add .
git commit -m "feat: completar funcionalidades básicas"
```

Crea un tag para marcar la versión 1.0.0:

```bash
# Tu código aquí
```

**Pista:** `git tag -a v1.0.0 -m "mensaje"`

---

### 11. Listar tags

Muestra todos los tags:

```bash
# Tu código aquí
```

---

### 12. Ver información del tag

Muestra la información detallada del tag:

```bash
# Tu código aquí
```

**Pista:** `git show v1.0.0`

---

### 13. Crear tag ligero

Haz otro commit pequeño:

```bash
echo "<!-- v1.0.1 -->" >> index.html
git add index.html
git commit -m "fix: corrección menor"
```

Crea un tag ligero (sin mensaje):

```bash
# Tu código aquí
```

**Pista:** `git tag v1.0.1` (sin -a)

**Pregunta:** ¿Cuál es la diferencia entre tag anotado y ligero?

---

### 14. Push de tags

Envía tus tags al repositorio remoto:

```bash
# Tu código aquí
```

**Pista:** `git push origin --tags` o `git push origin v1.0.0`

---

### 15. Checkout a un tag

Crea una rama desde un tag:

```bash
# Tu código aquí
```

**Pista:** `git checkout -b rama-desde-tag v1.0.0`

---

## Verificación

Al finalizar deberías tener:

- ✅ Experiencia usando stash para cambios temporales
- ✅ Varios tags creados (anotados y ligeros)
- ✅ Tags pusheados al remoto
- ✅ Comprensión de cuándo usar cada herramienta

Verifica con:

```bash
git stash list
git tag
git log --oneline --decorate
```

---

## Preguntas de Reflexión

1. ¿Cuándo es útil usar stash en lugar de hacer un commit?
2. ¿Qué es el versionado semántico (SemVer)?
3. ¿Por qué usarías tags en lugar de solo recordar hashes de commits?

---

## Desafío Extra

1. Crea un script que automáticamente cree un tag incrementando la versión
2. Investiga `git stash branch` y crea una rama desde un stash

---

**Anterior:** [← Ejercicio 4](exercise-04.md)  
**Siguiente:** [Ejercicio 6 - Rebase Interactivo →](exercise-06.md)  
**Solución:** [Ver solución →](solutions/solution-05.md)

# Ejercicio 4: Deshacer Cambios

**Nivel:** Intermedio  
**Tiempo estimado:** 20 minutos  
**Objetivos:**

- Usar git restore y git reset
- Revertir commits
- Entender la diferencia entre reset y revert

---

## Contexto

Aprenderás diferentes formas de deshacer cambios en Git, desde el working directory hasta commits ya hechos.

---

## Tareas

### 1. Preparación

Asegúrate de estar en main con el último commit.

```bash
git status
```

---

### 2. Deshacer cambios en working directory

Modifica `index.html` agregando un error intencional:

```html
<h1>ESTO ES UN ERROR</h1>
```

**NO hagas add.** Ahora deshaz este cambio:

```bash
# Tu código aquí
```

**Pista:** Usa `git restore` o `git checkout`

---

### 3. Deshacer cambios en staging area

Modifica `about.html` y haz `git add`.

Ahora saca el archivo del staging area (sin perder cambios):

```bash
# Tu código aquí
```

**Pregunta:** ¿Qué comando usaste?

---

### 4. Hacer commits para practicar

Crea tres commits rápidos:

```bash
echo "<!-- Commit 1 -->" >> index.html
git add . && git commit -m "commit 1"

echo "<!-- Commit 2 -->" >> index.html
git add . && git commit -m "commit 2"

echo "<!-- Commit 3 -->" >> index.html
git add . && git commit -m "commit 3"
```

---

### 5. Ver historial

Muestra el log con hashes cortos.

```bash
# Tu código aquí
```

---

### 6. Reset suave

Deshaz el último commit pero mantén los cambios en staging:

```bash
# Tu código aquí
```

**Pista:** `git reset --soft HEAD~1`

Verifica el estado. **Pregunta:** ¿Dónde están los cambios?

---

### 7. Reset mixto (default)

Haz commit nuevamente. Luego deshaz el commit dejando cambios en working directory:

```bash
# Tu código aquí
```

**Pregunta:** ¿Qué diferencia hay con --soft?

---

### 8. Reset duro (CUIDADO)

**ADVERTENCIA:** Esto borra cambios permanentemente.

Haz un cambio en `index.html`, agrégalo y commitea.

Ahora elimina ese commit Y los cambios:

```bash
# Tu código aquí
```

**Pista:** `git reset --hard HEAD~1`

---

### 9. Revertir un commit (método seguro)

Haz un commit con un error:

```bash
echo "<h1>ERROR</h1>" > error.html
git add error.html
git commit -m "agregar archivo con error"
```

Ahora revierte este commit creando un nuevo commit:

```bash
# Tu código aquí
```

**Pista:** Usa `git revert`

---

### 10. Recuperar commits perdidos

Si hiciste reset --hard, puedes recuperar commits con reflog:

```bash
git reflog
```

Encuentra el hash del commit perdido y recupéralo:

```bash
git reset --hard <hash>
```

---

## Verificación

Al finalizar deberías entender:

- ✅ Diferencia entre restore, reset y revert
- ✅ Cuándo usar cada comando
- ✅ Cómo recuperar commits perdidos

---

## Preguntas de Reflexión

1. ¿Cuándo usarías `git reset` vs `git revert`?
2. ¿Por qué `git reset --hard` es peligroso?
3. ¿Qué es reflog y para qué sirve?

---

## Desafío Extra

Investiga `git commit --amend` y úsalo para modificar el último commit.

---

**Anterior:** [← Ejercicio 3](exercise-03.md)  
**Siguiente:** [Ejercicio 5 - Stash y Tags →](exercise-05.md)  
**Solución:** [Ver solución →](solutions/solution-04.md)

# Ejercicio 6: Rebase Interactivo y Historia Limpia

**Nivel:** Avanzado  
**Tiempo estimado:** 30 minutos  
**Objetivos:**

- Usar rebase interactivo
- Reordenar, combinar y editar commits
- Mantener una historia limpia

---

## Contexto

Aprenderás a limpiar y organizar tu historial de commits antes de hacer merge a la rama principal, una práctica común en equipos profesionales.

---

## Tareas

### 1. Preparación

Crea una nueva rama para practicar:

```bash
git checkout -b feature/rebase-practice
```

---

### 2. Crear varios commits "sucios"

Haz varios commits pequeños e imperfectos:

```bash
echo "Línea 1" >> test.txt
git add test.txt
git commit -m "agregar test"

echo "Línea 2" >> test.txt
git add test.txt
git commit -m "fix typo"

echo "Línea 3" >> test.txt
git add test.txt
git commit -m "WIP"

echo "Línea 4" >> test.txt
git add test.txt
git commit -m "agregar mas cosas"

echo "Línea 5" >> test.txt
git add test.txt
git commit -m "fix: corregir bug"
```

Ver el historial:

```bash
git log --oneline
```

**Pregunta:** ¿Qué problemas ves con estos commits?

---

### 3. Iniciar rebase interactivo

Vamos a limpiar los últimos 5 commits:

```bash
# Tu código aquí
```

**Pista:** `git rebase -i HEAD~5`

---

### 4. Combinar commits (squash)

Se abrirá un editor con algo como:

```
pick abc1234 agregar test
pick def5678 fix typo
pick ghi9012 WIP
pick jkl3456 agregar mas cosas
pick mno7890 fix: corregir bug
```

Cambia a:

```
pick abc1234 agregar test
squash def5678 fix typo
squash ghi9012 WIP
squash jkl3456 agregar mas cosas
pick mno7890 fix: corregir bug
```

**Pregunta:** ¿Qué hace el comando `squash`?

Guarda y cierra el editor.

---

### 5. Editar mensaje del commit combinado

Se abrirá otro editor para el mensaje. Cámbialo a:

```
feat: agregar archivo de prueba con contenido inicial
```

Guarda y cierra.

---

### 6. Verificar resultado

Ver el nuevo historial:

```bash
git log --oneline
```

**Pregunta:** ¿Cuántos commits tienes ahora?

---

### 7. Reordenar commits

Crea más commits:

```bash
echo "Feature A" > feature-a.txt
git add feature-a.txt
git commit -m "feat: implementar feature A"

echo "Bugfix" > bugfix.txt
git add bugfix.txt
git commit -m "fix: arreglar bug crítico"

echo "Feature B" > feature-b.txt
git add feature-b.txt
git commit -m "feat: implementar feature B"
```

Inicia rebase para reordenar (últimos 3 commits):

```bash
# Tu código aquí
```

Reordena poniendo el bugfix primero:

```
pick [hash] fix: arreglar bug crítico
pick [hash] feat: implementar feature A
pick [hash] feat: implementar feature B
```

---

### 8. Editar un commit específico

Vamos a editar el contenido de un commit existente.

Inicia rebase:

```bash
git rebase -i HEAD~3
```

Cambia `pick` a `edit` en el commit que quieres modificar:

```
edit [hash] feat: implementar feature A
pick [hash] fix: arreglar bug crítico
pick [hash] feat: implementar feature B
```

Git se detendrá en ese commit. Modifica el archivo:

```bash
echo "Feature A - mejorada" > feature-a.txt
git add feature-a.txt
git commit --amend -m "feat: implementar feature A (mejorada)"
git rebase --continue
```

---

### 9. Dividir un commit

Crea un commit grande:

```bash
echo "Parte 1" > multi.txt
echo "Parte 2" >> multi.txt
git add multi.txt
git commit -m "agregar multiples cambios"
```

Rebase y marca como edit:

```bash
git rebase -i HEAD~1
# Cambia pick a edit
```

Deshaz el commit manteniendo cambios:

```bash
git reset HEAD~1
```

Haz commits separados:

```bash
echo "Parte 1" > multi.txt
git add multi.txt
git commit -m "feat: agregar parte 1"

echo "Parte 2" >> multi.txt
git add multi.txt
git commit -m "feat: agregar parte 2"

git rebase --continue
```

---

### 10. Usar fixup

Crea un commit y luego un "arreglo":

```bash
echo "Primera versión" > documento.txt
git add documento.txt
git commit -m "docs: agregar documentación"

echo "Corrección de typo" >> documento.txt
git add documento.txt
git commit -m "fix: typo en documentación"
```

Rebase usando fixup:

```bash
git rebase -i HEAD~2
```

Cambia el segundo commit a fixup:

```
pick [hash] docs: agregar documentación
fixup [hash] fix: typo en documentación
```

**Pregunta:** ¿Qué diferencia hay entre squash y fixup?

---

### 11. Rebase sobre main

Vuelve a main y haz un commit:

```bash
git checkout main
echo "Cambio en main" >> index.html
git add index.html
git commit -m "feat: actualizar index en main"
```

Vuelve a tu rama feature:

```bash
git checkout feature/rebase-practice
```

Actualiza tu rama con los cambios de main usando rebase:

```bash
# Tu código aquí
```

**Pista:** `git rebase main`

**Pregunta:** ¿Qué diferencia hay entre merge y rebase?

---

### 12. Abortar un rebase

Si algo sale mal durante un rebase:

```bash
git rebase --abort
```

**Pregunta:** ¿Cuándo usarías --abort?

---

## Verificación

Al finalizar deberías tener:

- ✅ Historia de commits limpia y organizada
- ✅ Commits combinados apropiadamente
- ✅ Experiencia resolviendo conflictos en rebase
- ✅ Comprensión de cuándo usar rebase vs merge

Verifica con:

```bash
git log --oneline --graph
```

---

## Preguntas de Reflexión

1. ¿Por qué es importante mantener una historia de commits limpia?
2. ¿Cuándo NO deberías usar rebase?
3. ¿Qué significa "nunca hagas rebase de commits públicos"?

---

## Desafío Extra

1. Investiga `git rebase --autosquash` con commits que empiecen con `fixup!`
2. Configura un alias para `git rebase -i` llamado `git cleanup`

---

**Anterior:** [← Ejercicio 5](exercise-05.md)  
**Siguiente:** [Ejercicio 7 - Cherry Pick y Workflows →](exercise-07.md)  
**Solución:** [Ver solución →](solutions/solution-06.md)

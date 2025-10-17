# Conflictos de Merge

Guía completa para entender, prevenir y resolver conflictos de merge en Git.

## Tabla de Contenidos

1. [¿Qué es un Conflicto?](#qué-es-un-conflicto)
2. [Detectar Conflictos](#detectar-conflictos)
3. [Resolver Conflictos](#resolver-conflictos)
4. [Herramientas para Resolver](#herramientas-para-resolver)
5. [Prevenir Conflictos](#prevenir-conflictos)
6. [Casos Especiales](#casos-especiales)

---

## ¿Qué es un Conflicto?

Un **conflicto de merge** ocurre cuando Git no puede fusionar automáticamente dos cambios porque modifican las mismas líneas de código de forma diferente.

### Ejemplo Visual

```
ANTES (rama main):
función sumar(a, b) {
  return a + b;
}

Tu cambio (rama feature):
función sumar(a, b) {
  return a + b + 1;  // Agregaste +1
}

Cambio de otro (también en main):
función sumar(a, b) {
  return a + b - 1;  // Agregó -1
}

RESULTADO: ¡CONFLICTO!
Git no sabe cuál versión mantener.
```

---

## Detectar Conflictos

### Durante Merge

```bash
git merge feature-rama
# CONFLICT (content): Merge conflict in archivo.js
# Automatic merge failed; fix conflicts and then commit the result.
```

### Durante Pull

```bash
git pull origin main
# CONFLICT (content): Merge conflict in archivo.js
```

### Durante Rebase

```bash
git rebase main
# CONFLICT (content): Merge conflict in archivo.js
# Could not apply abc123d... commit message
```

### Ver Archivos en Conflicto

```bash
# Ver estado
git status

# Salida mostrará:
# Unmerged paths:
#   both modified:   archivo.js
```

---

## Resolver Conflictos

### Paso 1: Identificar Archivos

```bash
git status

# O ver solo archivos en conflicto
git diff --name-only --diff-filter=U
```

### Paso 2: Abrir Archivo en Conflicto

Los conflictos se marcan así:

```javascript
function sumar(a, b) {
<<<<<<< HEAD
  return a + b + 1;  // Tu versión
=======
  return a + b - 1;  // Versión de otro
>>>>>>> feature-rama
}
```

**Partes del marcador:**

- `<<<<<<< HEAD` - Inicio de tu versión (rama actual)
- `=======` - Separador
- `>>>>>>> rama-nombre` - Fin de la otra versión

### Paso 3: Decidir Qué Mantener

**Opción 1: Mantener tu cambio**

```javascript
function sumar(a, b) {
  return a + b + 1;
}
```

**Opción 2: Mantener el otro cambio**

```javascript
function sumar(a, b) {
  return a + b - 1;
}
```

**Opción 3: Combinar ambos (si tiene sentido)**

```javascript
function sumar(a, b) {
  // Implementación combinada
  return a + b;
}
```

**Opción 4: Escribir algo completamente nuevo**

```javascript
function sumar(a, b) {
  // Nueva implementación después de discusión
  return (a + b) * 2;
}
```

### Paso 4: Eliminar Marcadores

Asegúrate de eliminar:

- `<<<<<<< HEAD`
- `=======`
- `>>>>>>> rama-nombre`

### Paso 5: Guardar y Marcar como Resuelto

```bash
# Guardar archivo

# Marcar como resuelto
git add archivo.js

# Verificar estado
git status
```

### Paso 6: Completar el Merge

```bash
# Hacer commit
git commit -m "Resolver conflicto de merge en archivo.js"

# O si es rebase
git rebase --continue
```

---

## Herramientas para Resolver

### Visual Studio Code

**Ventajas:**

- Interfaz visual
- Botones para aceptar cambios
- Vista previa lado a lado

**Uso:**

1. Abre archivo en conflicto
2. VSCode muestra botones:
   - "Accept Current Change" (tu versión)
   - "Accept Incoming Change" (otra versión)
   - "Accept Both Changes" (ambas)
   - "Compare Changes" (comparar)

### Git Mergetool

```bash
# Configurar herramienta (primera vez)
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Usar mergetool
git mergetool

# Esto abre tu editor configurado con el conflicto
```

### Herramientas Recomendadas

**VSCode:**

```bash
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

**KDiff3:**

```bash
git config --global merge.tool kdiff3
```

**Meld:**

```bash
git config --global merge.tool meld
```

**P4Merge:**

```bash
git config --global merge.tool p4merge
```

---

## Estrategias de Resolución

### Aceptar Toda tu Versión

```bash
# Durante merge, usar estrategia "ours"
git merge -X ours otra-rama
```

### Aceptar Toda la Otra Versión

```bash
# Durante merge, usar estrategia "theirs"
git merge -X theirs otra-rama
```

### Abortar Merge

Si decides no continuar:

```bash
# Abortar merge
git merge --abort

# Abortar rebase
git rebase --abort
```

---

## Prevenir Conflictos

### 1. Comunicación del Equipo

✅ **Coordinar cambios en archivos**
✅ **Avisar antes de modificar archivos críticos**
✅ **Usar sistema de asignación de tareas**

### 2. Commits Pequeños y Frecuentes

```bash
# MAL: Un commit gigante
git add .
git commit -m "Cambios grandes"

# BIEN: Commits atómicos
git add archivo1.js
git commit -m "feat: agregar función X"
git add archivo2.js
git commit -m "fix: corregir bug Y"
```

### 3. Actualizar Frecuentemente

```bash
# Cada mañana
git checkout main
git pull origin main
git checkout tu-rama
git merge main  # o git rebase main

# Resolver conflictos temprano
```

### 4. Usar Ramas de Corta Duración

```bash
# MAL: Rama que dura semanas
# (acumula muchos conflictos)

# BIEN: Ramas que duran días
# (menos conflictos, más fáciles de resolver)
```

### 5. Dividir Archivos Grandes

```javascript
// MAL: Todo en un archivo
// archivo-gigante.js (1000 líneas)

// BIEN: Dividir en módulos
// modulo-a.js
// modulo-b.js
// modulo-c.js
```

### 6. Code Ownership

- Asignar "dueños" a módulos específicos
- Pedir permiso antes de cambiar código de otros
- Usar CODEOWNERS en GitHub

---

## Casos Especiales

### Conflicto en package.json

**Problema:** Dos personas agregaron dependencias.

**Solución:**

```json
<<<<<<< HEAD
{
  "dependencies": {
    "axios": "^1.0.0",
    "lodash": "^4.17.21"
=======
{
  "dependencies": {
    "axios": "^1.0.0",
    "moment": "^2.29.0"
>>>>>>> feature
  }
}
```

**Resolver combinando:**

```json
{
  "dependencies": {
    "axios": "^1.0.0",
    "lodash": "^4.17.21",
    "moment": "^2.29.0"
  }
}
```

**Luego reinstalar:**

```bash
npm install
git add package.json package-lock.json
git commit -m "Resolver conflicto en dependencias"
```

---

### Conflicto en Archivo Binario

**Problema:** No puedes resolver conflictos en imágenes, PDFs, etc.

**Solución:**

```bash
# Mantener tu versión
git checkout --ours imagen.png
git add imagen.png

# Mantener la otra versión
git checkout --theirs imagen.png
git add imagen.png

# Commit
git commit -m "Resolver conflicto en imagen"
```

---

### Múltiples Archivos en Conflicto

```bash
# Ver todos los conflictos
git diff --name-only --diff-filter=U

# Resolver uno por uno
# ... editar archivo1 ...
git add archivo1.js

# ... editar archivo2 ...
git add archivo2.js

# Cuando todos estén resueltos
git commit
```

---

### Conflicto Durante Rebase

```bash
git rebase main
# CONFLICT in archivo.js

# 1. Resolver conflicto
# 2. Marcar como resuelto
git add archivo.js

# 3. Continuar rebase
git rebase --continue

# Si hay más conflictos, repetir
# Si quieres abortar
git rebase --abort
```

---

## Comandos Útiles

```bash
# Ver archivos en conflicto
git diff --name-only --diff-filter=U

# Ver diferencias completas
git diff

# Ver solo conflictos
git diff --check

# Aceptar toda tu versión
git checkout --ours archivo.js
git add archivo.js

# Aceptar toda la otra versión
git checkout --theirs archivo.js
git add archivo.js

# Ver historial de merges
git log --merge

# Ver qué causó el conflicto
git log --oneline --left-right HEAD...MERGE_HEAD
```

---

## Checklist de Resolución

- [ ] Identificar todos los archivos en conflicto
- [ ] Abrir cada archivo
- [ ] Entender ambos cambios
- [ ] Decidir qué mantener
- [ ] Eliminar todos los marcadores (`<<<<`, `====`, `>>>>`)
- [ ] Probar que el código funciona
- [ ] Hacer `git add` de cada archivo
- [ ] Verificar con `git status`
- [ ] Hacer commit o continuar rebase
- [ ] Hacer push
- [ ] Avisar al equipo si es necesario

---

## Recursos Adicionales

- 📖 **Flujo de Trabajo:** [/guides/es/flujo-de-trabajo.md](/guides/es/flujo-de-trabajo.md)
- 🔀 **Rebase vs Merge:** [/guides/es/flujo-de-trabajo.md#rebase-vs-merge](/guides/es/flujo-de-trabajo.md#rebase-vs-merge)
- 🛠️ **Problemas Comunes:** [problemas-comunes.md](problemas-comunes.md)

---

**¿Resolviste el conflicto?** Comparte tu experiencia o pide ayuda con casos complejos.

# Solución - Ejercicio 3: Trabajar con Repositorios Remotos

## Comandos Completos

### 1. Crear repositorio en GitHub

**Pasos en la interfaz web:**

1. Ir a https://github.com
2. Click en "+" → "New repository"
3. Nombre: `mi-sitio-web`
4. **NO** marcar "Initialize with README"
5. **NO** agregar .gitignore ni licencia
6. Click "Create repository"

**Respuesta a la pregunta:**
No inicializamos con README porque ya tenemos un repositorio local con commits. Si GitHub crea commits automáticamente, tendremos historias divergentes y conflictos al conectarlos.

**Alternativa si ya tiene README:**

```bash
git pull origin main --allow-unrelated-histories
```

---

### 2. Conectar repositorio local

```bash
# Agregar remoto (sustituye con tu URL)
git remote add origin https://github.com/tu-usuario/mi-sitio-web.git

# Alternativa con SSH
git remote add origin git@github.com:tu-usuario/mi-sitio-web.git
```

**Verificar URL:**

```bash
git remote get-url origin
```

---

### 3. Verificar remotos

```bash
git remote -v
```

**Salida esperada:**

```
origin  https://github.com/tu-usuario/mi-sitio-web.git (fetch)
origin  https://github.com/tu-usuario/mi-sitio-web.git (push)
```

**Comandos relacionados:**

```bash
git remote                    # Solo nombres
git remote show origin        # Información detallada
git remote -v                 # Con URLs
```

---

### 4. Enviar cambios al remoto

```bash
# Primera vez (establece tracking)
git push -u origin main

# O si tu rama principal es 'master'
git push -u origin master
```

**Salida esperada:**

```
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 8 threads
Compressing objects: 100% (8/8), done.
Writing objects: 100% (10/10), 1.23 KiB | 1.23 MiB/s, done.
Total 10 (delta 2), reused 0 (delta 0)
To https://github.com/tu-usuario/mi-sitio-web.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

**Explicación del flag `-u`:**

- Establece upstream tracking
- Futuras veces solo necesitas `git push`
- Vincula rama local con rama remota

**Pushes subsecuentes:**

```bash
git push  # Ya no necesitas -u origin main
```

---

### 5. Verificar en GitHub

1. Ir a https://github.com/tu-usuario/mi-sitio-web
2. Deberías ver:
   - index.html
   - about.html
   - styles.css
   - Historial de commits

---

### 6. Hacer cambio remoto

**En la interfaz de GitHub:**

1. Click en `index.html`
2. Click en el ícono de lápiz (Edit)
3. Agregar antes de `</body>`:

```html
<footer>© 2025 Mi Sitio Web</footer>
```

4. Scroll down → Commit message: "feat: agregar footer"
5. Click "Commit changes"

---

### 7. Traer cambios remotos

```bash
git pull origin main
# O simplemente (si ya configuraste upstream)
git pull
```

**Salida esperada:**

```
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://github.com/tu-usuario/mi-sitio-web
   abc1234..def5678  main       -> origin/main
Updating abc1234..def5678
Fast-forward
 index.html | 1 +
 1 file changed, 1 insertion(+)
```

**Verificar:**

```bash
cat index.html  # Deberías ver el footer
git log --oneline -1  # Ver último commit
```

---

### 8. Crear nueva rama y push

```bash
# Crear rama
git checkout -b feature/contacto

# Crear archivo
cat > contact.html << 'EOF'
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
EOF

# Commitear
git add contact.html
git commit -m "feat: agregar página de contacto"

# Push de la nueva rama
git push -u origin feature/contacto
```

**Salida esperada:**

```
To https://github.com/tu-usuario/mi-sitio-web.git
 * [new branch]      feature/contacto -> feature/contacto
Branch 'feature/contacto' set up to track remote branch 'feature/contacto' from 'origin'.
```

---

### 9. Crear Pull Request

**En GitHub:**

1. Verás banner: "feature/contacto had recent pushes"
2. Click "Compare & pull request"
3. Título: "Agregar página de contacto"
4. Descripción:

```markdown
## Descripción

Agrega una página de contacto con información básica.

## Cambios

- Nuevo archivo contact.html
- Formulario de contacto simple

## Testing

- [ ] Verificar que la página carga correctamente
- [ ] Validar HTML
```

5. Click "Create pull request"

**Respuesta a la pregunta - Ventajas de Pull Requests:**

1. **Code Review:** Otros pueden revisar antes de merge
2. **Discusión:** Comentarios y sugerencias en línea
3. **CI/CD:** Tests automáticos antes de merge
4. **Documentación:** Historia de por qué se hicieron cambios
5. **Quality Gates:** Requerir aprobaciones
6. **Integración continua:** Verificar que no rompe nada

---

### 10. Simular colaboración

**En otra carpeta (simular otro desarrollador):**

```bash
# Salir de la carpeta actual
cd ..

# Clonar el repositorio
git clone https://github.com/tu-usuario/mi-sitio-web.git mi-sitio-web-collab

# Entrar a la carpeta clonada
cd mi-sitio-web-collab

# Verificar
ls
git log --oneline
git remote -v
```

**Hacer un cambio como "colaborador":**

```bash
# Crear nueva rama
git checkout -b feature/footer-update

# Modificar footer
sed -i 's/© 2025/© 2025 - Todos los derechos reservados/' index.html

# Commitear
git add index.html
git commit -m "feat: mejorar texto del footer"

# Push
git push -u origin feature/footer-update
```

**Crear PR y mergear en GitHub**

**Volver a la carpeta original:**

```bash
cd ../mi-sitio-web

# Traer cambios
git checkout main
git pull

# Ver el cambio del "colaborador"
cat index.html
git log --oneline
```

---

## Verificación Final

```bash
# Ver remotos
git remote -v
# origin  https://github.com/tu-usuario/mi-sitio-web.git (fetch)
# origin  https://github.com/tu-usuario/mi-sitio-web.git (push)

# Ver todas las ramas (locales y remotas)
git branch -a
# * main
#   feature/contacto
#   remotes/origin/HEAD -> origin/main
#   remotes/origin/main
#   remotes/origin/feature/contacto

# Ver historial con ramas remotas
git log --oneline --graph --all --decorate

# Estado sincronizado
git status
# On branch main
# Your branch is up to date with 'origin/main'.
```

---

## Respuestas a Preguntas de Reflexión

### 1. ¿Qué diferencia hay entre git fetch y git pull?

**Git Fetch:**

```bash
git fetch origin
```

- **Descarga** commits, archivos y refs del remoto
- **NO fusiona** nada en tu rama actual
- Actualiza `origin/main` pero no tu `main` local
- Seguro, no cambia tu working directory

**Uso típico:**

```bash
git fetch origin
git log origin/main  # Ver qué hay de nuevo
git merge origin/main  # Decidir si mergear
```

**Git Pull:**

```bash
git pull origin main
# Equivale a:
git fetch origin main
git merge origin/main
```

- **Descarga Y fusiona** automáticamente
- Puede causar conflictos inesperados
- Cambia tu working directory

**Analogía:**

```
fetch = descargar paquete
pull = descargar + desempacar + instalar
```

**Cuándo usar cada uno:**

- `fetch`: Cuando quieres ver cambios primero
- `pull`: Cuando confías en los cambios remotos

---

### 2. ¿Por qué es importante hacer pull antes de push?

**Razones:**

1. **Evitar conflictos:**

```bash
# ❌ Sin pull primero
git push
# Error: Updates were rejected because the remote contains work
```

2. **Mantener historia lineal:**

```bash
# ✅ Con pull primero
git pull
git push  # Funciona sin problemas
```

3. **Colaboración efectiva:**

- Incorporas trabajo de otros antes de publicar el tuyo
- Resuelves conflictos localmente
- No fuerzas a otros a resolver tus conflictos

**Workflow correcto:**

```bash
# 1. Antes de empezar a trabajar
git pull

# 2. Trabajar y commitear
git add .
git commit -m "feat: mi cambio"

# 3. Antes de push
git pull  # Por si alguien pusheó mientras trabajabas

# 4. Push
git push
```

---

### 3. ¿Qué es un Pull Request y para qué sirve?

**Definición:**
Un Pull Request (PR) o Merge Request (MR en GitLab) es una solicitud para fusionar cambios de una rama a otra, generalmente con revisión de código.

**Propósitos principales:**

1. **Code Review:**

```
Developer A escribe código → Crea PR → Developer B revisa → Aprueba/Rechaza
```

2. **Control de calidad:**

- Tests automáticos (CI)
- Análisis de código estático
- Verificación de cobertura

3. **Documentación:**

- Describe QUÉ cambió
- Explica POR QUÉ cambió
- Referencias a issues/tickets

4. **Discusión:**

- Comentarios en líneas específicas
- Sugerencias de mejora
- Aprendizaje en equipo

**Estructura típica de un PR:**

```markdown
## Descripción

Breve resumen del cambio

## Tipo de cambio

- [ ] Bug fix
- [x] Nueva feature
- [ ] Breaking change

## Testing

- [x] Tests unitarios pasan
- [x] Tests de integración pasan
- [ ] Tests manuales realizados

## Screenshots

(si aplica)

## Checklist

- [x] Código sigue style guide
- [x] Comentarios en código complejo
- [x] Documentación actualizada
```

---

## Comandos Avanzados de Remotos

### Múltiples remotos

```bash
# Agregar segundo remoto (ej: fork)
git remote add upstream https://github.com/original/repo.git

# Ver todos los remotos
git remote -v
# origin    https://github.com/tu-usuario/repo.git (fetch)
# origin    https://github.com/tu-usuario/repo.git (push)
# upstream  https://github.com/original/repo.git (fetch)
# upstream  https://github.com/original/repo.git (push)

# Fetch de upstream
git fetch upstream

# Merge de upstream/main a tu main
git checkout main
git merge upstream/main
```

### Cambiar URL del remoto

```bash
# Cambiar de HTTPS a SSH
git remote set-url origin git@github.com:tu-usuario/repo.git

# Verificar
git remote get-url origin
```

### Eliminar/renombrar remotos

```bash
# Renombrar
git remote rename origin github

# Eliminar
git remote remove github

# Agregar nuevamente
git remote add origin <URL>
```

### Push de múltiples ramas

```bash
# Push de rama específica
git push origin feature/nueva

# Push de todas las ramas
git push --all origin

# Push de todos los tags
git push --tags origin

# Push forzado (¡cuidado!)
git push --force origin main  # Sobrescribe historia remota
```

---

## Workflows de Colaboración

### Workflow básico (equipo pequeño)

```bash
# 1. Clonar
git clone <URL>

# 2. Crear rama
git checkout -b feature/mi-feature

# 3. Trabajar
# ... hacer cambios ...
git add .
git commit -m "feat: implementar feature"

# 4. Actualizar con main
git checkout main
git pull
git checkout feature/mi-feature
git rebase main  # o merge main

# 5. Push
git push -u origin feature/mi-feature

# 6. Crear PR en GitHub

# 7. Después del merge, limpiar
git checkout main
git pull
git branch -d feature/mi-feature
git push origin --delete feature/mi-feature
```

### Fork workflow (open source)

```bash
# 1. Fork en GitHub (botón Fork)

# 2. Clonar tu fork
git clone https://github.com/tu-usuario/proyecto.git

# 3. Agregar upstream
git remote add upstream https://github.com/original/proyecto.git

# 4. Crear feature
git checkout -b feature/mejora

# 5. Trabajar y commitear
git add .
git commit -m "feat: agregar mejora"

# 6. Push a tu fork
git push origin feature/mejora

# 7. Crear PR desde tu fork al repo original

# 8. Mantener fork actualizado
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

## Errores Comunes y Soluciones

### Error: "fatal: remote origin already exists"

```bash
# Ver remotos actuales
git remote -v

# Eliminar y agregar nuevamente
git remote remove origin
git remote add origin <nueva-URL>

# O cambiar URL
git remote set-url origin <nueva-URL>
```

### Error: "Updates were rejected"

```bash
# Problema: El remoto tiene commits que no tienes localmente

# Solución 1: Pull primero (recomendado)
git pull origin main
git push origin main

# Solución 2: Pull con rebase
git pull --rebase origin main
git push origin main

# Solución 3: Force push (¡PELIGROSO!)
git push --force origin main  # Solo si estás seguro
```

### Error: "Permission denied (publickey)"

```bash
# Problema: Autenticación SSH falló

# Solución 1: Cambiar a HTTPS
git remote set-url origin https://github.com/usuario/repo.git

# Solución 2: Configurar SSH
ssh-keygen -t ed25519 -C "tu@email.com"
# Agregar ~/.ssh/id_ed25519.pub a GitHub Settings → SSH Keys
```

### Error: "Repository not found"

```bash
# Verificar URL
git remote get-url origin

# Corregir URL
git remote set-url origin https://github.com/usuario-correcto/repo-correcto.git
```

---

## Próximos Pasos

Has completado el Ejercicio 3. Ahora dominas:

- ✅ Conectar repositorios locales y remotos
- ✅ Push y pull de cambios
- ✅ Colaboración con Pull Requests
- ✅ Workflows de equipo

**Continúa con:** [Ejercicio 4 - Deshacer Cambios →](../exercise-04.md)

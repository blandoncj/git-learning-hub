# Preguntas Frecuentes (FAQ)

Respuestas a las preguntas más comunes sobre Git, GitHub, GitLab y Bitbucket.

## Tabla de Contenidos

1. [Preguntas Generales](#preguntas-generales)
2. [Primeros Pasos](#primeros-pasos)
3. [Autenticación](#autenticación)
4. [Ramas](#ramas)
5. [Commits](#commits)
6. [Push y Pull](#push-y-pull)
7. [Merges y Conflictos](#merges-y-conflictos)
8. [Errores Comunes](#errores-comunes)
9. [Seguridad](#seguridad)

---

## Preguntas Generales

### ¿Qué es Git?

Git es un **sistema de control de versiones distribuido** que te permite rastrear cambios en tu código, colaborar con otros desarrolladores y mantener un historial completo de tu proyecto.

**Más información:** [Introducción a Git](/docs/es/introduccion.md)

### ¿Cuál es la diferencia entre Git y GitHub?

- **Git:** Software de control de versiones (funciona en tu computadora)
- **GitHub:** Plataforma online para alojar repositorios Git y colaborar

**Analogía:** Git es como el motor de un auto, GitHub es la gasolinera donde lo cargas.

### ¿Necesito pagar por usar Git?

No, Git es **completamente gratuito** y de código abierto. Lo mismo vale para GitHub (para repositorios públicos), GitLab y Bitbucket.

### ¿Por dónde empiezo?

1. **[Instala Git](/docs/es/instalacion.md)**
2. **[Configura tu usuario](/docs/es/configuracion-inicial.md)**
3. **[Aprende los conceptos básicos](/docs/es/conceptos-basicos.md)**
4. **[Crea tu primer repositorio](/guides/es/flujo-de-trabajo.md#flujo-básico)**

---

## Primeros Pasos

### ¿Cómo creo mi primer repositorio?

```bash
# Opción 1: Repositorio local
git init nombre-proyecto
cd nombre-proyecto

# Opción 2: Clonar uno existente
git clone https://github.com/usuario/repo.git
cd repo
```

**Más información:** [Cheat Sheet Básico](/cheat-sheets/git-basico.md)

### ¿Cómo veo el estado de mi repositorio?

```bash
git status
```

Esto te muestra:

- Archivos modificados
- Archivos sin rastrear
- Cambios en staging area

### ¿Qué es un commit?

Un **commit** es una "fotografía" de tu proyecto en un momento específico. Contiene los cambios realizados, quién los hizo, cuándo y por qué.

**Más información:** [Conceptos Básicos](/docs/es/conceptos-basicos.md#commit)

### ¿Cómo hago mi primer commit?

```bash
# 1. Ver cambios
git status

# 2. Agregar cambios
git add .

# 3. Hacer commit
git commit -m "Descripción clara del cambio"

# 4. Ver historial
git log
```

---

## Autenticación

### ¿SSH o HTTPS? ¿Cuál debo usar?

**Usa SSH si:**

- Eres desarrollador profesional
- Trabajas en múltiples repositorios
- Quieres automatización

**Usa HTTPS si:**

- Eres principiante
- Tu firewall bloquea SSH
- Prefieres no complicarte

**Más información:** [SSH vs HTTPS](/guides/es/autenticacion-ssh-https.md)

### ¿Cómo genero claves SSH?

```bash
ssh-keygen -t ed25519 -C "tu.email@ejemplo.com"
```

Luego agrega la clave pública a GitHub/GitLab/Bitbucket.

**Más información:** [Cheat Sheet SSH/GPG](/cheat-sheets/ssh-gpg-keys.md)

### Me pide contraseña cada vez que hago push

Configura **Git Credentials Manager**:

```bash
# Windows
git config --global credential.helper manager

# macOS
git config --global credential.helper osxkeychain

# Linux
git config --global credential.helper cache
```

### ¿Puedo usar múltiples cuentas de Git?

Sí, puedes configurar múltiples claves SSH en `~/.ssh/config`:

```bash
Host github-personal
    HostName github.com
    IdentityFile ~/.ssh/id_personal

Host github-trabajo
    HostName github.com
    IdentityFile ~/.ssh/id_trabajo
```

**Más información:** [SSH Avanzado](/cheat-sheets/ssh-gpg-keys.md#ssh---configuración-avanzada)

---

## Ramas

### ¿Qué es una rama?

Una **rama** es una línea independiente de desarrollo. Te permite trabajar en nuevas características sin afectar el código principal.

**Analogía:** Como ramificaciones de un árbol, cada una es independiente pero conectada al tronco.

**Más información:** [Gestión de Ramas](/guides/es/gestion-ramas.md)

### ¿Cómo creo una rama?

```bash
# Crear y cambiar
git checkout -b nombre-rama

# O versión moderna
git switch -c nombre-rama
```

### ¿Cuál es la diferencia entre main, master y develop?

- **main/master:** La rama principal, código estable para producción
- **develop:** Rama de desarrollo donde se integran características
- **feature/...:** Ramas temporales para trabajar en nuevas características

**Más información:** [Estrategias de Ramificación](/guides/es/gestion-ramas.md#estrategias-de-ramificación)

### ¿Cómo elimino una rama?

```bash
# Eliminar local
git branch -d nombre-rama

# Eliminar remota
git push origin --delete nombre-rama
```

---

## Commits

### ¿Cómo escribo buenos mensajes de commit?

**Reglas:**

- ✅ Máximo 50 caracteres en la primera línea
- ✅ Usa imperativo ("agregar", no "agregado")
- ✅ Claro y descriptivo
- ✅ En presente

**Ejemplo:**

```
feat: agregar validación de email
```

**Más información:** [Convenciones de Commits](/guides/es/flujo-de-trabajo.md#convenciones-de-commits)

### ¿Puedo cambiar un commit que ya hice?

Sí, con `git commit --amend`:

```bash
# Cambiar el último commit
git commit --amend -m "Nuevo mensaje"

# O cambiar cambios
git add archivo.js
git commit --amend --no-edit
```

### ¿Cómo veo el historial de commits?

```bash
# Ver completo
git log

# Ver en una línea
git log --oneline

# Ver con gráfico de ramas
git log --oneline --graph --all
```

**Más información:** [Cheat Sheet Básico](/cheat-sheets/git-basico.md#historial)

---

## Push y Pull

### ¿Cuál es la diferencia entre push y pull?

- **Push:** Enviar tus cambios locales al repositorio remoto
- **Pull:** Descargar cambios del remoto a tu máquina local

```bash
git push  # Enviar
git pull  # Descargar
```

### ¿Por qué me rechaza el push?

Generalmente porque el remoto tiene cambios que no tienes localmente.

**Solución:**

```bash
# Traer cambios primero
git pull

# Resolver conflictos si hay
# Luego hacer push
git push
```

### ¿Necesito hacer push después de cada commit?

No, puedes hacer varios commits locales y hacer un solo push:

```bash
git commit -m "Cambio 1"
git commit -m "Cambio 2"
git commit -m "Cambio 3"

# Todos juntos
git push
```

---

## Merges y Conflictos

### ¿Qué es un merge?

Un **merge** (fusión) combina cambios de una rama con otra. Típicamente fusionas una rama de características con la rama principal.

**Más información:** [Merge y Conflictos](/guides/es/flujo-de-trabajo.md#merge-y-conflictos)

### ¿Qué es un conflicto?

Un **conflicto** ocurre cuando Git no puede fusionar automáticamente porque dos ramas modifican las mismas líneas.

**Solución:**

1. Edita el archivo en conflicto
2. Elige qué cambios mantener
3. Elimina los marcadores de conflicto
4. Haz commit

### ¿Cómo resuelvo conflictos?

Los conflictos se ven así en el archivo:

```
<<<<<<< HEAD
tu versión
=======
otra versión
>>>>>>> rama-nombre
```

Elige una, elimina los marcadores y haz commit.

**Más información:** [Resolver Conflictos](/guides/es/flujo-de-trabajo.md#merge-y-conflictos)

### ¿Qué es un Pull Request?

Un **Pull Request** (o Merge Request en GitLab) es una solicitud para revisar tus cambios antes de fusionarlos. Permite:

- Revisar código
- Discutir cambios
- Pedir mejoras
- Asegurar calidad

**Más información:** [Pull Requests](/guides/es/flujo-de-trabajo.md#pull-requests)

---

## Errores Comunes

### "fatal: not a git repository"

**Problema:** No estás en un repositorio Git.

**Solución:**

```bash
# Inicializar repositorio
git init

# O clonar uno existente
git clone url
```

### "error: pathspec 'rama' did not match any file"

**Problema:** La rama no existe.

**Solución:**

```bash
# Ver ramas disponibles
git branch -a

# Crear la rama
git checkout -b rama
```

### "Please commit your changes before switching branches"

**Problema:** Tienes cambios sin guardar.

**Solución 1:** Hacer commit

```bash
git add .
git commit -m "Cambios intermedios"
git checkout otra-rama
```

**Solución 2:** Guardar temporalmente

```bash
git stash
git checkout otra-rama
git stash pop
```

### "Updates were rejected because the tip of your current branch is behind"

**Problema:** El remoto tiene cambios que no tienes.

**Solución:**

```bash
git pull origin rama
git push
```

### "Your branch has diverged"

**Problema:** Tu rama y el remoto han evolucionado de forma diferente.

**Solución:**

```bash
# Opción 1: Rebase (historial lineal)
git pull --rebase

# Opción 2: Merge (preserva historial)
git pull
```

---

## Seguridad

### ¿Debería compartir mis claves SSH?

**¡NO!** Nunca compartas:

- Tu clave privada SSH
- Tu clave privada GPG
- Tus tokens de acceso

Comparte solo:

- Tu clave pública SSH (`.pub`)

### ¿Qué puedo hacer si accidentalmente commiteé información sensible?

**Opción 1:** Si aún no hiciste push

```bash
git reset --soft HEAD~1
# Edita el commit
git commit -m "Nuevo mensaje"
```

**Opción 2:** Si ya hiciste push

```bash
# Remover archivo del historial
git filter-branch --tree-filter 'rm -f archivo-sensible.txt' HEAD

# Forzar push
git push --force
```

### ¿Debería usar contraseñas en mis commits?

**No.** Nunca comitees:

- Contraseñas
- Tokens
- Claves de API

**Usa en su lugar:**

- Variables de entorno
- Archivos `.env` (no commiteados)
- Secrets en plataformas (GitHub Secrets, etc.)

### ¿Cómo firmo mis commits?

Usa GPG para firmar commits digitalmente:

```bash
git commit -S -m "Mensaje"
```

**Más información:** [Configuración GPG](/guides/es/configuracion-gpg.md)

---

## ¿Aún tienes preguntas?

- 📚 **Documentación completa:** [Inicio](/docs/es)
- 📖 **Guías detalladas:** [Guías](/guies/es)
- ⚡ **Referencias rápidas:** [Cheat Sheets](/cheat-sheets)
- 🔧 **Configuración:** [Primeros pasos](/docs/es/configuracion-inicial.md)

---

**¿Encontraste útil este FAQ?** Comparte comentarios o sugiere nuevas preguntas.

# Conceptos Básicos de Git

Una guía completa de la terminología y conceptos fundamentales que necesitas
entender para trabajar con Git.

## Tabla de Contenidos

1. [Repositorio](#repositorio)
2. [Commit](#commit)
3. [Branch (Rama)](#branch-rama)
4. [Push y Pull](#push-y-pull)
5. [Merge](#merge)
6. [Conflicto](#conflicto)
7. [Remote](#remote)
8. [Staging Area](#staging-area)
9. [Head](#head)
10. [Tag](#tag)

### Repositorio

**Definición:**

Un **repositorio** es un almacenamiento de datos que contiene:

- Todos los archivos de tu proyecto
- El historial completo de cambios
- Información de autores y fechas
- Ramas y tags

**Tipos:**

**Repositorio Local:**

- Ubicado en tu máquina local
- Contiene una copia completa del historial
- Permite trabajar sin conexión

**Repositorio Remoto:**

- Ubicado en un servidor (GitHub, GitLab, Bitbucket)
- Compartido entre múltiples colaboradores
- Versión "oficial" del proyecto

Crear un repositorio

```bash
# crear un nuevo repositorio local
git init

# clonar un repositorio remoto
git clone <url-del-repositorio>
```

### Commit

**Definición:**

Un **commit** es una "instantánea" del estado de tu proyecto en un momento
específico. Es como una fotografía de todos tus archivos en ese momento.

**Componentes de un Commit:**

- **Cambios:** Las líneas añadidas, modificadas o eliminadas
- **Mensaje:** Descripción de qué cambió y por qué
- **Autor:** Quién hizo el cambio
- **Fecha:** Cuándo se hizo el cambio
- **Hash:** Identificador único del commit (ej. `a1b2c3d..`)

**Ejemplos de Commits:**

```bash
# crear un commit simple
git commit -m "Agregar función de login"

# commit con descripción más detallada
git commit -m "Agregar autenticación de usuarios" -m "Implementa JWT para tokens de sesión"

# ver historial de commits
git log
```

**Buenas Prácticas:**

✓ Mensajes claros y descriptivos

```txt
Correcto: "Corregir bug en validación de email"
Incorrecto: "Arreglar cosas"
```

✓ Commits atómicos (un cambio por commit)

```txt
Correcto: 3 commits separados para 3 cambios
Incorrecto: 1 commit con múltiples cambios no relacionados
```

✓ Mensajes en presente

```txt
Correcto: "Agregar validación"
Incorrecto: "Agregué validación" o "Agregando validación"
```

### Branch (Rama)

**Definición:**

Una **rama** es una línea independiente de desarrollo. Permite trabajar en nuevas
características sin afectar la rama principal.

**Ramas Especiales:**

**Main (o Master):**

- La rama principal y estable del proyecto
- Contiene el código listo para producción
- Generalmente protegida y requiere revisión

**Develop:**

- Rama de desarrollo
- Integra características antes de fusionarlas a `main`
- Puede ser menos estable que `main`

**Operaciones Básicas:**

```bash
# ver ramas locales
git branch

# ver todas las ramas (locales y remotas)
git branch -a

# crear una nueva rama
git branch <nombre-de-la-rama>

# cambiar a otra rama
git checkout <nombre-de-la-rama>

# combinado: crear y cambiar
git checkout -b <nombre-de-la-rama>

# eliminar una rama local
git branch -d <nombre-de-la-rama>

# eliminar una rama remota
git push origin --delete <nombre-de-la-rama>
```

**Convenciones de Nombres:**

Es recomendable usar nombres descriptivos y consistentes:

```txt
feature/nueva-caracteristica        # Nueva funcionalidad - característica
fix/correccion-bug                  # Corrección de errores
hotfix/arreglo-critico              # Arreglo urgente en producción
refactor/nombre-mejorado            # Refactorización de código
docs/actualizacion-documentacion    # Cambios en la documentación
```

Ejemplo `feature/agregar-carrito-compras`

### Push y Pull

#### Push (Empujar)

**Definición:** Enviar tus commits locales al repositorio remoto.

```bash
# enviar rama actual al remoto
git push

# enviar rama específica al remoto
git push origin <nombre-de-la-rama>

# enviar todas las ramas al remoto
git push --all
```

#### Pull (Traer)

**Definición:** Descargar cambios del repositorio remotos e integrarlos localmente.

```bash
# traer cambios de la rama actual
git pull

# traer cambios de una rama específica
git pull origin <nombre-de-la-rama>
```

**Diferencia: Pull = Fetch + Merge**

```bash
# pull es equivalente a:
git fetch       # Descargar cambios
git merge       # Integrar cambios
```

### Merge

**Definición:**

**Merge** combina cambios de una rama con otra. Típicamente, integras una rama de
característica con la rama principal.

**Ejemplo:**

```bash
# estar en la rama donde quieres traer los cambios
git checkout main

# fusionar otra rama
git merge <nombre-de-la-rama>
```

**Tipos de Merge:**

**Fast-Forward Merge:**

- Ocurre cuando no hay cambios en la rama destino
- El historial es lineal

**Merge Commit:**

- Se crea un nuevo commit de fusión
- El historial muestra la rama fusionada

```bash
# forzar un merge commit incluso si es posible un fast-forward
git merge --no-ff <nombre-de-la-rama>
```

### Conflicto

Un **conflicto** ocurre cuando Git no puede fusionar automáticamente cambios
porque dos ramas modifican las mismas líneas de código.

**Detectar un conflicto:**

```bash
git merge <nombre-de-la-rama>
# Error: CONFLICT (content merge conflict)
```

**Resolver un conflicto:**

Git marca los conflictos en los archivos:

```txt
<<<<<<< HEAD
tu versión del código
=======
versión de la otra rama
>>>>>>> nombre-rama
```

Pasos para resolver:

1. Abre el archivo en conflicto
2. Decide qué cambios mantener
3. Elimina las marcas de conflicto (`<<<<<<<`, `=======`, `>>>>>>>`)
4. Guarda el archivo
5. Haz un commit para completar la fusión

```bash
git add <archivo-resuelto>
git commit -m "Resolver conflicto en <archivo>"
```

### Remote

**Definición:**

Un **remote** es una referencia a un repositorio remoto. Por defecto, se llama
`origin`.

**Operaciones:**

```bash
# ver remotes configurados
git remote -v

# agregar un nuevo remote
git remote add <nombre> <url-del-repositorio>

# cambiar URL de un remote
git remote set-url <nombre> <nueva-url>

# eliminar un remote
git remote remove <nombre>

# ver detalles de un remote
git remote show <nombre>
```

**Ejemplo:**

```bash
# por defecto, GitHub crea origin
git remote add origin <url-del-repositorio>

# ver que está configurado
git remote -v
# origin  <url-del-repositorio> (fetch)
# origin <url-del-repositorio>  (push)
```

### Staging Area

**Definición:**

El **Staging Area** es un área intermedia donde preparas los cambios antes de
hacer un commit.

Permite seleccionar qué cambios incluir en el próximo commit, no todos a la vez.

**Comandos:**

```bash
# ver estado actual
git status

# agregar archivo especifico al staging area
git add <archivo>

# agregar todos los archivos modificados
git add .

# remover archivo del staging area (sin perder cambios)
git restore --staged <archivo>

# ver cambios en staging area
git diff --staged
```

**Flujo Completo:**

```bash
# 1. hacer cambios en archivos
# ... editar archivo.txt ...

# 2. Ver estado
git status
# archivo.txt modificado

# 3. Agregar al staging area
git add archivo.txt

# 4. Hacer commit
git commit -m "Describir cambio"

# 5. Enviar al remoto
git push
```

### Head

**Definición:**

**HEAD** es un puntero que indica en qué rama estás actualmente. Generalmente
apunta a la rama y el commit más reciente.

**Ejemplos:**

```bash
# ver a dónde apunta HEAD
cat .git/HEAD

# cambiar HEAD (moverte a otra rama)
git checkout main

# ver diferencias entre HEAD y tu rama actual
git diff HEAD

# ver diferencias entre commits
git show HEAD~1  # ver el commit anterior
git show HEAD~2  # ver dos commits atrás
git show HEAD    # ver el commit actual
```

### Tag

**Definición:**

Un **tag** es una referencia a un punto específico en el historial, generalmente
para marcar versiones importantes del software.

**Tipos:**

**Tag Ligero:** Solo un nombre que apunta a un commit.

```bash
git tag v1.0.0
```

**Tag Anotado:** Incluye información adicional (autor, fecha, mensaje).

```bash
git tag -a v1.0.0 -m "Versión 1.0.0"
```

**Operaciones:**

```bash
# listar tags
git tag

# ver detalles de un tag
git show v1.0.0

# crear un tag
git tag <nombre-del-tag>

# eliminar un tag local
git tag -d <nombre-del-tag>

# eliminar un tag remoto
git push origin --delete <nombre-del-tag>

# enviar tags al remoto
git push origin v1.0.0          # enviar un tag específico
git push origin --tags         # enviar todos los tags
```

#### Resumen Visual

```txt
REPOSITORIO REMOTO (GitHub, GitLab, etc.)
         ↓ git clone / git pull
         ↓
┌─────────────────────┐
│  REPOSITORIO LOCAL  │
│                     │
│  ┌───────────────┐  │
│  │   Directorio  │  │  ← Tu proyecto
│  │   de trabajo  │  │
│  └────────┬──────┘  │
│           │ git add
│           ↓         │
│  ┌─────────────────┐ │
│  │  Staging Area   │ │  ← Archivos listos
│  │   (Index)       │ │
│  └────────┬────────┘ │
│           │ git commit
│           ↓          │
│  ┌─────────────────┐ │
│  │ Git Directory   │ │  ← Histórico completo
│  │ (.git)          │ │
│  │                 │ │
│  │ Commits, Ramas, │ │
│  │ Tags, etc.      │ │
│  └─────────────────┘ │
└─────────────────────┘
         ↑ git push
         ↑
REPOSITORIO REMOTO
```

#### Próximos Pasos

Ahora que comprendes los conceptos básicos:

1. [Autenticación SSH vs HTTPS](/guides/es/autenticacion-ssh-https.md) - Aprende a conectar con repositorios remotos.
2. [Ejercicios de Nivel Básico](/exercises/basic-level/)

---

**Anterior:** [Configuración Inicial](configuracion-inicial.md)

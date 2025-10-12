# Configuración Inicial de Git

Después de instalar Git, necesitas realizar una configuración básica
antes de empezar a trabajar.

## Configuración del Usuario

**Nombre de Usuario:**

La primera configuración es establecer tu nombre. Este nombre aparecerá
en todos tus commits.

```bash
git config --global user.name "Tu Nombre"
```

Ejemplo:

```bash
git config --global user.name "Pepito Pérez"
```

**Correo Electrónico:**

Luego, configura tu correo electrónico. También aparecerá en tus commits.

```bash
git config --global user.email "tu.email@ejemplo.com"
```

Ejemplo:

```bash
git config --global user.email "pepito@ejemplo.com"
```

**Verificar Configuración:**

Para verificar que se guardó correctamente:

```bash
git config --global user.name
git config --global user.email
```

O puedes ver toda la configuración con:

```bash
git config --global --list
```

### Configuraciones opcionales pero recomendadas

**Editor por Defecto:**

Configura qué editor deseas usar cuando Git necesite que escribas
mensajes. Por defecto es `nano`.

**Nano:**

```bash
git config --global core.editor "nano"
```

**Vim:**

```bash
git config --global core.editor "vim"
```

**Visual Studio Code:**

```bash
git config --global core.editor "code --wait"
```

**Nombre de Rama por Defecto:**

Desde Git 2.28, puedes cambiar el nombre de la rama por defecto
(antes era `master`, ahora es `main`).

```bash
git config --global init.defaultBranch main
```

**Alias Útiles:**

Los alias permiten crear atajos para comandos frecuentes:

```bash
# ver commits en una línea
git config --global alias.lg "log --oneline --graph --all --decorate"

# estado corto
git config --global alias.st "status -s"

# ver diferencias
git config --global alias.df "diff"

# ver ramas
git config --global alias.br "branch -a"

# commit
git config --global alias.cm "commit -m"

# agregar archivos
git config --global alias.ad "add"
```

Después de esto, puedes usar:

```bash
git lg
git st
```

**Colorización de Output:**

Habilita colores en la salida de Git para mejor legibilidad:

```bash
git config --global color.ui true
```

### Configuración Local vs Global vs Sistema

Git tiene tres niveles de configuración:

1. Global (`--global`)

   Aplica a todos los repositorios de tu usuario.

   ```bash
   git config --global user.name "Tu Nombre"
   ```

   Se guarda en `~/.gitconfig`

2. Local (sin bandera)

   Aplica solo al repositorio actual.

   ```bash
   git config user.name "Nombre Específico para este Proyecto"
   ```

   Se guarda en `.git/config` dentro del repositorio.

3. Sistema (`--system`)

   Aplica a todos los usuarios de la computadora (requiere permisos de administrador).

   ```bash
   sudo git config --system user.name "Nombre del Sistema"
   ```

   Se guarda en `/etc/gitconfig`

### Ver tu Configuración

**Ver solo configuración global:**

```bash
git config --global --list
```

**Ver configuración local (del repositorio actual):**

```bash
git config --local --list
```

**Ver configuración del sistema:**

```bash
git config --system --list
```

**Ver toda la configuración (todos los niveles):**

```bash
git config --list
```

### Archivos de Configuración

**Ubicaciones:**

**Windows:**

- Global: `C:\Users\TuUsuario\.gitconfig`
- Sistema: `C:\Program Files\Git\etc\gitconfig`

**macOS/Linux:**

- Global: `~/.gitconfig` o `~/.config/git/config`
- Sistema: `/etc/gitconfig`

**Editar directamente (Avanzado):**

Puedes editar manualmente estos archivos. Ejemplo de `~/.gitconfig`:

```ini
[user]
  name = Tu Nombre
  email = tu.email@ejemplo.com

[core]
  editor = code --wait

[init]
  defaultBranch = main

[alias]
  st = status -s
  lg = log --oneline --graph --all --decorate
  cm = commit -m
  ad = add
```

### Primeros Pasos después de la Configuración

Una vez que hayas completado la configuración, estás list para:

1. Crear tu primer repositorio:

   ```bash
   mkdir mi-proyecto
   cd mi-proyecto
   git init
   ```

2. O clonar un repositorio existente:

   ```bash
   git clone https://github.com/usuario/repositorio.git
   ```

3. Verificar el estado del repositorio:

   ```bash
   git status
   ```

### Próximos Pasos

Ahora que tienes Git configurado:

1. [Conceptos Básicos de Git](docs/es/conceptos-basicos.md) - Aprende los términos clave.
2. Lee la guía sobre [Autenticación SSH vs HTTPS](guides/es/autenticacion-ssh-https.md) para conectarte a repositorios remotos.
3. Empieza con los [ejercicios de nivel básico](ejercicios/nivel-basico.md) para practicar.

---

**Anterior:** [instalación](/docs/es/instalacion.md) | **Siguiente:** [conceptos básicos](/docs/es/conceptos-basicos.md)

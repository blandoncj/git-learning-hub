# Instalación de Git

Guía paso a paso para instalar Git en Windows, macOS y Linux.

## Windows

**Opción 1: Git for Windows (Recomendado):**

1. Descarga el instalador desde [git-scm.com](https://git-scm.com/download/win).
2. Ejecuta el archivo `.exe` descargado.
3. En la instalación, recomendamos dejar las opciones por defecto:
   - **Git Bash Here:** ✓ (útil para terminal de Git)
   - **Git GUI Here:** ✓ (interfaz gráfica opcional)
   - **Use Git from the Windows Command Prompt:** ✓ (recomendado)
4. Completa la instalación y reinicia tu computadora si es necesario.

**Verificar la instalación:**

Abre **Git Bash**, **PowerShell** o **Símbolo del sistema** y ejecuta:

```bash
git --version
```

Deberías ver algo como `git version 2.x.x`.

**Opción 2: Windows Package Manager (winget):**

Si tienes Windows Package Manager instalado:

```bash
winget install Git.Git
```

## macOS

**Opción 1: Instalador Oficial:**

1. Descarga el instalador desde [git-scm.com](https://git-scm.com/download/mac).
2. Ejecuta el instalador `.dmg`
3. Sigue las instrucciones para completar la instalación.
4. Completa la instalación y reinicia tu computadora si es necesario.

**Opción 2: Homebrew (Recomendado):**

Si tienes Homebrew instalado:

```bash
brew install git
```

**Opción 3: Xcode Command Line Tools:**

Instala las herramientas de Xcode que incluyen Git:

```bash
xcode-select --install
```

**Verificar la instalación:**

Abre **Terminal** y ejecuta:

```bash
git --version
```

## Linux

**Ubuntu/Debian:**

```bash
sudo apt update
sudo apt install git
```

**Fedora/RHEL/CentOS:**

```bash
sudo dnf install git
```

O para versiones más antiguas:

```bash
sudo yum install git
```

**Arch Linux:**

```bash
sudo pacman -S git
```

**Verificar la instalación:**

```bash
git --version
```

### Verificación General

Después de instalar en cualquier sistema operativo, ejecuta estos comandos para
asegurarte de que todo funciona correctamente:

```bash
# ver versión de Git
git --version

# ver ubicación del ejecutable de Git
which git       # macOS/Linux
where git       # Windows

# ver ayuda de Git
git --help
```

### Instalación de Visual Studio Code (Recomendado)

Para una mejor experiencia trabajando con Git, se recomienda usar Visual Studio Code,
que tiene soporte integrado para Git.

**Descargar VS Code:**

1. Ve a [code.visualstudio.com](https://code.visualstudio.com/).
2. Descarga para tu sistema operativo.
3. Sigue las instrucciones de instalación.

**Extensiones recomendadas:**

Una vez instalado VS Code, puedes agregar estas extensiones (opcionales pero útiles):

- **GitLens**: Visualización avanzada de Git.
- **Git Graph**: Visualización gráfica de ramas y commits.
- **GitHub Copilot**: Asistente de codificación impulsado por IA (requiere suscripción).

### Instalación de Git GUI (Opcional)

Si prefieres una interfaz gráfica en lugar de línea de comandos:

**SourceTree:**

- **Plaforma:** Windows, macOS
- **Costo:** Gratuito
- **Descargar:** [sourcetreeapp.com](https://www.sourcetreeapp.com/)

**GitKraken:**

- **Plataforma:** Windows, macOS, Linux
- **Costo:** Gratuito para uso básico, planes de pago disponibles
- **Descargar:** [gitkraken.com](https://www.gitkraken.com/)

**GitHub Desktop:**

- **Plataforma:** Windows, macOS
- **Costo:** Gratuito
- **Descargar:** [desktop.github.com](https://desktop.github.com/)

**Tortoise Git (Windows):**

- **Plataforma:** Solo Windows
- **Costo:** Gratuito
- **Descargar:** [tortoisegit.org](https://tortoisegit.org/)

#### Resolución de Problemas Comunes

**Error: "git: command not found":**

**Windows:**

- Verifica que Git esté en tu PATH. Intenta reiniciar tu terminal o computadora.
- Reinstala Git seleccionando la opción "Use Git from the Windows Command Prompt".

**macOS/Linux:**

- Verifica la instalación con `which git`.
- Si no está instalado, sigue los pasos de instalación para tu sistema operativo.

**Error: "Permission denied" (Linux/macOS):**

Si obtienes errores de permisos:

```bash
sudo chown -R $USER:$USER ~/.git-credentials
```

**Git lento:**

Intenta actualizar Git a la última versión.

#### Próximos Pasos

Una vez que Git esté instalado, puedes proceder a:

1. [Configuración Inicial](/docs/es/configuracion-inicial.md) - Configurar tu identidad y preferencias.
2. [Conceptos Básicos](/docs/es/conceptos-basicos.md) - Entender los términos clave.
3. [Introducción](/docs/es/introduccion.md) - Revisar conceptos generales.

---

**Anterior:** [introducción](/docs/es/introduccion.md) | **Siguiente:** [configuración inicial](/docs/es/configuracion-inicial.md)

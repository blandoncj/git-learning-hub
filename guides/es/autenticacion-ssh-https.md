# Autenticación SSH vs HTTPS

Guía completa sobre los dos protocolos principales para conectarte a repositorios remotos: SSH y HTTPS. Entenderás sus diferencias, ventajas y desventajas, y cuándo usar cada uno.

## Tabla de Contenidos

1. [Comparación Rápida](#comparación-rápida)
2. [SSH: ¿Qué es?](#ssh-qué-es)
3. [HTTPS: ¿Qué es?](#https-qué-es)
4. [Diferencias Clave](#diferencias-clave)
5. [Configurar SSH](#configurar-ssh)
6. [Configurar HTTPS](#configurar-https)
7. [Cuándo Usar Cada Uno](#cuándo-usar-cada-uno)
8. [Cambiar Entre Protocolos](#cambiar-entre-protocolos)
9. [Troubleshooting](#troubleshooting)

---

## Comparación Rápida

| Aspecto              | SSH                              | HTTPS                   |
| -------------------- | -------------------------------- | ----------------------- |
| **Seguridad**        | Muy alta (clave pública-privada) | Alta (certificados SSL) |
| **Configuración**    | Más compleja                     | Más simple              |
| **Contraseña**       | No necesita                      | Necesita cada vez       |
| **Firewall**         | Puede tener problemas            | Más compatible          |
| **Velocidad**        | Más rápido                       | Ligeramente más lento   |
| **Recomendado para** | Trabajo profesional              | Principiantes           |

---

## SSH: ¿Qué es?

### Definición

**SSH (Secure Shell)** es un protocolo de red seguro que utiliza criptografía de clave pública-privada para autenticarte en repositorios remotos sin necesidad de ingresar contraseña cada vez.

### ¿Cómo funciona?

1. Generas un par de claves:
   - **Clave privada:** La guardas en tu computadora (NUNCA la compartas)
   - **Clave pública:** La añades a tu cuenta de GitHub/GitLab/Bitbucket

2. Cuando haces un `git push` o `git pull`:
   - Git envía tu clave privada (sin transmitirla, solo la firma)
   - El servidor verifica con tu clave pública
   - Se establece la conexión segura

### Ventajas de SSH

✅ **No necesitas contraseña cada vez** - Después de configurar, es automático
✅ **Más seguro** - Usa criptografía de clave pública
✅ **Más rápido** - Protocolo optimizado para git
✅ **Ideal para CI/CD** - Automatización sin contraseñas
✅ **Profesional** - Estándar en desarrollo profesional

### Desventajas de SSH

❌ **Configuración más compleja** - Requiere generar claves
❌ **Problemas con firewall** - Algunos firewalls corporativos bloquean SSH
❌ **Requiere terminal** - No es tan amigable para principiantes GUI

---

## HTTPS: ¿Qué es?

### Definición

**HTTPS (HyperText Transfer Protocol Secure)** es el mismo protocolo que usa el navegador para acceder a sitios web. Usa certificados SSL para cifrar la comunicación.

### ¿Cómo funciona?

1. Usas tu nombre de usuario y contraseña (o token)
2. Git cifra la comunicación con HTTPS
3. El servidor verifica tus credenciales
4. Se establece la conexión

### Ventajas de HTTPS

✅ **Fácil de usar** - Solo necesitas usuario/contraseña
✅ **Compatible con firewall** - Puerto 443 generalmente abierto
✅ **Sin configuración de claves** - No hay que generar nada
✅ **Bueno para principiantes** - Menos pasos iniciales

### Desventajas de HTTPS

❌ **Necesitas ingresar credenciales** - O usar Git Credentials Manager
❌ **Menos seguro que SSH** - Contraseñas pueden ser interceptadas
❌ **Más lento** - Overhead de certificados SSL
❌ **Problemas con 2FA** - Requiere tokens especiales

---

## Diferencias Clave

### Autenticación

**SSH:**

```
Tú (Clave privada) → Servidor (Clave pública) → Verificado ✓
```

**HTTPS:**

```
Tú (Usuario + Contraseña) → Servidor → Base de datos → Verificado ✓
```

### Puertos

**SSH:** Puerto 22

```bash
ssh -T git@github.com
```

**HTTPS:** Puerto 443 (mismo que navegadores)

```bash
https://github.com/usuario/repo.git
```

### URLs

**SSH:**

```
git@github.com:usuario/repositorio.git
```

**HTTPS:**

```
https://github.com/usuario/repositorio.git
```

---

## Configurar SSH

### Paso 1: Generar Claves SSH

Abre tu terminal y ejecuta:

```bash
ssh-keygen -t ed25519 -C "tu.email@ejemplo.com"
```

**Notas:**

- `-t ed25519`: Tipo de clave (moderno y seguro)
- `-C "tu.email@ejemplo.com"`: Comentario identificador

**Alternativa (si tu sistema no soporta ed25519):**

```bash
ssh-keygen -t rsa -b 4096 -C "tu.email@ejemplo.com"
```

### Paso 2: Guardar las Claves

Cuando se te pida dónde guardar:

```
Enter file in which to save the key (/home/usuario/.ssh/id_ed25519):
```

Presiona **Enter** para guardar en la ubicación por defecto, o especifica una ruta.

### Paso 3: Proteger con Passphrase (Recomendado)

Se te pedirá una contraseña (passphrase):

```
Enter passphrase (empty for no passphrase):
```

Se recomienda ingresar una passphrase para mayor seguridad. Luego:

```
Confirm passphrase:
```

**Resultado:**

```
Your public key has been saved in /home/usuario/.ssh/id_ed25519.pub
Your private key has been saved in /home/usuario/.ssh/id_ed25519
```

### Paso 4: Iniciar el SSH Agent (macOS/Linux)

Esto evita que tengas que ingresar tu passphrase cada vez:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

**En Windows (Git Bash):**

```bash
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_ed25519
```

### Paso 5: Copiar la Clave Pública

```bash
# macOS
pbcopy < ~/.ssh/id_ed25519.pub

# Linux
cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard

# Windows (Git Bash)
cat ~/.ssh/id_ed25519.pub | clip
```

O simplemente abre el archivo y copia el contenido:

```bash
cat ~/.ssh/id_ed25519.pub
```

### Paso 6: Añadir la Clave a GitHub/GitLab/Bitbucket

#### GitHub

1. Ve a [github.com/settings/keys](https://github.com/settings/keys)
2. Haz clic en "New SSH key"
3. Pega tu clave pública
4. Dale un título (ej: "Mi laptop")
5. Haz clic en "Add SSH key"

#### GitLab

1. Ve a [gitlab.com/-/profile/keys](https://gitlab.com/-/profile/keys)
2. Pega tu clave pública en el campo
3. Dale un título
4. Haz clic en "Add key"

#### Bitbucket

1. Ve a [bitbucket.org/account/settings/ssh-keys/](https://bitbucket.org/account/settings/ssh-keys/)
2. Haz clic en "Add key"
3. Pega tu clave pública
4. Haz clic en "Add SSH key"

### Paso 7: Verificar la Conexión

```bash
# GitHub
ssh -T git@github.com

# GitLab
ssh -T git@gitlab.com

# Bitbucket
ssh -T git@bitbucket.org
```

**Respuesta esperada:**

```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## Configurar HTTPS

### Opción 1: Ingresar Contraseña Cada Vez

Cuando clonas o haces push/pull con HTTPS:

```bash
git clone https://github.com/usuario/repositorio.git
# Te pide usuario y contraseña
```

**No recomendado:** Es tedioso ingresar credenciales constantemente.

### Opción 2: Usar Git Credentials Manager (Recomendado)

Git Credentials Manager almacena tus credenciales de forma segura.

#### En Windows

Se instala automáticamente con Git for Windows. Si no:

```bash
# Usar Git Credential Manager (Windows)
git config --global credential.helper manager
```

#### En macOS

Se instala automáticamente. Si no:

```bash
git config --global credential.helper osxkeychain
```

#### En Linux

Instala primero:

```bash
sudo apt install git-credential-manager

# O para Fedora:
sudo dnf install git-credential-manager
```

Luego configura:

```bash
git config --global credential.helper manager-core
```

#### Verificar que funciona

```bash
git config --global credential.helper
# Debería mostrar: manager, manager-core, osxkeychain, etc.
```

### Opción 3: Usar un Token Personal (Para Seguridad Extra)

En lugar de contraseña, GitHub/GitLab/Bitbucket te permiten generar tokens.

#### GitHub

1. Ve a [github.com/settings/tokens](https://github.com/settings/tokens)
2. Haz clic en "Generate new token"
3. Dale permisos `repo` (acceso a repositorios)
4. Copia el token
5. Usa como contraseña al hacer git clone/push

#### GitLab

1. Ve a [gitlab.com/-/profile/personal_access_tokens](https://gitlab.com/-/profile/personal_access_tokens)
2. Crea un nuevo token
3. Copia el token
4. Usa como contraseña

#### Bitbucket

1. Ve a [bitbucket.org/account/settings/app-passwords/new](https://bitbucket.org/account/settings/app-passwords/new)
2. Crea una nueva contraseña de aplicación
3. Copia la contraseña
4. Usa como contraseña en Git

---

## Cuándo Usar Cada Uno

### Usa SSH Si

✅ Eres un desarrollador profesional
✅ Trabajas en múltiples repositorios
✅ Quieres automatización sin contraseñas
✅ Tu empresa no bloquea el puerto 22
✅ Valoras la máxima seguridad
✅ Trabajas con CI/CD

**Recomendación: SSH es mejor para trabajo profesional.**

### Usa HTTPS Si

✅ Eres principiante
✅ Tu firewall bloquea SSH (puerto 22)
✅ Trabajas en redes restrictivas
✅ Prefieres no complicarte con claves
✅ Solo usas Git ocasionalmente

**Recomendación: HTTPS es mejor para empezar.**

---

## Cambiar Entre Protocolos

### De HTTPS a SSH

Si clonas con HTTPS pero quieres cambiar a SSH:

```bash
# Ver remote actual
git remote -v
# origin  https://github.com/usuario/repo.git (fetch)

# Cambiar a SSH
git remote set-url origin git@github.com:usuario/repo.git

# Verificar
git remote -v
# origin  git@github.com:usuario/repo.git (fetch)
```

### De SSH a HTTPS

Si clonas con SSH pero quieres cambiar a HTTPS:

```bash
# Ver remote actual
git remote -v
# origin  git@github.com:usuario/repo.git (fetch)

# Cambiar a HTTPS
git remote set-url origin https://github.com/usuario/repo.git

# Verificar
git remote -v
# origin  https://github.com/usuario/repo.git (fetch)
```

---

## Troubleshooting

### "Permission denied (publickey)"

**Causa:** La clave SSH no está configurada correctamente.

**Solución:**

```bash
# Verificar que la clave existe
ls -la ~/.ssh/

# Iniciar SSH agent
eval "$(ssh-agent -s)"

# Agregar la clave al agent
ssh-add ~/.ssh/id_ed25519

# Verificar conexión
ssh -T git@github.com
```

### "HTTPS authentication failed"

**Causa:** Credenciales incorrectas o Git Credentials Manager no configurado.

**Solución:**

```bash
# Configurar Git Credentials Manager
git config --global credential.helper manager

# Limpiar credenciales guardadas (en Windows)
cmdkey /delete:git:https://github.com

# Limpiar credenciales guardadas (en macOS)
security delete-internet-password -s github.com

# Intentar de nuevo (te pedirá credenciales)
git push
```

### "Port 22 connection refused"

**Causa:** El firewall bloquea el puerto 22.

**Solución:** Usar HTTPS en lugar de SSH.

```bash
git remote set-url origin https://github.com/usuario/repo.git
```

### "Could not read from remote repository"

**Causa:** El remote no está configurado correctamente.

**Solución:**

```bash
# Ver remotes
git remote -v

# Si está vacío, agregar:
git remote add origin git@github.com:usuario/repo.git

# O si está mal, cambiar:
git remote set-url origin git@github.com:usuario/repo.git
```

### SSH Key demasiado permisiva

Si obtienes error de permisos en la clave SSH:

```bash
# En macOS/Linux
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

---

## Resumen

### Flujo para SSH

```
1. Generar claves SSH
   ↓
2. Copiar clave pública
   ↓
3. Añadir a GitHub/GitLab/Bitbucket
   ↓
4. Verificar conexión
   ↓
5. Usar repositorios sin contraseña
```

### Flujo para HTTPS

```
1. Instalar Git Credentials Manager
   ↓
2. (Opcional) Generar token personal
   ↓
3. Clonar con URL HTTPS
   ↓
4. Ingresar credenciales una vez
   ↓
5. Git Credentials Manager las guarda
```

---

## Próximos Pasos

1. **[Configuración GPG](configuracion-gpg.md)** - Firmar tus commits
2. **[Gestión de Ramas](gestion-ramas.md)** - Trabajar con branches
3. **[Flujo de Trabajo](flujo-de-trabajo.md)** - Workflows en equipo

---

**Siguiente:** [Configuración GPG](configuracion-gpg.md)

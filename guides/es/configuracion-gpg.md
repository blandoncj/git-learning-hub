# Configuración GPG - Firmar Commits

Guía completa sobre cómo usar GPG (GNU Privacy Guard) para firmar digitalmente tus commits en Git. Aprenderás por qué es importante, cómo configurarlo y cómo verificar commits firmados.

## Tabla de Contenidos

1. [¿Qué es GPG?](#qué-es-gpg)
2. [¿Por qué firmar commits?](#por-qué-firmar-commits)
3. [Generar una Clave GPG](#generar-una-clave-gpg)
4. [Configurar Git con GPG](#configurar-git-con-gpg)
5. [Firmar Commits](#firmar-commits)
6. [Verificar Commits Firmados](#verificar-commits-firmados)
7. [Configurar en GitHub/GitLab/Bitbucket](#configurar-en-githubgitlabbitbucket)
8. [Troubleshooting](#troubleshooting)

---

## ¿Qué es GPG?

### Definición

**GPG (GNU Privacy Guard)** es un software de cifrado de código abierto que implementa el estándar OpenPGP. Permite cifrar datos y crear firmas digitales usando criptografía asimétrica (clave pública-privada).

### En el contexto de Git

Cuando firmas un commit con GPG:

- Git crea una firma digital usando tu clave privada
- La firma se adjunta al commit
- Cualquiera puede verificar que tú (y solo tú) creaste ese commit usando tu clave pública
- GitHub/GitLab/Bitbucket marcan el commit como "Verified" (Verificado)

### Diferencia entre SSH y GPG

**SSH:** Autentica quién eres (acceso a repositorios)
**GPG:** Verifica autenticidad de commits (firma digital)

Ambos pueden (y deberían) usarse juntos.

---

## ¿Por qué firmar commits?

### Problemas sin Firmas

Sin GPG, cualquiera podría:

```bash
git config user.name "Linus Torvalds"
git commit -m "Cambio importante"
git push
```

El commit aparecería como si lo hizo Linus Torvalds, pero fue otro.

### Beneficios de Firmar

✅ **Autenticidad:** Prueba que tú creaste el commit
✅ **No repudiación:** No puedes negar que lo creaste
✅ **Integridad:** El commit no puede ser modificado después
✅ **Conformidad:** Muchas empresas y proyectos lo requieren
✅ **Confianza:** Los colaboradores saben que el código es legítimo
✅ **Auditoría:** Historial verificable de quién hizo qué

### Casos de Uso

- Proyectos open source importantes
- Código de producción crítico
- Conformidad normativa (regulaciones financieras, médicas, etc.)
- Equipos distribuidos que requieren máxima seguridad

---

## Generar una Clave GPG

### Paso 1: Instalar GPG

**Windows:**
Descarga desde [gpg4win.org](https://www.gpg4win.org/) o usa:

```bash
choco install gpg4win
```

**macOS:**

```bash
brew install gnupg
```

**Linux (Debian/Ubuntu):**

```bash
sudo apt install gnupg
```

**Linux (Fedora/RHEL):**

```bash
sudo dnf install gnupg
```

### Paso 2: Verificar Instalación

```bash
gpg --version
```

Deberías ver: `gpg (GnuPG) 2.x.x` o similar.

### Paso 3: Generar la Clave

```bash
gpg --full-generate-key
```

Se te harán varias preguntas:

**1. Tipo de clave:**

```
Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
  (14) Existing key from card
Your selection? 1
```

Elige **1** (RSA and RSA)

**2. Tamaño de la clave:**

```
What keysize do you want? (3072)
```

Elige **4096** (más seguro):

```
What keysize do you want? 4096
```

**3. Validez de la clave:**

```
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0)
```

Presiona **Enter** para dejar sin expiración (0), o elige un tiempo.

**4. Confirmación:**

```
Is this correct? (y/N) y
```

Ingresa **y** (yes)

**5. Información Personal:**

```
GnuPG needs to construct a user ID to identify your key.

Real name: Tu Nombre Completo
Email address: tu.email@ejemplo.com
Comment: Mi clave de trabajo (opcional)
```

Ingresa tu información real (debe coincidir con tu Git config).

**6. Confirmación final:**

```
Is this correct? (y/N) y
```

Ingresa **y**

**7. Passphrase:**

```
You need a Passphrase to protect your secret key.
```

Se abrirá un diálogo. Ingresa una contraseña fuerte (la usarás para firmar commits).

**Resultado esperado:**

```
gpg: key 1234567890ABCDEF generated
gpg: revocation certificate stored as '/home/user/.gnupg/revocation-certs.d/1234567890ABCDEF.rev'
public key created successfully
```

### Paso 4: Verificar la Clave

```bash
gpg --list-secret-keys --keyid-format LONG
```

Verás algo como:

```
sec   rsa4096/1234567890ABCDEF 2024-01-15 [SC]
      1234567890ABCDEF1234567890ABCDEF12345678
uid                 [ultimate] Tu Nombre <tu.email@ejemplo.com>
ssb   rsa4096/FEDCBA0987654321 2024-01-15 [E]
```

El ID de la clave es: **1234567890ABCDEF** (16 caracteres después de rsa4096/)

---

## Configurar Git con GPG

### Paso 1: Decirle a Git tu ID de Clave

```bash
git config --global user.signingkey 1234567890ABCDEF
```

Reemplaza `1234567890ABCDEF` con tu ID real.

**Para verificar:**

```bash
git config --global user.signingkey
```

### Paso 2: Configurar Git para Firmar por Defecto (Opcional)

Para que Git firme automáticamente todos tus commits:

```bash
git config --global commit.gpgsign true
```

**Ventaja:** No necesitas agregar `-S` a cada commit.
**Desventaja:** Cada commit pedirá tu passphrase.

### Paso 3: Configurar GPG Agent (Opcional pero Recomendado)

Para no ingresar tu passphrase cada vez, configura el GPG Agent:

**macOS/Linux:**

Edita o crea `~/.gnupg/gpg-agent.conf`:

```bash
default-cache-ttl 600
max-cache-ttl 7200
```

Esto cachea tu passphrase por 10 minutos (600 segundos).

Luego reinicia GPG Agent:

```bash
gpgconf --kill gpg-agent
```

**Windows:**

Ya está configurado automáticamente. Si tienes problemas, reinstala GPG4Win.

---

## Firmar Commits

### Opción 1: Firmar un Commit Individual

```bash
git commit -S -m "Tu mensaje del commit"
```

El `-S` significa "sign" (firmar). Git pedirá tu passphrase de GPG.

### Opción 2: Firmar con Configuración Automática

Si configuraste `commit.gpgsign = true`:

```bash
git commit -m "Tu mensaje del commit"
```

Git firmará automáticamente.

### Opción 3: Firmar un Commit Existente (Enmendar)

Si olvidaste firmar:

```bash
git commit --amend -S
```

### Ejemplos Completos

```bash
# Firmar y empujar
git add archivo.txt
git commit -S -m "Agregar nueva función"
git push origin main

# Ver log con firmas
git log --show-signature
```

---

## Verificar Commits Firmados

### En la Terminal

**Ver commits con firmas:**

```bash
git log --show-signature
```

Verás:

```
commit abc123 (HEAD -> main)
gpg: Signature made Mon Jan 15 10:30:45 2024
gpg:                using RSA key 1234567890ABCDEF
gpg:                issuer "tu.email@ejemplo.com"
gpg: Good signature from "Tu Nombre <tu.email@ejemplo.com>"
```

**Ver commits en una línea con estado de firma:**

```bash
git log --oneline --pretty='%h %G? %s'
```

Verás:

```
abc123 G Agregar nueva función
def456 N Cambio sin firmar
```

**Significados:**

- **G:** Firma buena (Good)
- **B:** Firma mala (Bad)
- **U:** Firma de confianza desconocida (Unknown)
- **X:** Firma ha expirado (eXpired)
- **Y:** Firma caduca pronto (expireY)
- **R:** Firma ha sido revocada (Revoked)
- **E:** No hay firma, no pudo verificarse
- **N:** No hay firma (None)

### En GitHub

Una vez que agregues tu clave pública GPG a GitHub (ver sección siguiente):

1. Ve a tu repositorio
2. Mira el historial de commits
3. Los commits firmados mostrarán una insignia "Verified" verde

### En GitLab

GitLab mostrará un ícono "Verified" en commits firmados correctamente.

### En Bitbucket

Bitbucket muestra información de firmas en el detalle del commit.

---

## Configurar en GitHub/GitLab/Bitbucket

### GitHub

#### Paso 1: Copiar tu Clave Pública GPG

```bash
gpg --armor --export tu.email@ejemplo.com
```

Copiar todo el output (incluyendo `-----BEGIN PGP PUBLIC KEY BLOCK-----` y `-----END PGP PUBLIC KEY BLOCK-----`).

#### Paso 2: Agregar a GitHub

1. Ve a [github.com/settings/keys](https://github.com/settings/keys)
2. Haz clic en "New GPG key"
3. Pega tu clave pública
4. Haz clic en "Add GPG key"

#### Paso 3: Verificar

Haz un commit firmado y haz push:

```bash
git commit -S -m "Test firma"
git push
```

Verás la insignia "Verified" en GitHub.

### GitLab

#### Paso 1: Copiar tu Clave Pública

```bash
gpg --armor --export tu.email@ejemplo.com
```

#### Paso 2: Agregar a GitLab

1. Ve a [gitlab.com/-/profile/gpg_keys](https://gitlab.com/-/profile/gpg_keys)
2. Pega tu clave pública
3. Haz clic en "Add key"

#### Paso 3: Verificar

Los commits firmados mostrarán "Verified" en GitLab.

### Bitbucket

#### Paso 1: Copiar tu Clave Pública

```bash
gpg --armor --export tu.email@ejemplo.com
```

#### Paso 2: Agregar a Bitbucket

1. Ve a [bitbucket.org/account/settings/gpg-keys/](https://bitbucket.org/account/settings/gpg-keys/)
2. Pega tu clave pública
3. Haz clic en "Add key"

#### Paso 3: Verificar

Los commits firmados mostrarán un indicador en Bitbucket.

---

## Troubleshooting

### "gpg: signing failed: No secret key"

**Causa:** Git no encontró tu clave GPG.

**Solución:**

```bash
# Verificar que tienes claves
gpg --list-secret-keys

# Configurar el ID de clave correcto
git config --global user.signingkey TU_ID_DE_CLAVE
```

### "gpg: failed to sign the data"

**Causa:** Problema con GPG Agent.

**Solución:**

```bash
# Reiniciar GPG Agent
gpgconf --kill gpg-agent

# Intentar nuevamente
git commit -S -m "Test"
```

### "error: gpg failed to sign the data"

**Causa:** Passphrase incorrecta o GPG no está configurado.

**Solución:**

```bash
# Probar GPG directamente
echo "test" | gpg --clearsign

# Si pide passphrase y funciona, el problema es Git
# Verificar configuración de Git
git config --global user.signingkey
```

### GPG pide passphrase cada vez

**Causa:** GPG Agent no está cacheando.

**Solución:**

Edita `~/.gnupg/gpg-agent.conf` y agrega:

```
default-cache-ttl 600
max-cache-ttl 7200
pinentry-program /usr/bin/pinentry-tty
```

Luego reinicia:

```bash
gpgconf --kill gpg-agent
```

### "No public key available"

**Causa:** Tu clave pública no está agregada a GitHub/GitLab/Bitbucket.

**Solución:**

Sigue los pasos en [Configurar en GitHub/GitLab/Bitbucket](#configurar-en-githubgitlabbitbucket).

### Verificar que GPG funciona

```bash
# Test de firma
echo "test message" | gpg --clearsign

# Si funciona, verás:
# -----BEGIN PGP SIGNED MESSAGE-----
# ...
# -----END PGP SIGNATURE-----
```

---

## Resumen: Workflow Completo

```
1. Instalar GPG
   ↓
2. Generar clave GPG (ssh-keygen para GPG)
   ↓
3. Configurar Git con tu ID de clave
   ↓
4. Copiar clave pública a GitHub/GitLab/Bitbucket
   ↓
5. Hacer commits firmados (-S flag)
   ↓
6. Ver "Verified" en repositorio remoto
```

---

## Comandos Rápidos

```bash
# Generar clave
gpg --full-generate-key

# Listar claves
gpg --list-secret-keys --keyid-format LONG

# Exportar clave pública
gpg --armor --export tu.email@ejemplo.com

# Configurar Git
git config --global user.signingkey ID_DE_CLAVE
git config --global commit.gpgsign true

# Firmar commit
git commit -S -m "Mensaje"

# Ver commits firmados
git log --show-signature
git log --oneline --pretty='%h %G? %s'

# Reiniciar GPG Agent
gpgconf --kill gpg-agent
```

---

## Próximos Pasos

1. **[Gestión de Ramas](gestion-ramas.md)** - Crear y trabajar con branches
2. **[Flujo de Trabajo](flujo-de-trabajo.md)** - Workflows en equipo
3. **[Rastreo de Cambios](rastreo-cambios.md)** - Auditar código

---

**Anterior:** [Autenticación SSH/HTTPS](autenticacion-ssh-https.md) | **Siguiente:** [Gestión de Ramas](gestion-ramas.md)

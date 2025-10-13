# SSH y GPG Keys - Cheat Sheet

Referencia rápida para generar, configurar y gestionar claves SSH y GPG.

## SSH - Generar Claves

```bash
# Generar clave ED25519 (recomendado)
ssh-keygen -t ed25519 -C "tu.email@ejemplo.com"

# Generar clave RSA (alternativa)
ssh-keygen -t rsa -b 4096 -C "tu.email@ejemplo.com"

# Generar sin passphrase
ssh-keygen -t ed25519 -N "" -C "tu.email@ejemplo.com"

# Usar ubicación personalizada
ssh-keygen -t ed25519 -f ~/.ssh/mi-clave -C "comentario"
```

---

## SSH - Gestionar Claves

```bash
# Ver claves existentes
ls -la ~/.ssh/

# Ver contenido de clave pública
cat ~/.ssh/id_ed25519.pub

# Copiar clave pública (macOS)
pbcopy < ~/.ssh/id_ed25519.pub

# Copiar clave pública (Linux)
cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard

# Copiar clave pública (Windows Git Bash)
cat ~/.ssh/id_ed25519.pub | clip

# Ver fingerprint de clave
ssh-keygen -lf ~/.ssh/id_ed25519.pub

# Cambiar passphrase
ssh-keygen -p -f ~/.ssh/id_ed25519

# Eliminar clave
rm ~/.ssh/id_ed25519 ~/.ssh/id_ed25519.pub
```

---

## SSH - Agent

```bash
# Iniciar SSH Agent (macOS/Linux)
eval "$(ssh-agent -s)"

# Iniciar SSH Agent (Windows Git Bash)
eval $(ssh-agent -s)

# Agregar clave al agent
ssh-add ~/.ssh/id_ed25519

# Ver claves en agent
ssh-add -l

# Ver claves con fingerprint
ssh-add -L

# Remover clave del agent
ssh-add -d ~/.ssh/id_ed25519

# Remover todas las claves
ssh-add -D

# Configurar auto-add (macOS, en ~/.ssh/config)
Host *
  AddKeysToAgent yes
  UseKeychain yes
```

---

## SSH - Configurar Git

```bash
# Ver remote actual
git remote -v

# Cambiar a SSH
git remote set-url origin git@github.com:usuario/repo.git

# Cambiar a HTTPS
git remote set-url origin https://github.com/usuario/repo.git

# Verificar conexión SSH GitHub
ssh -T git@github.com

# Verificar conexión SSH GitLab
ssh -T git@gitlab.com

# Verificar conexión SSH Bitbucket
ssh -T git@bitbucket.org
```

---

## SSH - Configuración Avanzada

```bash
# Ver configuración SSH
cat ~/.ssh/config

# Crear archivo de configuración
touch ~/.ssh/config
chmod 600 ~/.ssh/config

# Ejemplo de ~/.ssh/config
# Host github.com
#     HostName github.com
#     User git
#     IdentityFile ~/.ssh/id_ed25519
#     AddKeysToAgent yes
```

---

## SSH - Troubleshooting

```bash
# Probar conexión con verbose
ssh -vvv git@github.com

# Ver permisos de archivos SSH
ls -la ~/.ssh/
# Debe ser: -rw------- para archivos
# Debe ser: drwx------ para directorio

# Arreglar permisos
chmod 700 ~/.ssh/
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub

# Verificar que SSH busca la clave correcta
ssh -v git@github.com 2>&1 | grep "identity"
```

---

## GPG - Generar Claves

```bash
# Generar clave GPG
gpg --full-generate-key

# Generar clave rápido
gpg --quick-generate-key "Nombre <email@ejemplo.com>"

# Generar clave sin interfaz
gpg --batch --generate-key <<EOF
Key-Type: RSA
Key-Length: 4096
Name-Real: Tu Nombre
Name-Email: tu.email@ejemplo.com
Expire-Date: 0
%no-protection
EOF
```

---

## GPG - Gestionar Claves

```bash
# Listar claves privadas
gpg --list-secret-keys --keyid-format LONG

# Listar claves públicas
gpg --list-keys

# Ver clave específica
gpg --list-keys tu.email@ejemplo.com

# Exportar clave pública (ASCII)
gpg --armor --export tu.email@ejemplo.com

# Exportar clave pública (binaria)
gpg --export tu.email@ejemplo.com > mi-clave.gpg

# Exportar clave privada
gpg --armor --export-secret-keys tu.email@ejemplo.com

# Importar clave
gpg --import mi-clave.gpg

# Eliminar clave pública
gpg --delete-keys ID_CLAVE

# Eliminar clave privada
gpg --delete-secret-keys ID_CLAVE

# Ver ID de clave largo
gpg --keyid-format LONG --list-keys
```

---

## GPG - Configurar Git

```bash
# Ver ID de clave
gpg --list-secret-keys --keyid-format LONG
# Busca: sec   rsa4096/XXXXXXXXXXXXXXXX

# Configurar Git con ID de clave
git config --global user.signingkey XXXXXXXXXXXXXXXX

# Firmar commits automáticamente
git config --global commit.gpgsign true

# Verificar configuración
git config --global user.signingkey
git config --global commit.gpgsign
```

---

## GPG - Firmar Commits

```bash
# Firmar commit individual
git commit -S -m "Mensaje"

# Firmar con configuración automática
git commit -m "Mensaje"

# Enmendar commit para firmar
git commit --amend -S

# Firmar commit anterior
git commit -S --amend abc123def

# Firmar múltiples commits
git rebase --exec 'git commit --amend --no-edit -S' -i abc123def^
```

---

## GPG - Verificar Commits

```bash
# Ver commits con firmas
git log --show-signature

# Ver log con estado de firma
git log --oneline --pretty='%h %G? %s'

# Verificar commit específico
git verify-commit abc123def

# Ver detalles de firma
git show abc123def

# Significados %G?:
# G = firma buena (Good)
# B = firma mala (Bad)
# U = firma desconocida (Unknown)
# X = firma expirada (eXpired)
# Y = firma caduca pronto (expireY)
# R = firma revocada (Revoked)
# E = error
# N = sin firma (None)
```

---

## GPG - Agregar a Plataformas

```bash
# Exportar clave pública
gpg --armor --export tu.email@ejemplo.com > clave.asc

# Copiar clave
cat clave.asc

# GitHub: https://github.com/settings/keys
# GitLab: https://gitlab.com/-/profile/gpg_keys
# Bitbucket: https://bitbucket.org/account/settings/gpg-keys/

# Verificar en plataforma
# Hacer commit firmado y hacer push
# Debería mostrar "Verified" (GitHub/GitLab)
```

---

## GPG - Configurar Agent

```bash
# Editar configuración del agent
nano ~/.gnupg/gpg-agent.conf

# Agregar líneas para cachear passphrase
default-cache-ttl 600
max-cache-ttl 7200

# Reiniciar agent
gpgconf --kill gpg-agent

# O en Windows
gpg-connect-agent killagent /bye
gpg-connect-agent /bye
```

---

## GPG - Troubleshooting

```bash
# Probar GPG
echo "test" | gpg --clearsign

# Ver si GPG puede acceder al terminal
gpg --version

# Cambiar programa para ingresar passphrase
nano ~/.gnupg/gpg-agent.conf
# Agregar: pinentry-program /usr/bin/pinentry-tty

# Problemas con VSCode
# En configuración de Git:
git config --global core.editor "code --wait"
git config --global gpg.program "gpg"

# Problemas en macOS
# Instalar pinentry
brew install pinentry-mac

# Configurar
echo "pinentry-program /usr/local/bin/pinentry-mac" \
  >> ~/.gnupg/gpg-agent.conf
```

---

## Git Credentials Manager

```bash
# Ver helper configurado
git config --global credential.helper

# Configurar en Windows
git config --global credential.helper manager

# Configurar en macOS
git config --global credential.helper osxkeychain

# Configurar en Linux
git config --global credential.helper cache

# O almacenamiento persistente
git config --global credential.helper store

# Borrar credenciales guardadas (Windows)
cmdkey /delete:git:https://github.com

# Borrar credenciales guardadas (macOS)
security delete-internet-password -s github.com

# Ver credenciales (si tienes acceso)
security find-internet-password -s github.com -w
```

---

## HTTPS Token

```bash
# GitHub: Generar token en
# https://github.com/settings/tokens

# GitLab: Generar en
# https://gitlab.com/-/profile/personal_access_tokens

# Bitbucket: Generar en
# https://bitbucket.org/account/settings/app-passwords/

# Usar token como contraseña
git clone https://github.com/usuario/repo.git
# Nombre de usuario: tu-usuario-github
# Contraseña: tu-token-github
```

---

**Para más información:** [SSH vs HTTPS](/guides/es/autenticacion-ssh-https.md) | [Configuración GPG](/guides/es/configuracion-gpg.md)

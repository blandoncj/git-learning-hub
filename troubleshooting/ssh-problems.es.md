Soluciones específicas para problemas de autenticación SSH con Git.

## Tabla de Contenidos

1. [Problemas de Conexión](#problemas-de-conexión)
2. [Problemas de Permisos](#problemas-de-permisos)
3. [Problemas de Configuración](#problemas-de-configuración)
4. [Problemas con Agent](#problemas-con-agent)

---

## Problemas de Conexión

### "Permission denied (publickey)"

**Síntoma:** No puedes conectarte a GitHub/GitLab/Bitbucket.

```
Permission denied (publickey).
fatal: Could not read from remote repository.
```

**Diagnóstico:**

```bash
# Probar conexión con verbose
ssh -vvv git@github.com
```

**Causas comunes:**

1. **No tienes clave SSH**
2. **Clave no agregada al SSH agent**
3. **Clave pública no agregada a la plataforma**
4. **Permisos incorrectos en archivos**

**Solución completa:**

```bash
# 1. Verificar claves existentes
ls -la ~/.ssh/

# 2. Si no existe, generar clave
ssh-keygen -t ed25519 -C "tu.email@ejemplo.com"

# 3. Iniciar SSH agent
eval "$(ssh-agent -s)"

# 4. Agregar clave al agent
ssh-add ~/.ssh/id_ed25519

# 5. Copiar clave pública
cat ~/.ssh/id_ed25519.pub

# 6. Agregar a plataforma
# GitHub: https://github.com/settings/keys
# GitLab: https://gitlab.com/-/profile/keys
# Bitbucket: https://bitbucket.org/account/settings/ssh-keys/

# 7. Probar conexión
ssh -T git@github.com
```

---

### "ssh: connect to host github.com port 22: Connection refused"

**Síntoma:** El firewall bloquea el puerto 22.

**Solución:** Usar SSH sobre HTTPS (puerto 443):

```bash
# Editar ~/.ssh/config
nano ~/.ssh/config

# Agregar
Host github.com
  Hostname ssh.github.com
  Port 443
  User git

# Probar
ssh -T git@github.com
```

---

### "Connection timed out"

**Síntoma:** La conexión se agota.

**Causas:**

- Firewall corporativo
- VPN bloqueando
- Problema de red

**Solución:**

```bash
# Verificar conectividad
ping github.com

# Probar HTTPS en lugar de SSH
git remote set-url origin https://github.com/usuario/repo.git

# O configurar proxy si aplica
git config --global http.proxy http://proxy.ejemplo.com:8080
```

---

## Problemas de Permisos

### "bad permissions: ignore key"

**Síntoma:** SSH ignora tu clave por permisos incorrectos.

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/home/user/.ssh/id_rsa' are too open.
```

**Causa:** Los archivos SSH deben tener permisos restrictivos.

**Solución:**

```bash
# Arreglar permisos del directorio
chmod 700 ~/.ssh/

# Arreglar permisos de clave privada
chmod 600 ~/.ssh/id_ed25519

# Arreglar permisos de clave pública
chmod 644 ~/.ssh/id_ed25519.pub

# Arreglar config
chmod 600 ~/.ssh/config

# Verificar
ls -la ~/.ssh/
# Debe mostrar:
# drwx------  .ssh/
# -rw-------  id_ed25519
# -rw-r--r--  id_ed25519.pub
```

---

### "Host key verification failed"

**Síntoma:** SSH no puede verificar el host.

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
```

**Causas:**

- Cambio legítimo en el servidor
- Ataque man-in-the-middle (raro)

**Solución (si estás seguro que es legítimo):**

```bash
# Remover entrada del known_hosts
ssh-keygen -R github.com

# Conectar nuevamente (aceptar nueva clave)
ssh -T git@github.com
```

---

## Problemas de Configuración

### "Could not open a connection to your authentication agent"

**Síntoma:** SSH agent no está corriendo.

**Solución:**

```bash
# Iniciar SSH agent
eval "$(ssh-agent -s)"

# Agregar clave
ssh-add ~/.ssh/id_ed25519

# Verificar
ssh-add -l
```

---

### "sign_and_send_pubkey: no mutual signature supported"

**Síntoma:** Tu clave SSH usa algoritmo antiguo.

**Causa:** GitHub/GitLab deshabilitó algoritmos antiguos (RSA-SHA1).

**Solución:**

```bash
# Opción 1: Generar nueva clave ED25519
ssh-keygen -t ed25519 -C "tu.email@ejemplo.com"

# Opción 2: Usar RSA-SHA256 si necesitas RSA
# Editar ~/.ssh/config
Host github.com
  HostkeyAlgorithms +ssh-rsa
  PubkeyAcceptedAlgorithms +ssh-rsa
```

---

### Multiple SSH keys for different accounts

**Problema:** Tienes cuentas personales y de trabajo.

**Solución:**

```bash
# 1. Generar claves separadas
ssh-keygen -t ed25519 -f ~/.ssh/id_personal -C "personal@ejemplo.com"
ssh-keygen -t ed25519 -f ~/.ssh/id_trabajo -C "trabajo@empresa.com"

# 2. Configurar ~/.ssh/config
nano ~/.ssh/config

# 3. Agregar configuración
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_personal

Host github-trabajo
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_trabajo

# 4. Clonar usando el alias
git clone git@github-personal:usuario/repo-personal.git
git clone git@github-trabajo:empresa/repo-trabajo.git
```

---

## Problemas con Agent

### "Could not open a connection to your authentication agent"

**Síntoma:** SSH agent no está corriendo.

**Solución:**

```bash
# Iniciar agent
eval "$(ssh-agent -s)"

# Agregar clave
ssh-add ~/.ssh/id_ed25519

# Verificar que está cargada
ssh-add -l
```

---

### "Identity added" pero sigue pidiendo passphrase

**Síntoma:** SSH agent no persiste entre sesiones.

**Solución (macOS):**

```bash
# Editar ~/.ssh/config
nano ~/.ssh/config

# Agregar
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

**Solución (Linux):**

```bash
# Agregar a ~/.bashrc o ~/.zshrc
if [ -z "$SSH_AUTH_SOCK" ]; then
  eval $(ssh-agent -s)
  ssh-add ~/.ssh/id_ed25519
fi
```

---

### SSH agent tiene demasiadas claves

**Síntoma:** Error al intentar conectar.

```
Received disconnect from x.x.x.x: Too many authentication failures
```

**Solución:**

```bash
# Ver claves cargadas
ssh-add -l

# Remover todas las claves
ssh-add -D

# Agregar solo la necesaria
ssh-add ~/.ssh/id_ed25519

# O especificar en ~/.ssh/config
Host github.com
  IdentitiesOnly yes
  IdentityFile ~/.ssh/id_ed25519
```

---

## Diagnóstico Avanzado

### Probar conexión con verbose

```bash
# Nivel 1 de verbose
ssh -v git@github.com

# Nivel 3 (muy detallado)
ssh -vvv git@github.com

# Buscar en la salida:
# - "debug1: Offering public key" (clave siendo usada)
# - "debug1: Server accepts key" (clave aceptada)
# - "debug1: Authentication succeeded" (éxito)
```

---

### Verificar qué clave está usando Git

```bash
# Ver configuración actual
git config --get remote.origin.url

# Verificar con SSH
ssh -vT git@github.com 2>&1 | grep "identity file"
```

---

### Probar conexión sin config

```bash
# Probar con clave específica
ssh -i ~/.ssh/id_ed25519 -T git@github.com

# Probar puerto alternativo
ssh -p 443 -T git@ssh.github.com
```

---

## Tips de Prevención

### Configuración recomendada ~/.ssh/config

```bash
# GitHub
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes

# GitLab
Host gitlab.com
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes

# Bitbucket
Host bitbucket.org
  HostName bitbucket.org
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes
```

---

### Script de verificación

```bash
#!/bin/bash
# Guardar como check-ssh.sh

echo "=== Verificando configuración SSH ==="

# 1. Verificar claves
echo "1. Claves SSH:"
ls -la ~/.ssh/*.pub 2>/dev/null || echo "No hay claves públicas"

# 2. Verificar permisos
echo "2. Permisos:"
ls -ld ~/.ssh/
ls -l ~/.ssh/id_*

# 3. Verificar agent
echo "3. SSH Agent:"
ssh-add -l || echo "Agent no corriendo o sin claves"

# 4. Probar conexiones
echo "4. Probando GitHub:"
ssh -T git@github.com

echo "5. Probando GitLab:"
ssh -T git@gitlab.com

echo "6. Probando Bitbucket:"
ssh -T git@bitbucket.org
```

---

## Recursos Adicionales

- 📖 **Cheat Sheet SSH:** [/cheat-sheets/ssh-gpg-keys.md](/cheat-sheets/ssh-gpg-keys.md)
- 🔐 **Guía SSH vs HTTPS:** [/guias/es/autenticacion-ssh-https.md](/guias/es/autenticacion-ssh-https.md)
- 🛠️ **Problemas Comunes:** [problemas-comunes.md](problemas-comunes.md)

---

**¿Resolviste el problema?** Comparte tu solución o reporta problemas nuevos.

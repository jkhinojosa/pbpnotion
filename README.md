# Despliegue CI/CD para PBP Dashboards (Hostinger vía SSH)

Repositorio: [https://github.com/jkhinojosa/pbpnotion.git](https://github.com/jkhinojosa/pbpnotion.git)  
Dominio de producción: [https://pbpnotion.linkear.net](https://pbpnotion.linkear.net)

---

## ⚙️ Configuración de Secretos en GitHub

Para que GitHub Actions pueda conectarse a Hostinger y sincronizar los archivos automáticamente, debes agregar las siguientes variables secretas en tu repositorio:

👉 Ve a: **GitHub** > Tu Repositorio (`jkhinojosa/pbpnotion`) > **Settings** > **Secrets and variables** > **Actions** > **New repository secret**.

| Nombre del Secreto | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `SSH_HOST` | Dirección IP de tu servidor o dominio principal en Hostinger | `195.35.x.x` o `linkear.net` |
| `SSH_USER` | Usuario SSH de tu cuenta Hostinger | `u123456789` |
| `SSH_PRIVATE_KEY` | Llave privada SSH completa (incluyendo `-----BEGIN OPENSSH PRIVATE KEY-----` y `-----END ...-----`) | Llave privada generada |
| `SSH_PORT` | Puerto SSH de Hostinger (por defecto en Hostinger es 65002) | `65002` |
| `REMOTE_TARGET` | Ruta absoluta en el servidor donde apunta `pbpnotion.linkear.net` | `/home/u123456789/domains/linkear.net/public_html/pbpnotion/` |

---

## 🔍 ¿Cómo obtener estos datos en Hostinger (hPanel)?

1. **Datos de Conexión SSH**:
   - Ingresa a tu panel de Hostinger (hPanel).
   - Dirígete a **Avanzado** > **Acceso SSH** (o busca "SSH" en el buscador).
   - Habilita el acceso SSH si está deshabilitado.
   - Allí verás:
     - **IP de SSH** (úsalas en `SSH_HOST`)
     - **Puerto** (normalmente `65002`)
     - **Nombre de usuario** (úsalos en `SSH_USER`)

2. **Ruta en el servidor (`REMOTE_TARGET`)**:
   - Para un subdominio como `pbpnotion.linkear.net`, Hostinger suele colocar la carpeta en:
     `/home/<usuario>/domains/linkear.net/public_html/pbpnotion/`  
     o si fue creado como dominio independiente:  
     `/home/<usuario>/domains/pbpnotion.linkear.net/public_html/`
   - *Nota:* Asegúrate de incluir la barra diagonal final `/`.

3. **Configuración de Llaves SSH**:
   - En tu computadora local (o en Hostinger), genera un par de llaves:
     ```bash
     ssh-keygen -t ed25519 -C "github-actions-pbpnotion" -f ~/.ssh/pbpnotion_deploy
     ```
   - **Llave Pública (`pbpnotion_deploy.pub`)**: Copia su contenido y agrégala en Hostinger en **Acceso SSH** > **Claves SSH** (o pégala en `~/.ssh/authorized_keys`).
   - **Llave Privada (`pbpnotion_deploy`)**: Copia todo el contenido del archivo privado y pégalo en GitHub como `SSH_PRIVATE_KEY`.

---

## 🚀 Flujo de Trabajo (CI/CD)

- Cada vez que hagas `git push origin main`, GitHub Actions sincronizará automáticamente los cambios hacia Hostinger.
- También puedes disparar el despliegue manualmente desde la pestaña **Actions** en GitHub seleccionando **"Deploy to Hostinger via SSH"** y haciendo clic en **"Run workflow"**.

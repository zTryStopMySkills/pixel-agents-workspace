# Setup: GitHub Workspace Automatico

Cuando un host crea una sesion multiplayer en Pixel Agents, la extension crea automaticamente un repositorio en GitHub con todo el workspace. Esto permite que todos los agentes y companeros tengan el contexto completo del proyecto.

## Requisitos

### 1. Instalar GitHub CLI (`gh`)

**Windows** (con winget):
```bash
winget install --id GitHub.cli
```

**Windows** (con Scoop):
```bash
scoop install gh
```

**Windows** (con Chocolatey):
```bash
choco install gh
```

**macOS**:
```bash
brew install gh
```

**Linux (Debian/Ubuntu)**:
```bash
sudo apt install gh
```

**Linux (Fedora)**:
```bash
sudo dnf install gh
```

**Alternativa**: Descargar el instalador desde https://cli.github.com/

### 2. Autenticarse con GitHub

```bash
gh auth login
```

Sigue las instrucciones:
1. Selecciona **GitHub.com**
2. Selecciona **HTTPS** como protocolo
3. Selecciona **Login with a web browser** (o pega un token)
4. Copia el codigo que aparece y pegalo en el navegador
5. Autoriza la aplicacion

### 3. Verificar que funciona

```bash
gh auth status
```

Deberias ver algo como:
```
github.com
  ✓ Logged in to github.com account TU_USUARIO (keyring)
  - Active account: true
  - Git operations protocol: https
  - Token: gho_****
```

## Como Funciona

### Al crear una sesion multiplayer (Host)

1. Abres el panel Remote en Pixel Agents
2. Se crea la room automaticamente
3. **En background**, la extension:
   - Detecta si el workspace ya tiene un repo en GitHub
   - Si ya tiene: pushea los cambios mas recientes
   - Si no tiene: crea un repo publico `{nombre-carpeta}-workspace` en tu cuenta
   - Comparte la URL del repo con todos los guests que se conecten

### Al unirte a una sesion (Guest)

1. Entras el codigo de la room
2. El host te acepta
3. Recibes automaticamente:
   - El layout de la oficina
   - Los archivos del workspace (via File Sync)
   - La URL del repo en GitHub (visible en Remote Modal > "GitHub Workspace")

### Donde se ve el repo

En el **Remote Modal** (click en "Remote" en la barra inferior), aparece una seccion "GitHub Workspace" con:
- El nombre del repo (ej: `zTryStopMySkills/mi-proyecto-workspace`)
- Boton "Copy" para copiar la URL completa

## Sin GitHub CLI

Si no tienes `gh` instalado o no estas autenticado:
- El multiplayer **funciona normalmente** (file sync, chat, etc.)
- Solo se omite la creacion automatica del repo en GitHub
- Veras un log en la consola: `[Pixel Agents GitSync] Skipped: GitHub CLI (gh) not available`

## FAQ

**El repo se crea publico?**
Si, por defecto se crea publico para que todos los companeros puedan acceder sin configuracion adicional.

**Puedo usar un repo privado?**
Actualmente no desde la UI. Puedes crear el repo manualmente como privado, configurar el remote, y la extension lo usara automaticamente al detectar el remote existente.

**Que pasa si ya tengo un remote configurado?**
La extension lo detecta y usa ese repo. No crea uno nuevo.

**Con que frecuencia se pushea?**
Se pushea al iniciar la sesion multiplayer. Los cambios en tiempo real se sincronizan via File Sync (WebSocket), no via git push continuo.

**Puedo desactivar la creacion automatica?**
Si no quieres que se cree el repo, simplemente no instales `gh` o no te autentiques. El multiplayer funcionara sin esta feature.

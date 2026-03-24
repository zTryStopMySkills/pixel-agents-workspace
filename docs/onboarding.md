# Onboarding — Nuevos Miembros y Agentes

Guia para incorporarte rapidamente al proyecto Pixel Agents.

## Primeros 5 Minutos

### 1. Clonar y buildear

```bash
git clone https://github.com/pablodelucca/pixel-agents.git
cd pixel-agents
npm install
cd webview-ui && npm install && cd ..
npm run build
```

### 2. Ejecutar en modo desarrollo

- Abrir el proyecto en VS Code
- Presionar **F5** para lanzar Extension Development Host
- En la ventana nueva, buscar el panel "Pixel Agents" en la barra lateral

### 3. Probar la extension

- Click en **"+ Agent"** para lanzar un agente (necesitas Claude Code instalado)
- El personaje aparece en la oficina y refleja lo que el agente hace
- Click en **"Layout"** para abrir el editor de oficina

## Estructura del Proyecto (Resumen)

```
src/              → Extension backend (TypeScript, VS Code API)
webview-ui/       → Frontend React (TypeScript, Vite, Canvas 2D)
relay-server/     → Servidor multiplayer (Node.js + WebSocket)
scripts/          → Pipeline de extraccion de assets
docs/             → Documentacion del proyecto (ESTAS AQUI)
CLAUDE.md         → Referencia tecnica comprimida (para agentes AI)
```

Para detalles completos de arquitectura, ver [arquitectura.md](./arquitectura.md).

## Documentos que DEBES leer

| Prioridad | Documento | Para que |
|-----------|-----------|---------|
| 1 | [contexto-general.md](./contexto-general.md) | Que es el proyecto, vision, estado actual |
| 2 | [arquitectura.md](./arquitectura.md) | Como funciona todo, flujos principales |
| 3 | [roadmap.md](./roadmap.md) | Que esta hecho, que falta, prioridades |
| 4 | [decisiones-tecnicas.md](./decisiones-tecnicas.md) | Por que se tomo cada decision importante |
| 5 | `CLAUDE.md` (raiz) | Referencia tecnica comprimida (ideal para agentes AI) |

## Convenciones del Proyecto

### TypeScript
- **No enums** — usar objetos `as const`
- **`import type`** obligatorio para imports de solo tipos
- **`noUnusedLocals` / `noUnusedParameters`** activados
- **Constantes centralizadas**: `src/constants.ts` (backend) y `webview-ui/src/constants.ts` (webview)

### Git
- Branch principal: `main`
- Commits en ingles, descriptivos
- PRs con descripcion clara del cambio

### Estilo UI
- Estetica pixel art en toda la UI
- `borderRadius: 0` (sin esquinas redondeadas)
- Bordes `2px solid`, sombras offset sin blur
- Font: FS Pixel Sans
- Variables CSS: `--pixel-bg`, `--pixel-border`, `--pixel-accent`, etc.

## Si Eres un Agente AI

Ver la guia dedicada: [guia-agentes.md](./guia-agentes.md)

## Canales de Comunicacion

- **GitHub Issues**: Bugs y feature requests
- **GitHub Discussions**: Preguntas, propuestas de diseno, decisiones
- **Chat multiplayer**: Comunicacion en tiempo real dentro de la oficina (cuando el multiplayer este activo)

## Problemas Comunes

### El build falla
```bash
# Limpia y reinstala
rm -rf node_modules webview-ui/node_modules
npm install && cd webview-ui && npm install && cd ..
npm run build
```

### No veo el panel Pixel Agents
- Asegurate de estar en la ventana Extension Development Host (F5)
- Busca "Pixel Agents" en la barra de actividad o abre la vista desde el command palette

### Los agentes no aparecen
- Necesitas tener Claude Code instalado (`claude` disponible en terminal)
- El JSONL tarda ~1s en ser detectado despues de lanzar el agente

### fs.watch no funciona en Windows
- Es un problema conocido. El polling backup (cada 2s) se encarga automaticamente.

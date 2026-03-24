# Pixel Agents — Contexto General del Proyecto

## Que es Pixel Agents

Pixel Agents es una **extension de VS Code** que convierte tus terminales de Claude Code en personajes animados de pixel art dentro de una oficina virtual. Cada agente AI que lanzas aparece como un personaje que camina, se sienta, escribe y muestra su estado en tiempo real.

**Repositorio**: https://github.com/pablodelucca/pixel-agents
**Publisher**: pablodelucca (VS Code Marketplace)
**Version actual**: 1.3.1

## Vision

Crear un **coworking virtual de agentes AI** donde:
- Multiples agentes trabajan visualmente en una oficina pixel art
- Equipos remotos comparten la misma oficina via multiplayer
- Cada agente muestra su actividad en tiempo real (que herramienta usa, si espera permisos, etc.)
- La oficina es completamente personalizable (layout editor con muebles, suelos, paredes)

## Estado actual (Marzo 2026)

### Funcional (en produccion)
- Oficina pixel art con personajes animados por agente
- Layout editor completo (suelo, paredes, muebles, colores)
- Tracking de estado via JSONL transcripts
- Persistencia de layout en `~/.pixel-agents/layout.json`
- Sub-agentes como personajes con IDs negativos
- Sonido de notificacion al completar turno
- 6 paletas de personajes + hue shift para variedad infinita
- Efecto Matrix para spawn/despawn

### En desarrollo (rama actual)
- **Multiplayer**: Host/Guest via WebSocket relay server
  - Relay server en `relay-server/` (Node.js + ws, deploy en Railway)
  - Room system con codigos de 6 caracteres
  - File sync entre host y guests
  - Chat en tiempo real
  - Admin panel para gestion de peers
- **Roles y departamentos**: Host, Admin, roles custom con colores
- **Reacciones**: Emojis entre agentes (6 reacciones rapidas)
- **Gestos**: Walk-to (click derecho en agente remoto)
- **Activity Feed**: Panel lateral con eventos en tiempo real
- **Stats**: Modal con metricas por departamento (tokens, tools, tiempo)
- **Transcript viewer**: Lectura de transcripts por agente con anotaciones

## Stack Tecnologico

| Capa | Tecnologia |
|------|-----------|
| Extension backend | TypeScript, VS Code API, esbuild |
| Webview UI | React + TypeScript, Vite |
| Rendering | Canvas 2D (pixel-perfect, sin framework 2D) |
| Multiplayer relay | Node.js + ws (WebSocket) |
| Persistencia | JSONL transcripts, JSON layout file, VS Code globalState |
| Assets | PNGs procesados via pngjs, catalogo JSON |

## Equipo

| Quien | Rol | Notas |
|-------|-----|-------|
| pablodelucca | Creador/Maintainer | Publisher en VS Code Marketplace |
| jose2 | Desarrollo multiplayer | Branch actual con features multiplayer |
| Agentes AI | Desarrollo asistido | Claude Code como herramienta principal |

## Links Importantes

- **GitHub Issues**: https://github.com/pablodelucca/pixel-agents/issues
- **GitHub Discussions**: https://github.com/pablodelucca/pixel-agents/discussions
- **VS Code Marketplace**: Buscar "Pixel Agents" por pablodelucca
- **Relay Server**: Desplegado en Railway (ver `relay-server/`)

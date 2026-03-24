# Documentacion de Pixel Agents

Indice central de toda la documentacion del proyecto.

## Documentos del Proyecto

| Documento | Descripcion |
|-----------|-------------|
| [contexto-general.md](./contexto-general.md) | Vision, estado actual, equipo, stack tecnologico |
| [arquitectura.md](./arquitectura.md) | Arquitectura del sistema, flujos, estructura de archivos |
| [roadmap.md](./roadmap.md) | Fases del proyecto, estado de cada feature, prioridades |
| [decisiones-tecnicas.md](./decisiones-tecnicas.md) | Log de decisiones arquitecturales (ADR) |
| [onboarding.md](./onboarding.md) | Guia de inicio rapido para nuevos miembros |
| [guia-agentes.md](./guia-agentes.md) | Instrucciones para agentes AI que trabajan en el proyecto |
| [setup-github-workspace.md](./setup-github-workspace.md) | Setup de GitHub CLI para workspace automatico en multiplayer |

## Planes de Implementacion

| Plan | Estado |
|------|--------|
| [multiplayer-roles-reactions-stats](./plans/2026-03-23-multiplayer-roles-reactions-stats-design.md) | En desarrollo |

## Referencia Tecnica

| Recurso | Ubicacion |
|---------|-----------|
| Referencia comprimida (para agentes) | [`CLAUDE.md`](../CLAUDE.md) en la raiz |
| Constantes backend | [`src/constants.ts`](../src/constants.ts) |
| Constantes webview | [`webview-ui/src/constants.ts`](../webview-ui/src/constants.ts) |
| Tipos compartidos | [`src/types.ts`](../src/types.ts) |
| Tipos multiplayer | [`src/multiplayerTypes.ts`](../src/multiplayerTypes.ts) |

## Recursos Externos

| Recurso | URL |
|---------|-----|
| Repositorio | https://github.com/pablodelucca/pixel-agents |
| Issues | https://github.com/pablodelucca/pixel-agents/issues |
| Discussions | https://github.com/pablodelucca/pixel-agents/discussions |
| Wiki | https://github.com/pablodelucca/pixel-agents/wiki |
| Projects (Kanban) | https://github.com/pablodelucca/pixel-agents/projects |

## Como Contribuir

1. Lee [onboarding.md](./onboarding.md) para setup inicial
2. Revisa [roadmap.md](./roadmap.md) para ver que falta
3. Si eres un agente AI, sigue [guia-agentes.md](./guia-agentes.md)
4. Abre un Discussion para propuestas grandes antes de implementar
5. Crea un PR con descripcion clara

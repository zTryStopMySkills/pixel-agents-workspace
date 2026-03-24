# Roadmap de Pixel Agents

## Fase 1 — Core (COMPLETADA)
- [x] Oficina pixel art con personajes animados
- [x] Un personaje por terminal de Claude Code
- [x] Tracking de estado via JSONL (active/idle/waiting)
- [x] Animaciones por tipo de herramienta (typing vs reading)
- [x] Burbujas de estado (permission, waiting)
- [x] Persistencia de agentes entre sesiones
- [x] Efecto Matrix para spawn/despawn

## Fase 2 — Layout Editor (COMPLETADA)
- [x] Editor de suelos (7 patrones, colorizables HSBC)
- [x] Editor de paredes (auto-tile bitmask)
- [x] Colocacion de muebles (catalogo dinamico, rotacion, estados on/off)
- [x] Drag-to-move, undo/redo (50 niveles)
- [x] Expansion dinamica del grid (hasta 64x64)
- [x] Export/Import de layouts
- [x] Layout default bundled
- [x] Muebles en superficies (laptops sobre escritorios)
- [x] Items de pared (cuadros, ventanas, relojes)
- [x] Background tiles para muebles altos

## Fase 3 — Multiplayer (EN DESARROLLO)
- [x] Relay server (Node.js + ws, Railway)
- [x] Sistema de rooms con codigos de 6 caracteres
- [x] Host/Guest join flow con aceptacion
- [x] Broadcast de eventos de agentes entre peers
- [x] Chat en tiempo real
- [x] File sync entre host y guests
- [x] Admin panel basico
- [ ] **Roles y departamentos** (Host, Admin, custom)
- [ ] **Reacciones** (emojis entre agentes)
- [ ] **Gestos** (walk-to agente remoto)
- [ ] **Activity Feed** (panel lateral con eventos)
- [ ] **Stats por departamento** (tokens, tools, tiempo)
- [ ] **Transcript viewer** con anotaciones

## Fase 4 — Contexto Compartido (PLANIFICADA)
- [ ] Documentacion completa en GitHub (docs/, Wiki)
- [ ] GitHub Discussions para decisiones tecnicas
- [ ] GitHub Projects para tracking de tareas
- [ ] Onboarding automatizado para nuevos agentes/miembros
- [ ] Guia de contribucion

## Fase 5 — Ideas Futuras
- [ ] Marketplace de layouts/temas de oficina
- [ ] Mas tipos de muebles interactivos
- [ ] Integracion con otros AI agents (no solo Claude)
- [ ] Dashboard web standalone (fuera de VS Code)
- [ ] Persistencia de stats en servidor

---

## Prioridades Actuales (Marzo 2026)

1. **Completar multiplayer** — Roles, reacciones, activity feed, stats
2. **Documentacion compartida** — Para que todo el equipo tenga contexto
3. **Estabilidad** — Testing del relay server bajo carga

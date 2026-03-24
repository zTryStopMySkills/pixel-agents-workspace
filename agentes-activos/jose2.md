# jose2

## Info

- **GitHub**: @zTryStopMySkills
- **Rol**: Desarrollo multiplayer
- **Fecha de incorporacion**: 2026-03-23

## Trabajando en

- [ ] Sistema de roles y departamentos (Host, Admin, custom)
- [ ] Reacciones entre agentes (emojis)
- [ ] Activity Feed (panel lateral)
- [ ] Stats por departamento

## Archivos que estoy tocando

- `src/multiplayerHost.ts` — Logica del host, roles, admin permissions
- `src/multiplayerClient.ts` — Logica del guest
- `src/multiplayerTypes.ts` — Tipos compartidos multiplayer
- `src/multiplayerBroadcast.ts` — Broadcast de eventos
- `webview-ui/src/components/AdminPanel.tsx` — UI de admin
- `webview-ui/src/components/ActivityFeedPanel.tsx` — Feed de actividad
- `webview-ui/src/components/StatsModal.tsx` — Modal de estadisticas
- `webview-ui/src/hooks/useMultiplayer.ts` — Hook multiplayer

## Notas

Branch actual tiene todo el sistema multiplayer basico funcionando (relay server, rooms, chat, file sync). Fase actual: roles, reacciones y visibilidad de trabajo.

## Ultima actualizacion

2026-03-24 00:30

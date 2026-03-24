# Arquitectura de Pixel Agents

## Diagrama General

```
+------------------+     postMessage      +-------------------+
|   Extension      | <------------------> |    Webview (React) |
|   Backend        |                      |                   |
|  (Node.js)       |                      |  Canvas 2D        |
|                  |                      |  Game Loop (rAF)  |
|  - AgentManager  |                      |  OfficeState      |
|  - FileWatcher   |                      |  Characters FSM   |
|  - TimerManager  |                      |  Layout Editor    |
|  - LayoutPersist |                      |  Multiplayer UI   |
+--------+---------+                      +---------+---------+
         |                                          |
         |  fs.watch + polling                      |  WebSocket
         v                                          v
+------------------+                      +-------------------+
| JSONL Transcripts|                      |  Relay Server     |
| ~/.claude/       |                      |  (Node.js + ws)   |
| projects/...     |                      |  Railway deploy   |
+------------------+                      +-------------------+
```

## Flujo Principal: Un Agente Trabaja

```
1. Usuario click "+ Agent"
2. Extension crea terminal con `claude --session-id <uuid>`
3. AgentManager registra el agente y notifica webview (agentCreated)
4. FileWatcher busca ~/.claude/projects/<hash>/<uuid>.jsonl (poll 1s)
5. Cuando encuentra el JSONL, inicia fs.watch + polling backup (2s)
6. Cada linea nueva del JSONL se parsea (transcriptParser):
   - tool_use → agentToolStart → webview muestra animacion de typing/reading
   - tool_result → agentToolDone (con 300ms delay anti-flicker)
   - system + turn_duration → agentStatus 'waiting' → burbuja verde
7. Webview actualiza OfficeState → personaje cambia animacion
8. Game loop (requestAnimationFrame) renderiza en canvas
```

## Flujo Multiplayer

```
Host                    Relay Server              Guest
  |                         |                        |
  |--- createRoom --------->|                        |
  |<-- roomCreated (code) --|                        |
  |                         |<--- joinRoom (code) ---|
  |<-- joinRequest ---------|                        |
  |--- acceptPeer --------->|                        |
  |                         |--- welcome ----------->|
  |                         |                        |
  |=== Bidirectional sync via relay ================|
  |  agentEvents, chat, layout, file sync           |
  |=================================================|
```

## Estructura de Archivos Clave

### Extension Backend (`src/`)

| Archivo | Responsabilidad |
|---------|----------------|
| `extension.ts` | Punto de entrada: activate/deactivate |
| `PixelAgentsViewProvider.ts` | WebviewViewProvider, despacho de mensajes, carga de assets |
| `agentManager.ts` | Ciclo de vida de terminales: lanzar, eliminar, restaurar, persistir |
| `fileWatcher.ts` | fs.watch + polling, lectura de lineas nuevas, deteccion de /clear |
| `transcriptParser.ts` | Parseo JSONL: tool_use/tool_result a mensajes webview |
| `timerManager.ts` | Logica de timers para waiting/permission |
| `layoutPersistence.ts` | I/O del layout file, migracion, watch cross-window |
| `multiplayerHost.ts` | Logica del host multiplayer |
| `multiplayerClient.ts` | Logica del guest multiplayer |
| `multiplayerBroadcast.ts` | Broadcast de eventos entre peers |
| `multiplayerTypes.ts` | Tipos compartidos multiplayer |

### Webview UI (`webview-ui/src/`)

| Archivo | Responsabilidad |
|---------|----------------|
| `App.tsx` | Composicion root, hooks + componentes |
| `office/engine/officeState.ts` | Estado del juego: layout, personajes, asientos |
| `office/engine/renderer.ts` | Renderizado canvas: tiles, entidades z-sorted, overlays |
| `office/engine/characters.ts` | FSM de personajes: idle/walk/type + wander AI |
| `office/engine/gameLoop.ts` | Loop rAF con delta time |
| `hooks/useExtensionMessages.ts` | Handler de mensajes + estado de agentes/tools |
| `hooks/useMultiplayer.ts` | Estado y logica multiplayer en webview |
| `components/BottomToolbar.tsx` | Toolbar inferior: + Agent, Layout, Settings, Chat, Stats |

### Relay Server (`relay-server/`)

| Archivo | Responsabilidad |
|---------|----------------|
| `server.js` | WebSocket server: rooms, routing host/guest, ping/pong |

## Patrones Arquitecturales

### 1. Comunicacion Extension <-> Webview
- `postMessage` bidireccional
- Protocolo definido por tipos de mensaje (`openClaude`, `agentCreated`, `layoutLoaded`, etc.)
- Extension es la fuente de verdad para estado de agentes y layout

### 2. Rendering Imperativo
- `OfficeState` es una clase imperativa (NO React state)
- Game loop con `requestAnimationFrame` y delta time
- Zoom = pixels enteros por pixel de sprite (pixel-perfect)
- Z-sort de todas las entidades por coordenada Y

### 3. File Watching Hibrido
- `fs.watch` como mecanismo primario (puede fallar en Windows)
- Polling cada 2s como backup
- Buffer de lineas parciales para lecturas mid-write

### 4. Multiplayer via Relay
- Relay server es un router puro (no almacena estado de juego)
- Host es la fuente de verdad
- Guests reciben snapshots y eventos incrementales
- WebSocket con ping/pong para deteccion de desconexion

# Multiplayer: Roles, Reactions & Work Visibility

**Date:** 2026-03-23
**Status:** Design approved, pending implementation

---

## Context

Three coordinated improvements to the multiplayer experience:

1. **Roles system** — clarify the host/guest flow with named roles and departments
2. **Reactions & gestures** — peer-to-peer interaction between agents
3. **Work visibility** — real-time activity feed and productivity stats by department

---

## 1. Roles System

### Role types

| Role | Created by | Quantity | Description |
|------|-----------|----------|-------------|
| `Host` | Automatic | 1 | Full control. Not assignable. |
| `Admin` | Host | Unlimited (host decides) | Room management permissions. |
| Custom | Host | Unlimited | Free name + color (e.g. "Backend", "Frontend") |

### Admin permissions

| Action | Host | Admin | Guest |
|--------|------|-------|-------|
| Create/delete roles | ✅ | ❌ | ❌ |
| Assign roles to peers | ✅ | ✅ | ❌ |
| Accept/reject join requests | ✅ | ✅ | ❌ |
| Kick peers | ✅ | ✅ | ❌ |
| Launch agents | ✅ | ✅ | ✅ |

### Data model

```typescript
interface Role {
  id: string;        // UUID
  name: string;      // "Admin" is reserved; rest are free
  color: string;     // hex color e.g. "#4ade80"
  isAdmin?: boolean; // true only for the Admin role
}
```

### Persistence & sync

- Roles persisted in host `globalState` under key `pixel-agents.roles`
- `roleId?` added to `WS_Welcome` and `WS_PeerJoined` wire messages
- Admins receive the same `joinRequest` messages as the host
- Admins can send `acceptPeer`, `rejectPeer`, `kickPeer`

### Visual

- Badge below character name tag with role name and role color background
- `Character` gains `roleName?: string` and `roleColor?: string`
- Agents of a peer inherit the peer's role badge automatically

### UI changes

- `AdminPanel.tsx`: new "Roles" section — "+ New Role" button, color picker, editable name
- `AdminPanel.tsx`: peer list shows role dropdown (host + admins can change)
- Role badge rendered in `renderer.ts` below the existing name tag

---

## 2. Reactions & Gestures

### Quick reactions

- Hover any agent → floating reaction picker appears (6 emojis: 👍 ✅ 🔥 ❓ ⚡ 🎉)
- Click an emoji → bubble appears above the target agent for ~2.5s then fades
- Broadcast to all peers via `broadcastAgentEvent`

```typescript
// New AgentEventPayload kind
| { kind: 'reaction'; agentId: number; emoji: string }
```

- Reuses existing bubble system; `Character` gains `bubbleEmoji?: string`
- New `bubbleType` value: `'reaction'` (distinct from `'permission'` and `'waiting'`)

### Gestures (walk-to)

- Right-click a remote agent → local agent pathfinds to the nearest free adjacent tile
- On arrival: local agent faces the remote agent
- Broadcast `{ kind: 'gesture'; agentId: number; targetAgentId: number }`
- Remote agent also turns to face the approaching local agent

---

## 3. Work Visibility

### Real-time activity feed

- Toggleable side panel (right side, same style as Chat panel)
- "Feed" button in `BottomToolbar` opens/closes it — same pattern as Chat (`isFeedOpen` state)
- Stream of the last 50 events across all peers and agents
- Format: `[RoleName] Agent #N  →  <action>  <relative time>`
- Built from existing events: `agentToolStart`, `agentStatus`, `agentTokenUpdate` — no new WS messages needed

### Productivity stats

- Modal accessible via new "Stats" button in `BottomToolbar` (next to "Chat")
- Table grouped by role/department:

```
Department    Peers   Tokens    Tools used   Active time
──────────────────────────────────────────────────────
Backend         2     24.3k        142         1h 12m
Frontend        1      8.7k         67         0h 45m
[No role]       1      1.2k         11         0h 08m
```

- Data accumulated locally in React state from incoming events
- No backend persistence required

---

## Files to modify

| File | Change |
|------|--------|
| `src/multiplayerTypes.ts` | Add `roleId?` to `WS_Welcome`, `WS_PeerJoined`; add `reaction`, `gesture` kinds to `AgentEventPayload` |
| `src/multiplayerHost.ts` | Forward `joinRequest` to Admins; validate `acceptPeer`/`rejectPeer`/`kickPeer` from Admins |
| `src/PixelAgentsViewProvider.ts` | Persist/load roles in `globalState`; handle new role management messages |
| `webview-ui/src/office/types.ts` | Add `roleName?`, `roleColor?`, `bubbleEmoji?` to `Character` |
| `webview-ui/src/office/engine/officeState.ts` | `setAgentRole()`, `showReactionBubble()` |
| `webview-ui/src/office/engine/renderer.ts` | Render role badge; render emoji bubble |
| `webview-ui/src/hooks/useMultiplayer.ts` | Handle `reaction`, `gesture` events; apply role to characters |
| `webview-ui/src/components/AdminPanel.tsx` | Role management UI; peer role assignment |
| `webview-ui/src/components/BottomToolbar.tsx` | Add "Stats" button |
| `webview-ui/src/components/ActivityFeed.tsx` | New component — real-time event feed panel |
| `webview-ui/src/components/StatsModal.tsx` | New component — productivity stats modal |

---

## Verification

1. Host crea roles → se persisten al reabrir VS Code
2. Host asigna Admin a un peer → ese peer puede aceptar/rechazar/expulsar
3. Badge de rol visible sobre los personajes de cada peer
4. Click en emoji sobre un agente → burbuja aparece en todos los peers conectados
5. Click derecho en agente remoto → agente local camina hacia él y ambos se miran
6. Feed muestra actividad en tiempo real de todos los agentes
7. Modal Stats muestra tokens y herramientas agrupados por departamento
8. `npm run build` sin errores TypeScript

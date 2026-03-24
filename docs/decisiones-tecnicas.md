# Decisiones Tecnicas (ADR Log)

Registro de decisiones tecnicas importantes y el razonamiento detras de cada una.

---

## ADR-001: WebviewViewProvider en vez de WebviewPanel

**Fecha**: Inicio del proyecto
**Estado**: Aceptada

**Contexto**: Necesitabamos mostrar la oficina pixel art dentro de VS Code.

**Decision**: Usar `WebviewViewProvider` que vive en el panel lateral (junto al terminal) en vez de `WebviewPanel` que abre una tab separada.

**Razon**: El usuario necesita ver la oficina Y el terminal al mismo tiempo. Un panel lateral es mas natural para monitoreo continuo.

---

## ADR-002: Canvas 2D nativo sin framework de juego

**Fecha**: Inicio del proyecto
**Estado**: Aceptada

**Contexto**: Renderizar una oficina pixel art con personajes animados.

**Decision**: Canvas 2D puro con `requestAnimationFrame`, sin Phaser/PixiJS/etc.

**Razon**:
- Control total sobre pixel-perfect rendering (zoom = enteros)
- Sin dependencias pesadas en el webview
- La complejidad del juego es manejable sin framework

---

## ADR-003: JSONL watching en vez de hooks o IPC

**Fecha**: Fase 1
**Estado**: Aceptada

**Contexto**: Necesitabamos saber que esta haciendo cada agente Claude en su terminal.

**Decision**: Leer los archivos JSONL que Claude Code escribe en `~/.claude/projects/`.

**Alternativas descartadas**:
- Hook-based IPC: Los hooks se capturan al inicio, las env vars no propagan
- `--output-format stream-json`: Necesita stdin no-TTY, incompatible con terminales VS Code

**Razon**: Los JSONL son estables, append-only, y no requieren modificar Claude Code.

---

## ADR-004: fs.watch + polling hibrido

**Fecha**: Fase 1
**Estado**: Aceptada

**Contexto**: `fs.watch` es unreliable en Windows (puede perder eventos).

**Decision**: Usar `fs.watch` como primario + polling cada 2s como backup.

**Razon**: Garantiza que ningun evento se pierde en ninguna plataforma.

---

## ADR-005: Layout persistido en archivo, no en workspaceState

**Fecha**: Fase 2
**Estado**: Aceptada

**Contexto**: El layout de la oficina necesita compartirse entre ventanas de VS Code.

**Decision**: Persistir en `~/.pixel-agents/layout.json` (nivel usuario) con file watching para sync cross-window.

**Alternativa descartada**: `workspaceState` de VS Code (aislado por workspace).

**Razon**: Un usuario quiere la misma oficina en todos sus proyectos.

---

## ADR-006: Relay server puro (sin estado de juego)

**Fecha**: Fase 3 - Multiplayer
**Estado**: Aceptada

**Contexto**: Necesitabamos conectar host y guests para multiplayer.

**Decision**: El relay server solo routea mensajes WebSocket entre host y guests. No almacena estado del juego.

**Razon**:
- Host es la fuente de verdad (simplifica consistencia)
- Relay server es trivial de mantener y escalar
- Menor latencia (no procesa logica de juego)

---

## ADR-007: No usar enums de TypeScript

**Fecha**: Todo el proyecto
**Estado**: Aceptada

**Contexto**: `erasableSyntaxOnly` en tsconfig.

**Decision**: Usar objetos `as const` en lugar de `enum`.

**Razon**: Compatibilidad con el modo de TypeScript del proyecto. Los objetos `as const` funcionan identicamente para nuestro caso de uso.

---

## ADR-008: Constantes centralizadas

**Fecha**: Todo el proyecto
**Estado**: Aceptada

**Decision**: Todos los magic numbers en `src/constants.ts` (backend) y `webview-ui/src/constants.ts` (webview). Nunca inline.

**Razon**: Un solo lugar para ajustar timing, dimensiones, colores, etc. Evita drift entre valores duplicados.

---

## Como agregar una nueva decision

Copia esta plantilla:

```markdown
## ADR-XXX: Titulo breve

**Fecha**: YYYY-MM-DD
**Estado**: Propuesta | Aceptada | Rechazada | Reemplazada por ADR-YYY

**Contexto**: Que problema estamos resolviendo.

**Decision**: Que decidimos hacer.

**Alternativas descartadas** (opcional): Que otras opciones consideramos.

**Razon**: Por que esta decision es la mejor opcion.
```

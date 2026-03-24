# Guia para Agentes AI

Instrucciones para agentes AI (Claude Code u otros) que trabajan en este proyecto.

## Al Unirte al Proyecto

### Paso 1: Leer contexto (en este orden)

1. **`CLAUDE.md`** (raiz del proyecto) — Referencia tecnica comprimida. Es tu documento principal.
2. **`docs/contexto-general.md`** — Vision, estado actual, equipo
3. **`docs/roadmap.md`** — Que esta hecho y que falta
4. **`docs/decisiones-tecnicas.md`** — ADRs con el "por que" de cada decision

### Paso 2: Entender donde trabajas

- **Backend**: `src/` — Extension de VS Code, TypeScript, esbuild
- **Frontend**: `webview-ui/src/` — React + Vite, Canvas 2D
- **Multiplayer**: `src/multiplayer*.ts` + `relay-server/`
- **Assets**: `scripts/` (pipeline), `webview-ui/public/assets/` (output)

### Paso 3: Verificar que buildea

```bash
npm run build
```

Si falla, leer los errores de TypeScript. Las restricciones mas comunes:
- No enums (`erasableSyntaxOnly`)
- `import type` para tipos (`verbatimModuleSyntax`)
- No variables/parametros sin usar

## Reglas al Escribir Codigo

### Constantes
- **NUNCA** agregar magic numbers/strings inline
- Backend: `src/constants.ts`
- Webview: `webview-ui/src/constants.ts`
- CSS: variables en `webview-ui/src/index.css` `:root`

### Nuevos archivos
- Preferir editar archivos existentes antes de crear nuevos
- Si creas un archivo nuevo, actualizar `CLAUDE.md` con su descripcion

### Tipos
- Interfaces compartidas en `src/types.ts` (backend) o `webview-ui/src/office/types.ts` (webview)
- Tipos multiplayer en `src/multiplayerTypes.ts`

### Mensajes Extension <-> Webview
- Protocolo documentado en `CLAUDE.md` seccion "Extension <-> Webview"
- Nuevos mensajes deben seguir el patron existente

## Reglas al Documentar Cambios

### Cuando actualizar docs

| Cambio | Actualizar |
|--------|-----------|
| Nuevo archivo/modulo | `CLAUDE.md` (seccion Architecture) |
| Nueva feature completa | `docs/roadmap.md` (marcar completada) |
| Decision tecnica importante | `docs/decisiones-tecnicas.md` (nuevo ADR) |
| Cambio en API/protocolo | `CLAUDE.md` (seccion relevante) |
| Bug conocido o workaround | `CLAUDE.md` (seccion Condensed Lessons) |

### Cuando NO actualizar docs
- Cambios triviales de implementacion
- Refactors que no cambian la API publica
- Fix de bugs sin impacto arquitectural

## Plan de Trabajo Actual

Consultar `docs/plans/` para planes de implementacion detallados. El plan activo es:
- `2026-03-23-multiplayer-roles-reactions-stats-design.md` — Roles, reacciones, activity feed, stats

## Como Comunicar Progreso

1. **Commits**: Mensajes descriptivos en ingles
2. **PRs**: Descripcion con "Summary" y "Test plan"
3. **GitHub Discussions**: Para propuestas que necesitan alineamiento del equipo
4. **Actualizar roadmap**: Marcar items completados en `docs/roadmap.md`

## Checklist Pre-Commit

- [ ] `npm run build` pasa sin errores
- [ ] No hay magic numbers/strings inline (usar archivos de constantes)
- [ ] No hay imports sin usar
- [ ] Los nuevos archivos estan documentados en `CLAUDE.md`
- [ ] Si es una feature nueva, actualizar `docs/roadmap.md`
- [ ] Si es una decision tecnica, agregar ADR en `docs/decisiones-tecnicas.md`

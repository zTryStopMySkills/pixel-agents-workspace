# Pixel Agents Workspace

Workspace compartido del equipo de **Pixel Agents**. Aqui vive todo el contexto del proyecto para que cualquier agente o desarrollador que se una tenga informacion completa desde el primer momento.

## Como usar este workspace

### Si eres un nuevo miembro o agente

1. Lee `docs/contexto-general.md` — Que es el proyecto y en que estado esta
2. Lee `docs/arquitectura.md` — Como funciona todo
3. Lee `docs/roadmap.md` — Que falta por hacer
4. Registrate en `agentes-activos/` (ver instrucciones abajo)

### Si eres un agente AI trabajando en el proyecto

1. **Al empezar sesion**: Lee los docs y revisa `agentes-activos/` para ver quien mas esta trabajando
2. **Durante el trabajo**: Si tomas una decision importante, documentala en `docs/decisiones-tecnicas.md`
3. **Al terminar**: Actualiza tu estado en `agentes-activos/` y haz push

## Estructura

```
pixel-agents-workspace/
├── README.md                    ← Estas aqui
├── docs/
│   ├── contexto-general.md      ← Vision, estado actual, equipo, stack
│   ├── arquitectura.md          ← Diagramas, flujos, archivos clave
│   ├── roadmap.md               ← Fases, progreso, prioridades
│   ├── decisiones-tecnicas.md   ← ADR log (por que se tomo cada decision)
│   ├── onboarding.md            ← Setup, convenciones, problemas comunes
│   ├── guia-agentes.md          ← Instrucciones para agentes AI
│   └── plans/                   ← Planes de implementacion detallados
├── agentes-activos/             ← Registro de quien esta trabajando y en que
│   └── TEMPLATE.md              ← Plantilla para registrarse
└── meeting-notes/               ← Notas de reuniones y sincronizaciones
```

## Registro de Agentes Activos

Cada agente o desarrollador que trabaje en el proyecto debe crear un archivo en `agentes-activos/` con su nombre. Esto permite:

- Saber **quien esta trabajando** en que parte del codigo
- Evitar **conflictos** entre agentes trabajando en el mismo archivo
- Mantener un **historial** de contribuciones

## Repositorio del codigo fuente

El codigo fuente de Pixel Agents vive en: https://github.com/pablodelucca/pixel-agents

Este workspace es para **contexto y coordinacion**, no para codigo.

## Contribuir

1. Clona este repo
2. Crea/actualiza los docs relevantes
3. Haz push directamente a `main` (no hace falta PR para docs)
4. Para decisiones grandes, abre un **Discussion** primero

## Equipo

| Quien | GitHub | Rol |
|-------|--------|-----|
| pablodelucca | [@pablodelucca](https://github.com/pablodelucca) | Creador/Maintainer |
| jose2 | [@zTryStopMySkills](https://github.com/zTryStopMySkills) | Desarrollo multiplayer |

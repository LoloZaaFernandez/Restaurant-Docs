# Restaurant SaaS — Docs (Obsidian Vault)

Segunda cerebro técnico del proyecto. Acá vive el **contexto, las decisiones y la arquitectura**. Las tareas y el progreso están en Notion.

---

## Setup (primera vez)

### 1. Clonar el repo
```bash
git clone <url-del-repo> Docs
```

### 2. Abrir en Obsidian
- Abrir Obsidian → "Open folder as vault"
- Seleccionar la carpeta `Docs/`

### 3. Instalar plugins de la comunidad
En Obsidian: `Settings → Community plugins → Browse` e instalar:

| Plugin | Para qué |
|---|---|
| **Obsidian Git** | Sync automático con el repo |
| **Dataview** | Consultar notas como base de datos |
| **Templater** | Templates para ADRs, reuniones, módulos |
| **Excalidraw** | Diagramas de arquitectura |
| **Kanban** | Tablero visual (complemento a Notion) |

### 4. Configurar Obsidian Git
`Settings → Obsidian Git`:
- Auto pull interval: `10` minutos
- Auto push interval: `10` minutos
- Pull on startup: ✅
- Commit message: `docs: vault sync {{date}}`

---

## Estructura del vault

```
Docs/
├── 00 - Inicio/          → Visión del producto y onboarding
├── 01 - Arquitectura/    → Clean Architecture, multi-tenant, stack
├── 02 - Modulos/         → Documentación técnica por módulo
├── 03 - Base de Datos/   → Schemas, modelos, relaciones
├── 04 - API/             → Endpoints documentados
├── 05 - Decisiones ADR/  → Architecture Decision Records
├── 06 - Reuniones/       → Meeting notes por fecha
├── 07 - Equipo/          → Roles, contexto del equipo
└── _templates/           → Templates para nuevas notas
```

---

## Convenciones

- Usar `[[links internos]]` para conectar conceptos entre notas
- Todo ADR lleva número correlativo: `ADR-001`, `ADR-002`...
- Las reuniones llevan fecha: `2026-07-02 Nombre.md`
- Frontmatter en todas las notas (para Dataview)
- Tags: `#arquitectura` `#modulo` `#adr` `#decision` `#pendiente`

---

## Qué va acá vs en Notion

| Obsidian (este vault) | Notion |
|---|---|
| Por qué tomamos cada decisión | Qué hay que hacer y quién lo hace |
| Cómo funciona cada módulo | Estado de avance por fase |
| Diagramas de arquitectura | Kanban del equipo |
| Contexto técnico permanente | Fechas límite y responsables |

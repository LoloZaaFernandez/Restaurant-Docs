---
tags: [equipo, roles]
status: activo
---

# Roles del equipo

## Convenciones de trabajo

- Las tareas se asignan en **Notion** (base de datos "Restaurant SaaS — Tareas del proyecto")
- El contexto técnico vive en **este vault**
- El código vive en los repos de **Backend** y **Frontend**
- Dudas técnicas → abrir una nota en `06 - Reuniones/` con la fecha

## Flujo de trabajo

```
1. Tomar tarea de Notion (cambiar estado a "In progress")
2. Leer la nota del módulo correspondiente en 02 - Modulos/
3. Leer los ADRs relevantes antes de codear
4. Crear rama en git: feat/nombre-de-la-tarea
5. Al terminar → PR + cambiar tarea a "Done" en Notion
```

## Dónde buscar qué

| Pregunta | Dónde buscar |
|---|---|
| ¿Qué tengo que hacer hoy? | Notion → vista "Por Responsable" |
| ¿Cómo funciona este módulo? | `02 - Modulos/` en este vault |
| ¿Por qué hacemos esto así? | `05 - Decisiones ADR/` en este vault |
| ¿Dónde va este código? | `README.md` del repo Backend o Frontend |
| ¿Cómo se estructura la DB? | `03 - Base de Datos/` en este vault |

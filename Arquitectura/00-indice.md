# Fundamentals of Software Architecture

## Resumen de estudio · Richards & Ford

Notas de lectura del libro **"Fundamentals of Software Architecture: An Engineering Approach"** de Mark Richards y Neal Ford (O'Reilly, 2020).

Este resumen prioriza los capítulos de mayor impacto práctico para el rol de arquitecto. Formato pensado para lectura cómoda: párrafos cortos, jerarquía clara, diagramas para conceptos visuales, terminología en negrita para anclaje.

---

## Estructura del repositorio

Cada capítulo vive en un archivo `.md` individual para facilitar lectura, anotación y navegación desde la tablet.

Cada `.md` tiene su mirror en `/offline/` como HTML self-contained, listo para lectura sin conexión.

```
fundamentals-richards-ford/
├── 00-indice.md            ← este archivo
├── 03-modularidad.md
├── 04-...                  (próximos)
└── offline/
    └── 03-modularidad.html
```

---

## Plan de lectura priorizado

### Prioridad alta — impacto inmediato

| Estado | Cap. | Título                              | Foco principal                  |
|--------|------|-------------------------------------|---------------------------------|
| ✅     | 3    | [Modularidad](./03-modularidad.md)  | Cohesion, coupling, connascence |
| ⏳     | 4    | Architecture Characteristics        | El "qué" de la arquitectura     |
| ⏳     | 5    | Identifying Characteristics         | Cómo derivar del negocio        |
| ⏳     | 6    | Measuring and Governing             | Fitness functions               |
| ⏳     | 7    | Scope of Characteristics            | Quanta arquitectónico           |
| ⏳     | 8    | Component-Based Thinking            | Particionado                    |
| ⏳     | 9    | Foundations of Distributed          | Falacias distribuidas           |
| ⏳     | 18   | Choosing the Style                  | Marco de decisión               |
| ⏳     | 19   | Architecture Decisions              | ADRs                            |
| ⏳     | 20   | Analyzing Architecture Risk         | Risk storming                   |
| ⏳     | 21   | Diagramming and Presenting          | Comunicar arquitectura          |

### Prioridad media — estilos arquitectónicos

| Estado | Cap. | Título                       |
|--------|------|------------------------------|
| ⏳     | 10   | Layered Architecture         |
| ⏳     | 13   | Service-Based Architecture   |
| ⏳     | 14   | Event-Driven Architecture    |
| ⏳     | 16   | Orchestration-Driven SOA     |
| ⏳     | 17   | Microservices                |
| ⏳     | 22   | Making Teams Effective       |
| ⏳     | 23   | Negotiation & Leadership     |

### Leyenda de estado

- ✅ Completo y disponible
- 🔄 En revisión
- ⏳ Pendiente
- ⏸️ Postergado

---

## Cómo usar este material

**Lectura activa.** Las notas son un resumen denso, no reemplazan el libro. Si algo no cierra, volvé al capítulo original.

**Anotaciones propias.** Cada `.md` está pensado para que agregues tus notas debajo de cada sección. Usá `> ` para citas que querés destacar y `**[NOTA]**` para apuntes personales.

**Dibujos.** El editor permite dibujar sobre los diagramas. Esa capacidad es lo que diferencia el estudio en tablet del estudio en papel — usala.

**Offline.** Sin conexión, abrí el `.html` correspondiente desde el archivo local en la tablet.

---

## Convenciones de formato

- **Términos clave** en negrita la primera vez que aparecen en una sección.
- `Código` en monospace para nombres técnicos exactos.
- Diagramas como SVG inline para no depender de assets externos.
- Listas con un máximo de 7 ítems por bloque para no saturar memoria de trabajo.
- Párrafos cortos (3-5 líneas máximo).
- Cero cursiva para énfasis (mala para dislexia). Sólo se usa para citas o títulos de libros.

---

## Próximos pasos

Después de Cap. 3 viene **Cap. 4 — Architecture Characteristics Defined** (el vocabulario que vas a usar en cada ADR), seguido por **Cap. 9 — Falacias del cómputo distribuido** (lectura obligatoria antes de cualquier diseño de integración entre sistemas).

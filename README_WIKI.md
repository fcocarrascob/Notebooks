# Wiki de Ingeniería Estructural

Base de conocimiento técnico mantenida por LLM siguiendo el patrón [LLM Wiki](llm_wiki.md).

## Estructura

```
wiki/                        # Páginas mantenidas por LLM
  standards/                 # Normativas y códigos (ACI, API, NCh)
  formulas/                  # Índices de fórmulas por estándar + capítulo
  materials/                 # Propiedades de materiales (concreto, acero)
  concepts/                  # Conceptos técnicos transversales
  applications/              # Workflows de diseño y análisis
  references/                # Hubs de referencias cruzadas
  index.md                   # Catálogo de todas las páginas
  log.md                     # Registro cronológico de operaciones

raw/                         # Fuentes inmutables
  pdf/                       # PDFs de normativas y papers
  references/                # Artículos y notas externas

CLAUDE.md                    # Esquema operativo del agente LLM
```

## Operaciones

### Ingest Manual (agregar nueva fuente)

1. Colocar el archivo fuente en `raw/pdf/` o `raw/references/`
2. Indicar al LLM que procese la fuente: *"Ingesta el archivo raw/pdf/[nombre].pdf"*
3. El LLM:
   - Lee la fuente y discute hallazgos clave
   - Crea/actualiza páginas en `wiki/`
   - Actualiza `wiki/index.md` con nuevas entradas
   - Inserta cross-references bidireccionales
   - Registra la operación en `wiki/log.md`

### Query (consultar la wiki)

Preguntar al LLM sobre cualquier tema cubierto. El LLM busca en `wiki/index.md`, lee las páginas relevantes y sintetiza la respuesta.

**Ejemplos:**
- *"¿Cuál es la fórmula de punzonamiento del ACI 318-25?"*
- *"¿Cómo calculo el período convectivo de un estanque según API 650?"*
- *"¿Qué diferencias hay entre NCh2369 y ASCE 7 para estanques?"*

### Lint (mantenimiento)

Solicitar al LLM una revisión periódica:
- *"Revisa la wiki y reporta problemas"*
- Detecta: contradicciones, claims obsoletos, páginas huérfanas, cross-refs faltantes

## Convenciones de Idioma

- **Español** para contenido: descripciones, explicaciones, procedimientos
- **Inglés** para IDs: nombres de archivos, propiedades YAML, términos técnicos universales
- Ejemplo: `ACI-318-Ch10-Columns.md` → "Diseño de columnas rectangulares según ACI 318-25"

## Relación con Notebooks

Los notebooks de cálculo (`notebooks/`) son herramientas independientes. La wiki es base de conocimiento pura — documenta fórmulas, conceptos y workflows sin vincular notebooks específicos. Ambos se complementan: la wiki provee el conocimiento, los notebooks lo aplican.

## Configuración

El esquema completo del agente está en [`CLAUDE.md`](CLAUDE.md). Incluye:
- Convenciones de nombres y formato
- Reglas de cross-references
- Templates para cada tipo de página
- Workflows detallados de ingest/query/lint

# Wiki Personal de Ingeniería

**Branch:** `feature/engineering-wiki-structure`
**Description:** Base de conocimiento técnico independiente mantenida por LLM usando el patrón LLM Wiki

## Goal

Crear una wiki personal de conocimiento de ingeniería estructural que acumule y organice información de códigos (ACI, API, NCh), fórmulas, conceptos técnicos, materiales y casos de referencia. La wiki actúa como capa persistente entre fuentes raw (PDFs, papers) y consultas del usuario, con referencias cruzadas automáticas entre estándares → fórmulas → conceptos → aplicaciones. Los notebooks de cálculo permanecen como herramientas independientes.

## Implementation Steps

### Step 1: Estructura Base y Esquema del Agente
**Files:** 
- `wiki/.gitkeep`, `raw/pdf/.gitkeep`, `raw/references/.gitkeep`
- `CLAUDE.md` (esquema del agente)
- `wiki/index.md`, `wiki/log.md`

**What:** Crear estructura de directorios (wiki/, raw/) y escribir CLAUDE.md con reglas operativas del LLM: convenciones de nombres (español/inglés), workflow de ingest manual, formato de páginas, política de cross-refs. Incluir templates para tipos de páginas: estándares, fórmulas, conceptos, materiales, aplicaciones.

**Testing:** Verificar que directorios existen y CLAUDE.md contiene secciones: Operations (ingest/query/lint), Page Types (standards/formulas/concepts/materials/applications), Frontmatter Schema, Cross-Reference Rules, Language Conventions

---

### Step 2: Páginas de Estándares Base
**Files:**
- `wiki/standards/ACI-318-25.md`
- `wiki/standards/API-650.md`
- `wiki/standards/NCh2369-2025.md`
- `wiki/index.md` (actualización)
- `wiki/log.md` (append)

**What:** Crear páginas para los tres estándares principales con: título, alcance (scope), estructura de capítulos, sistema de unidades (SI), región de aplicabilidad, áreas de aplicación típicas. Usar [ACI_318-25_SI_indice.md](docs/Hormigon/ACI_318-25_SI_indice.md) como base para ACI.

**Testing:** Cada página tiene frontmatter YAML con: `standard_code`, `edition`, `scope[]`, `unit_system`, `region`, `domains[]`. Index.md lista las 3 páginas bajo categoría "Standards" con descripción breve.

---

### Step 3: Índice de Fórmulas
**Files:**
- `wiki/formulas/ACI-318-Ch10-Columns.md`
- `wiki/formulas/API-650-Seismic.md`
- `wiki/formulas/NCh2369-Spectrum.md`
- `wiki/standards/ACI-318-25.md` (update backlinks)
- `wiki/standards/API-650.md` (update backlinks)
- `wiki/standards/NCh2369-2025.md` (update backlinks)

**What:** Extraer fórmulas usadas en notebooks existentes y organizarlas por estándar + capítulo. Incluir: ecuación LaTeX, número de sección, variables definidas, unidades, ejemplo de uso. Cross-ref bidireccional: formulas/ → standards/, standards/ → formulas/.

**Testing:** Buscar en formulas/ debe encontrar fórmula de momento de columna ($M_u = \phi \cdot ...$) con link a ACI 318 §10.3. El standard page debe listar esta fórmula en su sección § Chapter 10.

---

### Step 4: Base de Conocimiento — Materiales, Geometría y Conceptos
**Files:**
- `wiki/materials/Concrete-Properties.md`
- `wiki/materials/Steel-Reinforcing.md`
- `wiki/concepts/Seismic-Design-Principles.md`
- `wiki/concepts/Load-Combinations.md`
- `wiki/applications/Column-Design-Workflow.md`
- `wiki/applications/Tank-Seismic-Analysis.md`
- `wiki/index.md` (actualización)

**What:** Crear base de conocimiento técnico: 
- **Materials:** Propiedades (f'c, fy, módulos elásticos, factores de reducción)
- **Concepts:** Principios de diseño (combinaciones de carga, factores sísmicos, damping)
- **Applications:** Workflows generales de aplicación (sin vincular notebooks específicos)

Usar [losa_camiones_mineros.md](docs/Hormigon/losa_camiones_mineros.md) como fuente para conceptos de cargas dinámicas.

**Testing:** `Concrete-Properties.md` tiene tabla con grados típicos, módulos elásticos y fórmula ACI de Ec. `Seismic-Design-Principles.md` explica clasificación de sitio, amortiguamiento, factores de respuesta con refs a NCh2369 y ACI.

---

### Step 5: Referencias Cruzadas y Documentación
**Files:**
- `wiki/references/Standards-by-Domain.md`
- `wiki/references/Chile-Specific-Variants.md`
- `README_WIKI.md`
- `wiki/index.md` (finalización)
- `wiki/log.md` (primera entrada)

**What:** 
- Crear hub de referencias cruzadas: mapear dominios → estándares aplicables, documentar variantes chilenas vs internacionales
- Escribir README_WIKI.md: estructura, operaciones (ingest manual/query/lint), convenciones de idioma, ejemplos de uso
- Completar index.md con todas las categorías pobladas
- Primera entrada en log.md

**Testing:** 
- `Standards-by-Domain.md` mapea al menos 3 dominios (hormigón, tanques, sismo) a sus códigos aplicables
- `Chile-Specific-Variants.md` documenta diferencias NCh vs códigos internacionales
- README tiene secciones: Structure, Manual Ingest Workflow, Query Examples, Lint Operations, Language Conventions
- Log tiene entrada: `## [2026-04-09] setup | Initial wiki structure`

---

## Design Decisions (Based on User Input)

1. **Raw Sources:** PDFs de normativas/códigos se almacenan en `raw/pdf/`. Usuario los agregará manualmente según necesidad.

2. **Language Convention:** 
   - **Español** para descripciones, explicaciones, procedimientos
   - **Inglés** para IDs técnicos, nombres de archivos, términos estándar
   - Ejemplo: `ACI-318-Ch10-Columns.md` → contenido "Diseño de columnas rectangulares según..."

3. **Visualization:** Sin Obsidian por ahora. Wiki funciona con cualquier editor markdown (VS Code, etc.). Graph view no es prioridad.

4. **Ingest Strategy:** Manual. Usuario decide cuándo procesar nuevos PDFs. CLAUDE.md incluye workflow paso-a-paso para ingest guiado.

5. **Taxonomy:** Estructura extensible para agregar dominios futuros (acero, geotecnia, etc.) cuando se requiera. Index.md usa categorías flexibles.

6. **Notebooks Separation:** Notebooks permanecen independientes como herramientas de cálculo. La wiki es base de conocimiento pura sin vínculos directos a notebooks.

# Engineering Wiki — Implementation Plan

## Goal
Crear una wiki personal de ingeniería estructural siguiendo el patrón LLM Wiki, con páginas de estándares (ACI, API, NCh), fórmulas, conceptos, materiales y aplicaciones, incluyendo referencias cruzadas bidireccionales.

## Prerequisites
Make sure that the user is currently on the `feature/engineering-wiki-structure` branch before beginning implementation.
If not, move them to the correct branch. If the branch does not exist, create it from main.

```powershell
git branch --show-current
# Expected: feature/engineering-wiki-structure
# If not: git checkout -b feature/engineering-wiki-structure
```

---

### Step-by-Step Instructions

---

#### Step 1: Estructura Base y Esquema del Agente

- [x] Crear directorios base:

```powershell
New-Item -ItemType Directory -Force -Path wiki/standards, wiki/formulas, wiki/materials, wiki/concepts, wiki/applications, wiki/references, raw/pdf, raw/references
```

- [x] Crear archivo `.gitkeep` en cada directorio vacío de `raw/`:

```powershell
New-Item -ItemType File -Force -Path raw/pdf/.gitkeep
New-Item -ItemType File -Force -Path raw/references/.gitkeep
```

- [x] Copy and paste code below into `CLAUDE.md`:

```markdown
# CLAUDE.md — Engineering Wiki Schema

> Esquema operativo para el mantenimiento de la wiki de ingeniería estructural.
> Este archivo define convenciones, workflows y reglas para el agente LLM.

---

## 1. Estructura de Directorios

```
wiki/                    # Páginas mantenidas por LLM
  standards/             # Páginas de estándares/normativas
  formulas/              # Índices de fórmulas por estándar + capítulo
  materials/             # Propiedades de materiales
  concepts/              # Conceptos técnicos transversales
  applications/          # Workflows de aplicación (sin vincular notebooks)
  references/            # Hubs de referencias cruzadas
  index.md               # Catálogo de todas las páginas
  log.md                 # Registro cronológico de operaciones

raw/                     # Fuentes inmutables (el LLM NO modifica)
  pdf/                   # PDFs de normativas y papers
  references/            # Artículos, notas externas
```

## 2. Convenciones de Idioma

- **Español** para: descripciones, explicaciones, procedimientos, contenido de páginas
- **Inglés** para: nombres de archivos, IDs técnicos (e.g. `ACI-318-25`), propiedades YAML, términos estándar universales
- **Ejemplo:** Archivo `ACI-318-Ch10-Columns.md` → contenido: "Diseño de columnas rectangulares según ACI 318-25 Capítulo 10"

## 3. Formato de Páginas

### 3.1 Frontmatter YAML

Todas las páginas deben iniciar con frontmatter YAML:

```yaml
---
title: "Título descriptivo en español"
type: standard | formula | material | concept | application | reference
tags: [tag1, tag2]
related: [ruta/a/pagina.md]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

### 3.2 Campos por Tipo de Página

**Standards:**
```yaml
standard_code: "ACI-318-25"
edition: "2025"
scope: ["hormigón estructural", "edificaciones"]
unit_system: "SI"
region: "Internacional"
domains: ["hormigón", "sismo"]
```

**Formulas:**
```yaml
standard_ref: "ACI-318-25"
chapter: "10"
section: "10.3"
variables: [M_u, phi, A_s, f_y]
units: "SI"
```

**Materials:**
```yaml
material_type: "concrete | steel | composite"
properties: [f_c, E_c, gamma]
```

**Concepts:**
```yaml
domain: "sismo | hormigón | estanques"
level: "fundamental | intermediate | advanced"
```

**Applications:**
```yaml
domain: "hormigón | estanques | sismo"
standards_used: ["ACI-318-25", "NCh2369-2025"]
```

## 4. Operaciones

### 4.1 Ingest (Manual)

Workflow paso a paso cuando el usuario agrega una nueva fuente:

1. Usuario coloca el archivo en `raw/pdf/` o `raw/references/`
2. LLM lee la fuente completa
3. LLM discute hallazgos clave con el usuario
4. LLM ejecuta actualizaciones:
   a. Crear página de resumen si la fuente es nueva
   b. Actualizar páginas existentes afectadas (fórmulas, conceptos, materiales)
   c. Actualizar `wiki/index.md` con nuevas entradas
   d. Insertar cross-references bidireccionales
   e. Append entrada en `wiki/log.md`
5. Usuario revisa cambios

### 4.2 Query

1. LLM lee `wiki/index.md` para localizar páginas relevantes
2. LLM lee las páginas identificadas
3. LLM sintetiza respuesta con citas a secciones específicas
4. Si la respuesta genera conocimiento nuevo → crear página y actualizar index

### 4.3 Lint

Verificaciones periódicas de salud de la wiki:

- Contradicciones entre páginas
- Claims obsoletos por fuentes más recientes
- Páginas huérfanas (sin links entrantes)
- Conceptos mencionados sin página propia
- Cross-references faltantes
- Frontmatter incompleto o inconsistente

## 5. Cross-References

### Reglas

- Toda fórmula debe linkar a su estándar origen: `[ACI 318-25 §10.3](../standards/ACI-318-25.md)`
- Todo estándar debe listar sus fórmulas en una sección "Fórmulas Relacionadas"
- Los materiales referencian estándares que definen sus propiedades
- Los conceptos referencian estándares y fórmulas que los implementan
- Las aplicaciones referencian estándares, fórmulas, materiales y conceptos usados

### Formato de Links

Usar rutas relativas desde la página actual:
- Desde `formulas/` a `standards/`: `../standards/ACI-318-25.md`
- Desde `standards/` a `formulas/`: `../formulas/ACI-318-Ch10-Columns.md`

## 6. Templates

### Template: Standard Page

```markdown
---
title: "{Nombre del Estándar}"
type: standard
standard_code: "{CODIGO}"
edition: "{AÑO}"
scope: []
unit_system: "SI"
region: ""
domains: []
tags: []
related: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# {Nombre del Estándar}

## Alcance
{Descripción del alcance del estándar}

## Estructura de Capítulos
{Lista de partes y capítulos principales}

## Áreas de Aplicación
{Dominios donde se aplica}

## Fórmulas Relacionadas
{Links a páginas en formulas/}

## Notas
{Observaciones, variantes regionales, cambios respecto ediciones anteriores}
```

### Template: Formula Page

```markdown
---
title: "{Estándar} — {Capítulo/Tema}"
type: formula
standard_ref: "{CODIGO}"
chapter: "{N}"
section: "{N.N}"
variables: []
units: "SI"
tags: []
related: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# {Tema de fórmulas}

## Fuente
{Link al estándar}: Capítulo {N}, Sección {N.N}

## Fórmulas

### {Nombre de la fórmula}
$$formula$$

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|

**Condiciones de aplicación:**
{Restricciones, rangos válidos}

## Ejemplo de Uso
{Ejemplo numérico breve}
```

### Template: Concept Page

```markdown
---
title: "{Nombre del Concepto}"
type: concept
domain: ""
level: ""
tags: []
related: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# {Nombre del Concepto}

## Definición
{Explicación clara y concisa}

## Principios Fundamentales
{Desarrollo del concepto}

## Estándares Relacionados
{Links a standards/}

## Fórmulas Relacionadas
{Links a formulas/}

## Referencias
{Fuentes consultadas}
```

### Template: Material Page

```markdown
---
title: "{Nombre del Material}"
type: material
material_type: ""
properties: []
tags: []
related: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# {Nombre del Material}

## Propiedades de Diseño
| Propiedad | Símbolo | Valor típico | Unidad | Referencia |
|-----------|---------|-------------|--------|-----------|

## Grados Disponibles
{Tabla de grados/calidades}

## Normativa Aplicable
{Links a standards/}

## Notas de Diseño
{Consideraciones prácticas}
```

### Template: Application Page

```markdown
---
title: "{Nombre del Workflow}"
type: application
domain: ""
standards_used: []
tags: []
related: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# {Nombre del Workflow}

## Objetivo
{Qué se diseña/analiza}

## Estándares Aplicables
{Links a standards/}

## Datos de Entrada
{Lista de parámetros necesarios}

## Procedimiento
1. {Paso 1}
2. {Paso 2}
...

## Verificaciones
{Lista de chequeos requeridos}

## Fórmulas Utilizadas
{Links a formulas/}

## Referencias
{Links a conceptos y materiales relevantes}
```
```

- [x] Copy and paste code below into `wiki/index.md`:

```markdown
---
title: "Índice de la Wiki de Ingeniería"
type: index
created: 2026-04-09
updated: 2026-04-09
---

# Índice — Wiki de Ingeniería Estructural

Catálogo de todas las páginas de la wiki, organizado por categoría.

---

## Standards
| Página | Descripción |
|--------|------------|
| *(vacío — se poblará en Step 2)* | |

## Formulas
| Página | Descripción |
|--------|------------|
| *(vacío — se poblará en Step 3)* | |

## Materials
| Página | Descripción |
|--------|------------|
| *(vacío — se poblará en Step 4)* | |

## Concepts
| Página | Descripción |
|--------|------------|
| *(vacío — se poblará en Step 4)* | |

## Applications
| Página | Descripción |
|--------|------------|
| *(vacío — se poblará en Step 4)* | |

## References
| Página | Descripción |
|--------|------------|
| *(vacío — se poblará en Step 5)* | |
```

- [x] Copy and paste code below into `wiki/log.md`:

```markdown
---
title: "Log de Operaciones"
type: log
created: 2026-04-09
---

# Log de Operaciones — Wiki de Ingeniería

Registro cronológico append-only de operaciones sobre la wiki.

---

<!-- Las entradas se agregan al final, formato: ## [YYYY-MM-DD] tipo | Descripción -->
```

##### Step 1 Verification Checklist
- [x] Existen los directorios: `wiki/standards/`, `wiki/formulas/`, `wiki/materials/`, `wiki/concepts/`, `wiki/applications/`, `wiki/references/`
- [x] Existen los directorios: `raw/pdf/`, `raw/references/`
- [x] `CLAUDE.md` existe en la raíz del proyecto y contiene secciones: Estructura de Directorios, Convenciones de Idioma, Formato de Páginas, Operaciones (Ingest/Query/Lint), Cross-References, Templates (5 tipos)
- [x] `wiki/index.md` existe con frontmatter YAML válido y categorías vacías
- [x] `wiki/log.md` existe con frontmatter YAML válido
- [x] No hay errores de sintaxis en ningún archivo markdown

#### Step 1 STOP & COMMIT
**STOP & COMMIT:** Agent must stop here and wait for the user to test, stage, and commit the change.

---

#### Step 2: Páginas de Estándares Base

- [x] Copy and paste code below into `wiki/standards/ACI-318-25.md`:

```markdown
---
title: "ACI CODE-318-25 — Código de Construcción para Concreto Estructural"
type: standard
standard_code: "ACI-318-25"
edition: "2025"
scope: ["hormigón estructural", "edificaciones", "fundaciones", "muros", "losas"]
unit_system: "SI"
region: "Internacional (uso global, referencia principal en América)"
domains: ["hormigón", "sismo", "fundaciones"]
tags: [ACI, concreto, hormigón, diseño-estructural]
related:
  - ../formulas/ACI-318-Ch10-Columns.md
  - ../materials/Concrete-Properties.md
  - ../materials/Steel-Reinforcing.md
  - ../concepts/Load-Combinations.md
  - ../concepts/Seismic-Design-Principles.md
  - ../applications/Column-Design-Workflow.md
  - ../references/Standards-by-Domain.md
created: 2026-04-09
updated: 2026-04-09
---

# ACI CODE-318-25 — Código de Construcción para Concreto Estructural (SI)

> **Versión:** ACI CODE-318-25, Edición 2025 — Sistema Internacional de Unidades (SI)
> **Total de páginas:** 702
> **Fuente:** `raw/pdf/ACI 318-25_SI.pdf`

## Alcance

Código de diseño y construcción para concreto estructural en edificaciones y estructuras relacionadas. Cubre desde requisitos generales del sistema estructural hasta evaluación de estructuras existentes. Incluye concreto reforzado, preesforzado, simple y prefabricado. Aplica a todos los elementos de concreto en la estructura, incluyendo losas, vigas, columnas, muros, fundaciones, diafragmas y conexiones.

## Estructura de Capítulos

### Parte 1 — General (Cap. 1–4)
- **Cap. 1** — General: alcance, aplicabilidad, autoridad de construcción
- **Cap. 2** — Notación y Terminología
- **Cap. 3** — Normas de Referencia
- **Cap. 4** — Requisitos del Sistema Estructural: cargas, análisis, resistencia, serviciabilidad, durabilidad, integridad, resistencia al fuego

### Parte 2 — Cargas y Análisis (Cap. 5–6)
- **Cap. 5** — Cargas: factores y combinaciones de carga (§5.3)
- **Cap. 6** — Análisis Estructural: métodos de primer y segundo orden, análisis inelástico, FEM

### Parte 3 — Elementos (Cap. 7–13)
- **Cap. 7** — Losas Unidireccionales
- **Cap. 8** — Losas Bidireccionales: incluye punzonamiento (§8.4–8.5)
- **Cap. 9** — Vigas: flexión, corte, torsión, vigas profundas
- **Cap. 10** — Columnas: diseño a flexo-compresión
- **Cap. 11** — Muros
- **Cap. 12** — Diafragmas
- **Cap. 13** — Fundaciones: superficiales y profundas

### Parte 4 — Uniones / Conexiones / Anclajes (Cap. 14–17)
- **Cap. 14** — Concreto Simple
- **Cap. 15** — Uniones Vaciadas en Sitio
- **Cap. 16** — Conexiones entre Elementos: prefabricados, ménsulas
- **Cap. 17** — Anclaje al Concreto: tensión, corte, interacción, requisitos sísmicos, shear lugs

### Parte 5 — Resistencia Sísmica (Cap. 18)
- **Cap. 18** — Estructuras Resistentes a Sismos: marcos ordinarios/intermedios/especiales, muros especiales, diafragmas, fundaciones

### Parte 6 — Materiales y Durabilidad (Cap. 19–20)
- **Cap. 19** — Concreto: propiedades de diseño, durabilidad, clases de exposición
- **Cap. 20** — Acero de Refuerzo: barras, torones, conectores, embebidos

### Parte 7 — Resistencia y Serviciabilidad (Cap. 21–24)
- **Cap. 21** — Factores de Reducción φ
- **Cap. 22** — Resistencia Seccional: flexión, axial combinado, corte, torsión, fricción de corte
- **Cap. 23** — Método Biela-Tirante (Strut-and-Tie)
- **Cap. 24** — Serviciabilidad: deflexiones, control de fisuración, retracción

### Parte 8 — Refuerzo (Cap. 25)
- **Cap. 25** — Detalles: espaciado, ganchos, empalmes, refuerzo transversal, postensado

### Parte 9 — Construcción (Cap. 26)
- **Cap. 26** — Documentos, inspección, materiales, cimbras, aceptación

### Parte 10 — Evaluación (Cap. 27)
- **Cap. 27** — Evaluación de Resistencia de Estructuras Existentes

### Apéndices
- **A** — Análisis No Lineal de Historia de Respuesta (NRHA)
- **B** — Diseño Eólico Basado en Desempeño
- **C** — Sustentabilidad y Resiliencia
- **D** — Información de Acero de Refuerzo
- **E** — Equivalencias de Unidades SI / MKS / US Customary

## Guía Rápida por Tema

| Tema | Capítulo(s) | Página(s) |
|------|------------|-----------|
| Factores de carga y combinaciones | Cap. 5 | 69 |
| Análisis de segundo orden / P-delta | Cap. 6 §6.7 | 95 |
| Losas planas (flat plates / flat slabs) | Cap. 8 | 111 |
| Punzonamiento | Cap. 8 §8.4–8.5 | 114–120 |
| Diseño de vigas (flexión, corte, torsión) | Cap. 9, 22 | 141, 437 |
| Diseño de columnas | Cap. 10 | 169 |
| Muros de concreto | Cap. 11 | 179 |
| Fundaciones superficiales y profundas | Cap. 13 | 205 |
| Anclaje al concreto (pernos, conectores) | Cap. 17 | 253 |
| Diseño sísmico (marcos y muros especiales) | Cap. 18 | 307 |
| Propiedades del concreto (f'c, Ec) | Cap. 19 | 395 |
| Propiedades del acero (fy, fu) | Cap. 20 | 409 |
| Factores φ de resistencia | Cap. 21 | 429 |
| Resistencia a corte (Vc, Vs) | Cap. 22 §22.5 | 441 |
| Torsión | Cap. 22 §22.7 | 459 |
| Método biela-tirante (STM) | Cap. 23 | 477 |
| Deflexiones y control de fisuración | Cap. 24 | 497 |
| Longitudes de desarrollo y empalmes | Cap. 25 | 509 |
| Concreto preesforzado | Cap. 20 §20.3, 24 §24.5, 25 §25.8 | 415, 506, 556 |

## Áreas de Aplicación

- Diseño de edificios de concreto reforzado y preesforzado
- Fundaciones superficiales y profundas
- Estructuras resistentes a sismos (marcos y muros especiales)
- Estanques y estructuras de contención (en conjunto con ACI 350)
- Estructuras industriales (pisos, rampas, talleres)
- Evaluación y refuerzo de estructuras existentes

## Fórmulas Relacionadas

- [ACI 318 Cap. 10 — Columnas](../formulas/ACI-318-Ch10-Columns.md): diseño a flexo-compresión, refuerzo mínimo/máximo

## Notas

- La edición 2025 incorpora el **factor de efecto de tamaño** $\lambda_s = \sqrt{2/(1+d/250)} \leq 1.0$ para corte y punzonamiento (introducido en ACI 318-19, mantenido en ACI 318-25)
- Las unidades SI son nativas en esta edición; no son conversiones del sistema US Customary
- En Chile, ACI 318 se usa como referencia principal para hormigón armado, complementado con normativas locales (NCh)
```

- [x] Copy and paste code below into `wiki/standards/API-650.md`:

```markdown
---
title: "API 650 — Welded Tanks for Oil Storage"
type: standard
standard_code: "API-650"
edition: "13th Edition (2020) + Addenda"
scope: ["estanques de almacenamiento", "acero soldado", "petróleo y derivados"]
unit_system: "SI"
region: "Internacional (origen USA, uso global)"
domains: ["estanques", "sismo"]
tags: [API, estanques, tanques, acero, almacenamiento, sismo]
related:
  - ../formulas/API-650-Seismic.md
  - ../concepts/Seismic-Design-Principles.md
  - ../applications/Tank-Seismic-Analysis.md
  - ../references/Standards-by-Domain.md
created: 2026-04-09
updated: 2026-04-09
---

# API 650 — Welded Tanks for Oil Storage

> **Versión:** 13th Edition, 2020 (con Addenda)
> **Aplicación:** Diseño, fabricación, montaje y prueba de estanques cilíndricos verticales de acero soldado para almacenamiento de productos petroleros y químicos
> **Fuente:** `raw/pdf/` (agregar cuando esté disponible)

## Alcance

Estándar para estanques de almacenamiento atmosféricos (presión interna ≤ 2.5 kPa / 1 psi) de acero al carbono soldado, con fondo plano apoyado uniformemente sobre suelo o fundación. Cubre estanques cilíndricos verticales abiertos o con techo fijo/flotante. No aplica a estanques criogénicos (ver API 620) ni a recipientes a presión (ver ASME BPVC).

## Estructura Principal

| Sección | Contenido |
|---------|-----------|
| §1 | Alcance |
| §2 | Referencias |
| §3 | Definiciones |
| §4 | Materiales |
| §5 | Diseño |
| §5.6 | Diseño del cuerpo (shell design) — métodos 1-Foot y Variable-Design-Point |
| §5.10 | Diseño del piso (bottom plates) |
| §5.11 | Diseño de techo (fijo / flotante) |
| §5.12 | Aberturas y conexiones (nozzles) |
| §6 | Fabricación |
| §7 | Montaje |
| §8 | Inspección y pruebas |
| §E (Annex E) | **Diseño Sísmico** — modelo masas impulsiva/convectiva, períodos, fuerzas de diseño |

## Análisis Sísmico — Annex E

El Annex E define un modelo de masas equivalentes para evaluar la respuesta sísmica de estanques líquidos:

- **Masa impulsiva** ($m_i$): porción del líquido que se mueve rígidamente con el cuerpo del estanque
- **Masa convectiva** ($m_c$): porción del líquido que oscila (sloshing) en la superficie libre
- **Períodos** $T_i$ (impulsivo) y $T_c$ (convectivo): dependen de geometría y rigidez
- **Espectro de diseño**: puede usar el espectro propio de API 650 o el de la normativa local (e.g. NCh2369)

> En Chile se utiliza el espectro de NCh2369-2025 con los parámetros de amortiguamiento especificados por API 650 Annex E (ξ_i = 2%, ξ_c = 0.5%).

## Áreas de Aplicación

- Estanques de almacenamiento de petróleo crudo y derivados
- Estanques de agua industrial (con modificaciones)
- Estanques de almacenamiento químico (líquidos no criogénicos)
- Análisis sísmico de estanques cilíndricos apoyados sobre suelo

## Fórmulas Relacionadas

- [API 650 — Sismo (Annex E)](../formulas/API-650-Seismic.md): masas equivalentes, alturas, períodos impulsivo y convectivo

## Notas

- API 650 define su propio espectro sísmico (basado en ASCE 7), pero en Chile se reemplaza por NCh2369
- Los parámetros de amortiguamiento para estanques son significativamente distintos a los de edificaciones: $\xi_i = 2\%$ (impulsivo) vs $\xi_c = 0.5\%$ (convectivo)
- El factor de reducción para la componente impulsiva es $R_i = 4$ y para convectiva $R_c = 1.5$ (NCh2369 §11.2.1.6-7)
- El método de Variable-Design-Point (§5.6.4) es más económico que el método 1-Foot para estanques de diámetro grande
```

- [x] Copy and paste code below into `wiki/standards/NCh2369-2025.md`:

```markdown
---
title: "NCh2369-2025 — Diseño Sísmico de Estructuras e Instalaciones Industriales"
type: standard
standard_code: "NCh2369-2025"
edition: "2025"
scope: ["diseño sísmico", "estructuras industriales", "instalaciones", "estanques"]
unit_system: "SI"
region: "Chile"
domains: ["sismo", "estanques", "estructuras industriales"]
tags: [NCh, sismo, chile, espectro, industrial]
related:
  - ../formulas/NCh2369-Spectrum.md
  - ../concepts/Seismic-Design-Principles.md
  - ../applications/Tank-Seismic-Analysis.md
  - ../references/Standards-by-Domain.md
  - ../references/Chile-Specific-Variants.md
created: 2026-04-09
updated: 2026-04-09
---

# NCh2369-2025 — Diseño Sísmico de Estructuras e Instalaciones Industriales

> **Versión:** NCh2369.Of2025 (reemplaza NCh2369.Of2003)
> **Aplicación:** Diseño sísmico de estructuras e instalaciones industriales en Chile
> **Organismo:** Instituto Nacional de Normalización (INN), Chile

## Alcance

Norma chilena para el diseño sísmico de estructuras e instalaciones industriales, incluyendo estanques, silos, equipos, tuberías, estructuras de soporte, y edificios industriales. Complementa la NCh433 (diseño sísmico de edificios) para el ámbito industrial. Define zonificación sísmica, clasificación de suelos, espectros de diseño, factores de modificación de respuesta, y requisitos especiales para componentes industriales.

## Contenido Principal

| Capítulo | Contenido |
|----------|-----------|
| §1–3 | Alcance, referencias normativas, definiciones |
| §4 | Requisitos generales de diseño |
| §5 | **Zonificación sísmica** — Zonas 1, 2, 3 con aceleraciones $A_r$ |
| §6 | **Clasificación de suelos** — Tipos A (roca), B, C, D, E (suelo blando) |
| §7 | **Espectro de diseño** — forma espectral, factores de amplificación |
| §8 | Combinaciones de carga |
| §9 | Análisis estático y dinámico |
| §10 | Estructuras de acero y hormigón |
| §11 | **Estanques y silos** — masas impulsiva/convectiva, amortiguamiento, factores R |
| §12 | Equipos y componentes |
| §13 | Tuberías |

## Parámetros del Espectro de Diseño

### Zonas Sísmicas

| Zona | $A_r$ (aceleración efectiva) |
|------|-----|
| 1 | 0.20g |
| 2 | 0.30g |
| 3 | 0.40g |

### Parámetros de Suelo

| Suelo | S | r | $T_0$ [s] | p | q |
|-------|---|---|-----------|---|---|
| A (roca) | 0.90 | 3.0 | 0.15 | 1.00 | 2.50 |
| B | 1.00 | 3.0 | 0.30 | 1.00 | 2.50 |
| C | 1.05 | 3.5 | 0.40 | 1.50 | 2.80 |
| D | 1.20 | 4.0 | 0.75 | 1.70 | 3.00 |
| E | 1.30 | 4.5 | 1.20 | 1.85 | 3.00 |

### Forma Espectral Bruta

$$S_a^{bruta} = A_r \cdot S \cdot \frac{1 + r\left(\frac{T'}{T_0}\right)^p}{1 + \left(\frac{T'}{T_0}\right)^q}$$

donde $T' = T$ para dirección horizontal y $T' = T/1.7$ para dirección vertical.

### Factor de Amortiguamiento

$$\eta = \left(\frac{0.05}{\xi}\right)^{0.4}$$

### Espectro de Diseño

$$S_a(T) = \frac{I \cdot \eta \cdot S_a^{bruta}}{R^*}$$

### Factor R* (Reducción por período corto)

Para $T \leq 0.16 R T_1$:

$$R^* = 1.5 + (R - 1.5) \cdot \frac{T}{0.16 R T_1}$$

(interpolación lineal de 1.5 a R)

Para $T > 0.16 R T_1$: $R^* = R$

## Parámetros Especiales para Estanques (§11)

| Parámetro | Componente Impulsiva | Componente Convectiva |
|-----------|---------------------|----------------------|
| Factor R | $R_i = 4$ | $R_c = 1.5$ |
| Amortiguamiento ξ | 2% | 0.5% |
| Referencia | §11.2.1.6 | §11.2.1.7 |

## Áreas de Aplicación

- Diseño sísmico de plantas industriales (minería, petroquímica, energía)
- Estanques de almacenamiento sobre suelo (en conjunto con API 650)
- Silos de almacenamiento de sólidos
- Equipos mecánicos y recipientes a presión soportados
- Sistemas de tuberías industriales
- Estructuras de soporte de equipos (pipe racks, plataformas)

## Fórmulas Relacionadas

- [NCh2369 — Espectro Sísmico](../formulas/NCh2369-Spectrum.md): espectro de diseño, factores de suelo, amortiguamiento, R*

## Notas

- NCh2369 es específica para instalaciones industriales; para edificios habitacionales/comerciales se usa NCh433
- Los factores de amortiguamiento para estanques ($\xi = 2\%$ impulsivo, $\xi = 0.5\%$ convectivo) difieren significativamente del estándar de edificaciones ($\xi = 5\%$)
- La componente vertical del espectro usa $T' = T/1.7$ (período equivalente desplazado)
- En la práctica chilena, se combina NCh2369 (espectro) con API 650 (modelo de masas) para el diseño sísmico de estanques
- La edición 2025 actualiza la zonificación sísmica respecto a la edición 2003
```

- [x] Actualizar `wiki/index.md` — reemplazar la sección Standards con:

```markdown
## Standards
| Página | Descripción |
|--------|------------|
| [ACI-318-25](standards/ACI-318-25.md) | Código de construcción para concreto estructural — edición 2025 SI |
| [API-650](standards/API-650.md) | Estanques cilíndricos de acero soldado para almacenamiento |
| [NCh2369-2025](standards/NCh2369-2025.md) | Diseño sísmico de estructuras e instalaciones industriales (Chile) |
```

##### Step 2 Verification Checklist
- [x] `wiki/standards/ACI-318-25.md` tiene frontmatter con campos: `standard_code`, `edition`, `scope[]`, `unit_system`, `region`, `domains[]`
- [x] `wiki/standards/API-650.md` tiene frontmatter con los mismos campos
- [x] `wiki/standards/NCh2369-2025.md` tiene frontmatter con los mismos campos
- [x] Las 3 páginas contienen secciones: Alcance, Estructura de Capítulos, Áreas de Aplicación, Fórmulas Relacionadas, Notas
- [x] `wiki/index.md` lista las 3 páginas bajo "Standards" con links válidos
- [x] Los links relativos en `related:` del frontmatter son rutas válidas (se verificarán cuando existan los archivos destino)

#### Step 2 STOP & COMMIT
**STOP & COMMIT:** Agent must stop here and wait for the user to test, stage, and commit the change.

---

#### Step 3: Índice de Fórmulas

- [x] Copy and paste code below into `wiki/formulas/ACI-318-Ch10-Columns.md`:

```markdown
---
title: "ACI 318-25 Cap. 10 — Diseño de Columnas"
type: formula
standard_ref: "ACI-318-25"
chapter: "10"
section: "10.3–10.7, 22.4"
variables: [P_u, M_u, phi, A_s, A_g, f_c, f_y, rho_g, a, d]
units: "SI"
tags: [columnas, flexo-compresión, hormigón, ACI]
related:
  - ../standards/ACI-318-25.md
  - ../materials/Concrete-Properties.md
  - ../materials/Steel-Reinforcing.md
  - ../applications/Column-Design-Workflow.md
created: 2026-04-09
updated: 2026-04-09
---

# ACI 318-25 Cap. 10 — Diseño de Columnas

## Fuente
[ACI CODE-318-25](../standards/ACI-318-25.md): Capítulo 10 (Columnas), Capítulo 22 §22.4 (Resistencia axial combinada)

---

## Fórmulas

### Resistencia Axial Nominal Máxima

Para columnas con estribos (§22.4.2):

$$P_{n,max} = 0.80 \left[ 0.85 f'_c (A_g - A_{st}) + f_y A_{st} \right]$$

Para columnas con espiral (§22.4.2):

$$P_{n,max} = 0.85 \left[ 0.85 f'_c (A_g - A_{st}) + f_y A_{st} \right]$$

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $P_{n,max}$ | Resistencia axial nominal máxima | N |
| $f'_c$ | Resistencia a compresión del concreto | MPa |
| $A_g$ | Área bruta de la sección transversal | mm² |
| $A_{st}$ | Área total de acero longitudinal | mm² |
| $f_y$ | Fluencia del acero de refuerzo | MPa |

### Resistencia a Flexión Nominal

$$M_n = A_s f_y \left( d - \frac{a}{2} \right)$$

$$a = \frac{A_s f_y}{0.85 f'_c b}$$

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $M_n$ | Momento nominal | N·mm |
| $A_s$ | Área de acero en tracción | mm² |
| $d$ | Altura efectiva (distancia al centroide del acero en tracción) | mm |
| $a$ | Profundidad del bloque de compresión equivalente | mm |
| $b$ | Ancho de la sección | mm |

### Verificación de Diseño

$$\phi P_n \geq P_u \quad \text{y} \quad \phi M_n \geq M_u$$

**Factores φ (§21.2):**
| Condición | $\phi$ |
|-----------|--------|
| Compresión controlada (estribos) | 0.65 |
| Compresión controlada (espiral) | 0.75 |
| Tracción controlada | 0.90 |
| Transición | Interpolar linealmente |

### Límites de Refuerzo (§10.6)

$$0.01 A_g \leq A_{st} \leq 0.08 A_g$$

Es decir:
$$1\% \leq \rho_g \leq 8\%$$

> **Práctica recomendada:** mantener $\rho_g$ entre 1% y 4% para facilitar la construcción y evitar congestión de acero.

### Refuerzo Mínimo por Flexión (§10.3.1)

$$A_{s,min} = \frac{0.25 \sqrt{f'_c}}{f_y} b_w d \geq \frac{1.4}{f_y} b_w d$$

### Esbeltez y Efectos de Segundo Orden (§6.6.4, §6.7)

Para columnas esbeltas ($k\ell_u / r > 22$ para arriostradas, $k\ell_u / r > 22$ para no arriostradas):

**Método de amplificación de momentos (§6.6.4):**

$$M_c = \delta_{ns} M_2$$

$$\delta_{ns} = \frac{C_m}{1 - P_u / (0.75 P_c)} \geq 1.0$$

$$P_c = \frac{\pi^2 (EI)_{eff}}{(k \ell_u)^2}$$

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $k$ | Factor de longitud efectiva | — |
| $\ell_u$ | Longitud no arriostrada | mm |
| $r$ | Radio de giro ($\approx 0.3h$ para rectangular) | mm |
| $C_m$ | Factor de equivalencia de momentos | — |
| $P_c$ | Carga crítica de Euler | N |
| $(EI)_{eff}$ | Rigidez efectiva reducida | N·mm² |

---

## Ejemplo de Uso

Sección rectangular $b = 300$ mm, $h = 500$ mm, $f'_c = 28$ MPa, $f_y = 420$ MPa:

$$A_g = 300 \times 500 = 150{,}000 \text{ mm}^2$$

$$A_{st,min} = 0.01 \times 150{,}000 = 1{,}500 \text{ mm}^2$$

$$A_{st,max} = 0.08 \times 150{,}000 = 12{,}000 \text{ mm}^2$$

Usando $\rho_g = 2\%$: $A_{st} = 3{,}000$ mm² → 6 barras Ø25 ($A_s = 6 \times 491 = 2{,}946$ mm²)

$$P_{n,max} = 0.80 [0.85 \times 28 \times (150{,}000 - 2{,}946) + 420 \times 2{,}946] = 3{,}790 \text{ kN}$$
```

- [x] Copy and paste code below into `wiki/formulas/API-650-Seismic.md`:

```markdown
---
title: "API 650 Annex E — Análisis Sísmico de Estanques"
type: formula
standard_ref: "API-650"
chapter: "Annex E"
section: "E.4–E.6"
variables: [m_i, m_c, h_i, h_c, T_i, T_c, D, h_liq, e_pared]
units: "SI"
tags: [estanques, sismo, API, masas-equivalentes, período]
related:
  - ../standards/API-650.md
  - ../standards/NCh2369-2025.md
  - ../concepts/Seismic-Design-Principles.md
  - ../applications/Tank-Seismic-Analysis.md
created: 2026-04-09
updated: 2026-04-09
---

# API 650 Annex E — Análisis Sísmico de Estanques Cilíndricos

## Fuente
[API 650](../standards/API-650.md): Annex E — Seismic Design of Storage Tanks

---

## Modelo de Masas Equivalentes

El modelo divide la masa total del líquido en dos componentes que responden de forma diferente al sismo:

### Masa Impulsiva

Porción del líquido que se mueve rígidamente con el cuerpo del estanque:

$$m_i = M_{liq} \cdot \frac{\tanh(0.866\, D / h_{liq})}{0.866\, D / h_{liq}}$$

### Masa Convectiva

Porción del líquido que oscila libremente en la superficie (sloshing):

$$m_c = M_{liq} \cdot 0.23 \cdot \frac{\tanh(3.68\, h_{liq} / D)}{h_{liq} / D}$$

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $M_{liq}$ | Masa total del líquido | kg |
| $D$ | Diámetro del estanque | m |
| $h_{liq}$ | Altura del líquido | m |
| $m_i$ | Masa impulsiva | kg |
| $m_c$ | Masa convectiva | kg |

> Verificación: $m_i + m_c \approx M_{liq}$ (la suma se aproxima al total)

---

## Alturas de Aplicación de las Masas

### Altura impulsiva (sin presión sobre fondo)

$$h_i = h_{liq} \begin{cases} 0.375 & \text{si } h_{liq}/D \leq 0.75 \\ 0.5 - 0.09375 / (h_{liq}/D) & \text{si } h_{liq}/D > 0.75 \end{cases}$$

### Altura convectiva (sin presión sobre fondo)

$$h_c = h_{liq} \left(1 - \frac{\cosh(3.68\, h_{liq}/D) - 1}{3.68\,(h_{liq}/D)\,\sinh(3.68\, h_{liq}/D)}\right)$$

### Altura impulsiva con presión sobre fondo ($h'_i$)

$$h'_i = h_{liq} \begin{cases} \dfrac{0.866\, D/h_{liq}}{2\,\tanh(0.866\, D/h_{liq})} - 0.125 & \text{si } h_{liq}/D \leq 1.33 \\ 0.45 & \text{si } h_{liq}/D > 1.33 \end{cases}$$

### Altura convectiva con presión sobre fondo ($h'_c$)

$$h'_c = h_{liq} \left(1 - \frac{\cosh(3.68\, h_{liq}/D) - 2.01}{3.68\,(h_{liq}/D)\,\sinh(3.68\, h_{liq}/D)}\right)$$

**Variables adicionales:**
| Símbolo | Descripción | Uso |
|---------|------------|-----|
| $h_i$ | Altura de aplicación de masa impulsiva | Momento de volcamiento en base del cuerpo |
| $h_c$ | Altura de aplicación de masa convectiva | Momento de volcamiento en base del cuerpo |
| $h'_i$ | Altura impulsiva incluyendo presión sobre fondo | Momento de volcamiento en fundación |
| $h'_c$ | Altura convectiva incluyendo presión sobre fondo | Momento de volcamiento en fundación |

---

## Períodos de Vibración

### Período Impulsivo

$$T_i = \frac{C_i \cdot h_{liq} \cdot \sqrt{\gamma_{liq} / g}}{\sqrt{e_{pared} / D} \cdot \sqrt{E} \cdot 1000} \quad [s]$$

donde $C_i$ es un coeficiente que depende de $h_{liq}/D$ (tablas de API 650 Fig. E-1).

### Período Convectivo

$$T_c = \frac{2\pi}{\sqrt{3.68 \cdot \tanh(3.68\, h_{liq}/D)}} \cdot \sqrt{\frac{D}{g}}$$

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $T_i$ | Período impulsivo | s |
| $T_c$ | Período convectivo | s |
| $e_{pared}$ | Espesor de la pared del estanque | m |
| $E$ | Módulo de elasticidad del acero | MPa |
| $\gamma_{liq}$ | Peso específico del líquido | N/m³ |
| $g$ | Aceleración de gravedad (9.81) | m/s² |
| $C_i$ | Coeficiente de período impulsivo | — |

> **Nota:** El período convectivo $T_c$ es típicamente largo (3–10 s) y solo depende de la geometría. El período impulsivo $T_i$ es corto (0.05–0.5 s) y depende también de la rigidez del cuerpo.

---

## Parámetros Sísmicos (NCh2369 §11.2.1)

| Parámetro | Impulsivo | Convectivo |
|-----------|----------|-----------|
| Factor R | $R_i = 4$ | $R_c = 1.5$ |
| Amortiguamiento ξ | 2% | 0.5% |
| Factor η | $(0.05/0.02)^{0.4} = 1.45$ | $(0.05/0.005)^{0.4} = 2.51$ |

---

## Ejemplo de Uso

Estanque cilíndrico: $D = 12$ m, $h_{liq} = 11$ m, $e_{pared} = 5$ mm, agua ($\gamma = 9{,}810$ N/m³):

$$h_{liq}/D = 11/12 = 0.917$$

$$M_{liq} = \rho \cdot \pi (D/2)^2 \cdot h_{liq} = 1{,}000 \times \pi \times 6^2 \times 11 = 1{,}244{,}071 \text{ kg}$$

$$m_i = 1{,}244{,}071 \times \frac{\tanh(0.866 \times 12/11)}{0.866 \times 12/11} = 1{,}244{,}071 \times 0.751 = 934{,}417 \text{ kg}$$

$$T_c = \frac{2\pi}{\sqrt{3.68 \times \tanh(3.68 \times 11/12)}} \times \sqrt{\frac{12}{9.81}} = 3.56 \text{ s}$$
```

- [x] Copy and paste code below into `wiki/formulas/NCh2369-Spectrum.md`:

```markdown
---
title: "NCh2369-2025 — Espectro Sísmico de Diseño"
type: formula
standard_ref: "NCh2369-2025"
chapter: "5–7"
section: "§5, §6, §7"
variables: [S_a, A_r, S, T_0, r, p, q, eta, xi, I, R, R_star, T]
units: "SI"
tags: [sismo, espectro, NCh2369, chile, amortiguamiento]
related:
  - ../standards/NCh2369-2025.md
  - ../concepts/Seismic-Design-Principles.md
  - ../applications/Tank-Seismic-Analysis.md
created: 2026-04-09
updated: 2026-04-09
---

# NCh2369-2025 — Espectro Sísmico de Diseño

## Fuente
[NCh2369-2025](../standards/NCh2369-2025.md): Capítulos 5 (Zonificación), 6 (Suelos), 7 (Espectro)

---

## Espectro de Aceleración Bruto

$$S_a^{bruta}(T) = A_r \cdot S \cdot \frac{1 + r\left(\dfrac{T'}{T_0}\right)^p}{1 + \left(\dfrac{T'}{T_0}\right)^q}$$

donde:
- $T' = T$ para dirección horizontal
- $T' = T / 1.7$ para dirección vertical

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $S_a^{bruta}$ | Aceleración espectral bruta | g |
| $A_r$ | Aceleración efectiva de la zona | g |
| $S$ | Factor de amplificación por suelo | — |
| $T$ | Período de la estructura | s |
| $T_0$ | Período de esquina del suelo | s |
| $r, p, q$ | Parámetros de forma espectral por tipo de suelo | — |

---

## Factor de Amortiguamiento

$$\eta = \left(\frac{0.05}{\xi}\right)^{0.4}$$

| Caso | ξ | η |
|------|---|---|
| Estructura convencional | 5% | 1.00 |
| Estanque impulsivo | 2% | 1.45 |
| Estanque convectivo | 0.5% | 2.51 |

---

## Espectro de Diseño

$$S_a(T) = \frac{I \cdot \eta \cdot S_a^{bruta}(T)}{R^*}$$

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $S_a$ | Aceleración espectral de diseño | g |
| $I$ | Factor de importancia | — |
| $\eta$ | Factor de amortiguamiento | — |
| $R^*$ | Factor de reducción efectivo | — |

---

## Factor de Reducción R* (Período Corto)

Para $T > 0.16\, R\, T_1$:

$$R^* = R$$

Para $T \leq 0.16\, R\, T_1$:

$$R^* = 1.5 + (R - 1.5) \cdot \frac{T}{0.16\, R\, T_1}$$

Esto genera una interpolación lineal desde $R^* = 1.5$ (en $T = 0$) hasta $R^* = R$ (en $T = 0.16\, R\, T_1$).

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $R$ | Factor de reducción de respuesta | — |
| $R^*$ | Factor de reducción efectivo (ajustado por período corto) | — |
| $T_1$ | Período fundamental de la estructura | s |

---

## Parámetros de Zona Sísmica

| Zona | $A_r$ [g] |
|------|-----------|
| 1 | 0.20 |
| 2 | 0.30 |
| 3 | 0.40 |

## Parámetros de Suelo

| Suelo | Descripción | S | r | $T_0$ [s] | p | q |
|-------|------------|---|---|-----------|---|---|
| A | Roca dura | 0.90 | 3.0 | 0.15 | 1.00 | 2.50 |
| B | Roca blanda / suelo muy denso | 1.00 | 3.0 | 0.30 | 1.00 | 2.50 |
| C | Suelo denso / rígido | 1.05 | 3.5 | 0.40 | 1.50 | 2.80 |
| D | Suelo medianamente denso | 1.20 | 4.0 | 0.75 | 1.70 | 3.00 |
| E | Suelo blando | 1.30 | 4.5 | 1.20 | 1.85 | 3.00 |

---

## Espectro Vertical

El espectro vertical se obtiene del horizontal usando:

- Factor de escala: $0.7\sqrt{}$ del espectro horizontal (regla general NCh)
- Período equivalente: $T' = T / 1.7$

---

## Ejemplo de Uso

Estanque en Zona 3, Suelo C, componente impulsiva ($\xi = 2\%$, $R_i = 4$, $I = 1.2$):

$$A_r = 0.40g, \quad S = 1.05, \quad T_0 = 0.40, \quad r = 3.5, \quad p = 1.50, \quad q = 2.80$$

$$\eta = (0.05/0.02)^{0.4} = 1.45$$

Para $T_i = 0.15$ s:

$$S_a^{bruta} = 0.40 \times 1.05 \times \frac{1 + 3.5 \times (0.15/0.40)^{1.5}}{1 + (0.15/0.40)^{2.8}} = 0.40 \times 1.05 \times \frac{1 + 0.804}{1 + 0.039} = 0.420 \times 1.735 = 0.729g$$

Usando $R^* = R = 4$ (suponiendo $T_i > 0.16 R T_1$):

$$S_a = \frac{1.2 \times 1.45 \times 0.729}{4} = 0.317g$$
```

- [x] Actualizar backlinks en estándares — agregar/actualizar la sección "Fórmulas Relacionadas" en cada archivo de estándares.

En `wiki/standards/ACI-318-25.md`, la sección ya contiene el link correcto. No requiere cambios.

En `wiki/standards/API-650.md`, la sección ya contiene el link correcto. No requiere cambios.

En `wiki/standards/NCh2369-2025.md`, la sección ya contiene el link correcto. No requiere cambios.

- [x] Actualizar `wiki/index.md` — reemplazar la sección Formulas con:

```markdown
## Formulas
| Página | Descripción |
|--------|------------|
| [ACI-318-Ch10-Columns](formulas/ACI-318-Ch10-Columns.md) | Diseño de columnas — flexo-compresión, esbeltez, refuerzo |
| [API-650-Seismic](formulas/API-650-Seismic.md) | Análisis sísmico de estanques — masas, alturas, períodos |
| [NCh2369-Spectrum](formulas/NCh2369-Spectrum.md) | Espectro sísmico de diseño — forma espectral, amortiguamiento, R* |
```

##### Step 3 Verification Checklist
- [x] `wiki/formulas/ACI-318-Ch10-Columns.md` tiene frontmatter con: `standard_ref`, `chapter`, `section`, `variables[]`, `units`
- [x] `wiki/formulas/API-650-Seismic.md` tiene frontmatter con los mismos campos
- [x] `wiki/formulas/NCh2369-Spectrum.md` tiene frontmatter con los mismos campos
- [x] Cada fórmula contiene: ecuación LaTeX, tabla de variables con unidades, condiciones de aplicación, ejemplo numérico
- [x] Cross-refs bidireccionales: cada fórmula linka a su estándar y cada estándar lista la fórmula en "Fórmulas Relacionadas"
- [x] `wiki/index.md` sección Formulas lista las 3 páginas con links

#### Step 3 STOP & COMMIT
**STOP & COMMIT:** Agent must stop here and wait for the user to test, stage, and commit the change.

---

#### Step 4: Base de Conocimiento — Materiales, Conceptos y Aplicaciones

- [x] Copy and paste code below into `wiki/materials/Concrete-Properties.md`:

```markdown
---
title: "Propiedades del Concreto"
type: material
material_type: "concrete"
properties: [f_c, E_c, f_r, gamma, lambda_s]
tags: [concreto, hormigón, materiales, propiedades]
related:
  - ../standards/ACI-318-25.md
  - ../formulas/ACI-318-Ch10-Columns.md
  - ../concepts/Load-Combinations.md
created: 2026-04-09
updated: 2026-04-09
---

# Propiedades del Concreto

## Propiedades de Diseño

| Propiedad | Símbolo | Fórmula / Valor | Unidad | Referencia |
|-----------|---------|-----------------|--------|-----------|
| Resistencia a compresión | $f'_c$ | Especificada por diseñador | MPa | ACI 318-25 §19.2.1 |
| Módulo de elasticidad | $E_c$ | $4{,}700\sqrt{f'_c}$ | MPa | ACI 318-25 §19.2.2 |
| Módulo de ruptura | $f_r$ | $0.62\lambda\sqrt{f'_c}$ | MPa | ACI 318-25 §19.2.3 |
| Peso unitario (concreto normal) | $\gamma_c$ | 2,400 (23.5 kN/m³) | kg/m³ | ACI 318-25 §19.2.2 |
| Factor de efecto de tamaño | $\lambda_s$ | $\sqrt{2/(1 + d/250)} \leq 1.0$ | — | ACI 318-25 §22.5, §22.6 |
| Factor λ (concreto normal) | $\lambda$ | 1.0 | — | ACI 318-25 §19.2.4 |
| Factor λ (concreto liviano) | $\lambda$ | 0.75–0.85 | — | ACI 318-25 §19.2.4 |

## Grados Típicos

| Grado | $f'_c$ [MPa] | $E_c$ [MPa] | $f_r$ [MPa] | Uso típico |
|-------|-------------|-------------|-------------|-----------|
| H20 | 20 | 21,019 | 2.77 | Elementos menores, rellenos |
| H25 | 25 | 23,500 | 3.10 | Fundaciones, muros |
| H30 | 30 | 25,743 | 3.40 | Vigas, columnas estándar |
| H35 | 35 | 27,806 | 3.67 | Industrial, estanques |
| H40 | 40 | 29,725 | 3.92 | Losas pesadas, alta durabilidad |
| H45 | 45 | 31,528 | 4.16 | Elementos especiales |
| H50 | 50 | 33,234 | 4.38 | Alta resistencia |

> **Nota:** $E_c = 4{,}700\sqrt{f'_c}$ es para concreto de peso normal ($\gamma_c \approx 2{,}400$ kg/m³). Para otros pesos, $E_c = 0.043 w_c^{1.5} \sqrt{f'_c}$.

## Factor de Efecto de Tamaño $\lambda_s$

Introducido en ACI 318-19 y mantenido en ACI 318-25. Reduce la resistencia al corte en elementos con $d > 250$ mm:

$$\lambda_s = \sqrt{\frac{2}{1 + d/250}} \leq 1.0$$

| $d$ [mm] | $\lambda_s$ |
|-----------|------------|
| 250 | 1.000 |
| 300 | 0.953 |
| 400 | 0.877 |
| 500 | 0.816 |
| 600 | 0.767 |
| 700 | 0.725 |

> **Impacto:** Para losas gruesas industriales ($d > 300$ mm), $\lambda_s$ reduce significativamente $V_c$. No usar tablas de losas convencionales sin este factor.

## Clases de Exposición (ACI 318-25, Cap. 19)

| Clase | Condición | Requisitos principales |
|-------|-----------|----------------------|
| F0 | Sin ciclos hielo-deshielo | Sin requisitos especiales |
| F1 | Ciclo hielo-deshielo, exposición moderada | $f'_c \geq 31$ MPa, aire incorporado |
| S1 | Contacto con sulfatos (150–1500 ppm) | Cemento Tipo II, $w/c \leq 0.50$ |
| W1 | Humedad, sin cloruros | $f'_c \geq 28$ MPa |
| W2 | Contacto frecuente con agua | $f'_c \geq 35$ MPa, $w/c \leq 0.45$ |
| C1 | Exposición moderada a cloruros | $f'_c \geq 30$ MPa, recubrimiento mayor |
| C2 | Exposición severa a cloruros | $f'_c \geq 35$ MPa, $w/c \leq 0.40$ |

## Normativa Aplicable
- [ACI 318-25](../standards/ACI-318-25.md) — Cap. 19 (propiedades y durabilidad del concreto)

## Notas de Diseño

- Para ambiente minero (abrasión, hidrocarburos): especificar $f'_c \geq 35$ MPa con endurecedor superficial de cuarzo/corindón
- Cubrimiento mínimo en exposición severa: $c_{min} \geq 50$ mm (ACI 318-25 Tabla 20.6.1.3)
- Fibras de acero (30–50 kg/m³ ASTM A820) incrementan $V_c$ aprox. 30% y reducen fisuración en losas industriales
```

- [x] Copy and paste code below into `wiki/materials/Steel-Reinforcing.md`:

```markdown
---
title: "Acero de Refuerzo"
type: material
material_type: "steel"
properties: [f_y, f_u, E_s, epsilon_y]
tags: [acero, refuerzo, materiales, barras]
related:
  - ../standards/ACI-318-25.md
  - ../formulas/ACI-318-Ch10-Columns.md
  - ../applications/Column-Design-Workflow.md
created: 2026-04-09
updated: 2026-04-09
---

# Acero de Refuerzo

## Propiedades de Diseño

| Propiedad | Símbolo | Valor | Unidad | Referencia |
|-----------|---------|-------|--------|-----------|
| Módulo de elasticidad | $E_s$ | 200,000 | MPa | ACI 318-25 §20.2.2 |
| Deformación de fluencia | $\varepsilon_y$ | $f_y / E_s$ | — | — |

## Grados Disponibles (ASTM A615)

| Grado | $f_y$ [MPa] | $f_u$ mín [MPa] | Elongación mín. | Uso típico |
|-------|-------------|-----------------|-----------------|-----------|
| 40 (280) | 280 | 420 | 11–12% | Uso limitado, estribos |
| 60 (420) | 420 | 620 | 9% | **Estándar en Chile y Latinoamérica** |
| 75 (520) | 520 | 690 | 7% | Columnas de alta carga |
| 80 (550) | 550 | 690 | 7% | Elementos especiales (ACI 318-25 permite) |

> **Práctica estándar en Chile:** Grado 60 (420 MPa) para refuerzo longitudinal y transversal. ACI 318-25 permite hasta Grado 100 (690 MPa) bajo ciertas restricciones.

## Barras — Áreas y Pesos

| Designación | Ø [mm] | Área [mm²] | Peso [kg/m] |
|-------------|--------|-----------|------------|
| Ø8 | 8 | 50.3 | 0.395 |
| Ø10 | 10 | 78.5 | 0.617 |
| Ø12 | 12 | 113 | 0.888 |
| Ø16 | 16 | 201 | 1.578 |
| Ø18 | 18 | 254 | 2.000 |
| Ø20 | 20 | 314 | 2.466 |
| Ø22 | 22 | 380 | 2.984 |
| Ø25 | 25 | 491 | 3.853 |
| Ø28 | 28 | 616 | 4.834 |
| Ø32 | 32 | 804 | 6.313 |
| Ø36 | 36 | 1,018 | 7.990 |

## Restricciones Sísmicas

Para elementos en zonas sísmicas especiales (ACI 318-25, Cap. 18):

- Refuerzo longitudinal: $f_y \leq 550$ MPa (Grado 80 máximo)
- Refuerzo transversal: $f_{yt} \leq 420$ MPa para confinamiento, $\leq 690$ MPa para corte
- Relación $f_u/f_y \geq 1.25$ (para asegurar ductilidad)

## Normativa Aplicable
- [ACI 318-25](../standards/ACI-318-25.md) — Cap. 20 (propiedades del acero, durabilidad)

## Notas de Diseño

- Verificar certificados de calidad del acero: resistencia real vs nominal
- En ambiente minero con exposición a cloruros: considerar acero galvanizado o acero inoxidable
- El recubrimiento mínimo protege al acero de la corrosión; es la primera línea de defensa
```

- [x] Copy and paste code below into `wiki/concepts/Seismic-Design-Principles.md`:

```markdown
---
title: "Principios de Diseño Sísmico"
type: concept
domain: "sismo"
level: "fundamental"
tags: [sismo, diseño-sísmico, espectro, amortiguamiento, zonificación]
related:
  - ../standards/NCh2369-2025.md
  - ../standards/ACI-318-25.md
  - ../formulas/NCh2369-Spectrum.md
  - ../formulas/API-650-Seismic.md
  - ../applications/Tank-Seismic-Analysis.md
  - ../references/Chile-Specific-Variants.md
created: 2026-04-09
updated: 2026-04-09
---

# Principios de Diseño Sísmico

## Definición

El diseño sísmico busca garantizar que las estructuras resistan terremotos con un nivel de desempeño aceptable: protección de la vida (estado límite último) y limitación de daño (serviciabilidad). Se basa en definir la demanda sísmica mediante un espectro de diseño y verificar que la capacidad de la estructura sea suficiente.

## Principios Fundamentales

### 1. Zonificación Sísmica

El territorio se divide en zonas según el nivel de peligro sísmico. Cada zona tiene una aceleración efectiva de diseño ($A_r$).

- **Chile (NCh2369):** 3 zonas — Zona 1 (0.20g), Zona 2 (0.30g), Zona 3 (0.40g)
- Las zonas de mayor peligro se concentran en la costa del Pacífico (subducción de las placas de Nazca y Sudamericana)

### 2. Clasificación de Suelos

El tipo de suelo amplifica o atenúa las ondas sísmicas. La clasificación se basa en la velocidad de ondas de corte ($V_s$) o ensayos geotécnicos.

| Tipo | Descripción | Efecto |
|------|------------|--------|
| A (Roca dura) | $V_s > 900$ m/s | Mínima amplificación |
| B (Roca) | 360 < $V_s$ ≤ 760 m/s | Amplificación leve |
| C (Suelo denso) | 180 < $V_s$ ≤ 360 m/s | Amplificación moderada |
| D (Suelo blando) | $V_s$ ≤ 180 m/s | Amplificación significativa |
| E (Suelo muy blando) | Suelos especiales | Amplificación severa, posible licuefacción |

### 3. Espectro de Diseño

El espectro de respuesta describe la aceleración máxima esperada en función del período de la estructura. Se construye a partir de:

- **Aceleración de zona** ($A_r$): peligro sísmico base
- **Amplificación de suelo** ($S$): efecto local del suelo
- **Forma espectral**: depende de parámetros del suelo ($r$, $T_0$, $p$, $q$)
- **Amortiguamiento** ($\eta$): ajuste por disipación de energía
- **Factor de reducción** ($R^*$): reducción por ductilidad y redundancia
- **Importancia** ($I$): amplificación para estructuras esenciales

Ver: [NCh2369 — Espectro Sísmico](../formulas/NCh2369-Spectrum.md)

### 4. Amortiguamiento

El amortiguamiento mide la disipación de energía de la estructura. Valores típicos:

| Estructura | ξ | η |
|-----------|---|---|
| Edificios de hormigón | 5% | 1.00 |
| Estructuras de acero | 2% | 1.45 |
| Estanques — componente impulsiva | 2% | 1.45 |
| Estanques — componente convectiva | 0.5% | 2.51 |
| Equipos rígidos | 2–3% | 1.26–1.45 |

$$\eta = \left(\frac{0.05}{\xi}\right)^{0.4}$$

> Un amortiguamiento menor que 5% **incrementa** la demanda sísmica (η > 1).

### 5. Factores de Reducción de Respuesta

El factor $R$ reduce la demanda elástica al reconocer la capacidad dúctil de la estructura:

| Sistema | R típico |
|---------|---------|
| Marco de momento especial (hormigón) | 7–8 |
| Marco de momento ordinario | 3 |
| Muro especial de hormigón | 5–6 |
| Estanque sobre suelo (impulsivo) | 4 |
| Estanque sobre suelo (convectivo) | 1.5 |

> El factor $R$ para la componente convectiva es bajo (1.5) porque el sloshing es esencialmente elástico y no disipa energía por deformación inelástica.

### 6. Factor de Amplificación Dinámica (DAF)

Para cargas de impacto y vehículos (no sísmico, pero relacionado con diseño dinámico):

$$P_{din} = DAF \times P_{estática}$$

| Condición | DAF |
|-----------|-----|
| Superficie lisa, baja velocidad | 1.10–1.20 |
| Juntas o badenes | 1.25–1.40 |
| Frenado de emergencia | 1.30–1.50 |
| Impacto en rampas | 1.50–2.00 |

## Estándares Relacionados

- [NCh2369-2025](../standards/NCh2369-2025.md) — Diseño sísmico industrial (Chile)
- [ACI 318-25](../standards/ACI-318-25.md) — Cap. 18: diseño sísmico de hormigón
- [API 650](../standards/API-650.md) — Annex E: diseño sísmico de estanques

## Fórmulas Relacionadas

- [NCh2369 — Espectro](../formulas/NCh2369-Spectrum.md)
- [API 650 — Sismo](../formulas/API-650-Seismic.md)

## Referencias

- NCh2369-2025, Capítulos 5–7
- ACI 318-25, Capítulo 18
- API 650, Annex E
```

- [x] Copy and paste code below into `wiki/concepts/Load-Combinations.md`:

```markdown
---
title: "Combinaciones de Carga"
type: concept
domain: "hormigón"
level: "fundamental"
tags: [cargas, combinaciones, factores, LRFD, ACI]
related:
  - ../standards/ACI-318-25.md
  - ../concepts/Seismic-Design-Principles.md
  - ../applications/Column-Design-Workflow.md
created: 2026-04-09
updated: 2026-04-09
---

# Combinaciones de Carga

## Definición

Las combinaciones de carga definen los escenarios de diseño que la estructura debe resistir. El método LRFD (Load and Resistance Factor Design) aplica factores de mayoración a las cargas y factores de reducción ($\phi$) a la resistencia, asegurando que $\phi R_n \geq U$ para cada combinación.

## Combinaciones ACI 318-25 (§5.3)

### Combinaciones Principales

$$U_1 = 1.4D$$

$$U_2 = 1.2D + 1.6L + 0.5(L_r \text{ ó } S \text{ ó } R)$$

$$U_3 = 1.2D + 1.6(L_r \text{ ó } S \text{ ó } R) + (L \text{ ó } 0.5W)$$

$$U_4 = 1.2D + 1.0W + L + 0.5(L_r \text{ ó } S \text{ ó } R)$$

$$U_5 = 1.2D + 1.0E + L + 0.2S$$

$$U_6 = 0.9D + 1.0W$$

$$U_7 = 0.9D + 1.0E$$

### Variables

| Símbolo | Carga |
|---------|-------|
| $D$ | Carga muerta (peso propio + acabados permanentes) |
| $L$ | Carga viva |
| $L_r$ | Carga viva de techo |
| $S$ | Carga de nieve |
| $R$ | Carga de lluvia |
| $W$ | Carga de viento |
| $E$ | Carga sísmica |

## Aplicación en Minería

Para losas industriales con tráfico de camiones mineros, la carga de vehículo se trata como carga viva $L$ amplificada por el factor de amplificación dinámica (DAF):

$$L_{diseño} = DAF \times P_{estática}$$

La combinación gobernante suele ser:

$$U_2 = 1.2D + 1.6L_{diseño}$$

> En este caso, $L_{diseño}$ ya incluye el DAF, por lo que el factor 1.6 se aplica sobre la carga amplificada dinámicamente.

## Combinaciones para Sismo

Las combinaciones sísmicas ($U_5$, $U_7$) incluyen la carga $E$ que proviene del espectro de diseño:

$$E = E_h + E_v = \rho Q_E + 0.2 S_{DS} D$$

donde:
- $\rho$ = factor de redundancia (1.0 o 1.3)
- $Q_E$ = efecto de fuerzas sísmicas horizontales
- $S_{DS}$ = parámetro de aceleración espectral de diseño

## Estándares Relacionados

- [ACI 318-25](../standards/ACI-318-25.md) — Cap. 5 §5.3

## Fórmulas Relacionadas

- [ACI 318 Cap. 10 — Columnas](../formulas/ACI-318-Ch10-Columns.md) — las verificaciones usan $P_u$ y $M_u$ de estas combinaciones

## Referencias

- ACI 318-25 §5.3
- ASCE 7-22: Minimum Design Loads (fuente principal de combinaciones)
```

- [x] Copy and paste code below into `wiki/applications/Column-Design-Workflow.md`:

```markdown
---
title: "Workflow: Diseño de Columnas de Hormigón"
type: application
domain: "hormigón"
standards_used: ["ACI-318-25"]
tags: [columnas, diseño, workflow, hormigón]
related:
  - ../standards/ACI-318-25.md
  - ../formulas/ACI-318-Ch10-Columns.md
  - ../materials/Concrete-Properties.md
  - ../materials/Steel-Reinforcing.md
  - ../concepts/Load-Combinations.md
created: 2026-04-09
updated: 2026-04-09
---

# Workflow: Diseño de Columnas de Hormigón Armado

## Objetivo
Diseñar una columna de hormigón armado sometida a carga axial y momentos flectores según ACI 318-25.

## Estándares Aplicables
- [ACI 318-25](../standards/ACI-318-25.md): Cap. 10 (Columnas), Cap. 22 §22.4 (Resistencia combinada), Cap. 6 §6.6/6.7 (Esbeltez)

## Datos de Entrada

| Parámetro | Símbolo | Descripción |
|-----------|---------|------------|
| Cargas factorizadas | $P_u$, $M_{ux}$, $M_{uy}$ | De combinaciones de carga §5.3 |
| Dimensiones | $b$, $h$ | Sección transversal propuesta |
| Longitud no arriostrada | $\ell_u$ | Altura libre entre restricciones laterales |
| Condiciones de apoyo | $k$ | Factor de longitud efectiva |
| Resistencia del concreto | $f'_c$ | Grado seleccionado |
| Fluencia del acero | $f_y$ | Grado 60 (420 MPa) estándar |

## Procedimiento

### 1. Predimensionamiento
- Estimar sección por carga axial: $A_g \approx P_u / (0.45 f'_c)$ (primera aproximación)
- Relación b/h: usar 1:1 para cargas centradas, hasta 1:3 para momentos dominantes

### 2. Verificar Esbeltez (§6.2.5)
- Calcular $k\ell_u / r$ donde $r \approx 0.3h$ (rectangular)
- Si $k\ell_u / r \leq 22$ (arriostrada): se puede ignorar efecto de esbeltez
- Si $k\ell_u / r > 22$: amplificar momentos por efectos de segundo orden (§6.6.4)

### 3. Determinación de Refuerzo
- Seleccionar cuantía: $\rho_g$ entre 1% y 4% (recomendado)
- Calcular $A_{st} = \rho_g \times A_g$
- Seleccionar barras y distribución perimetral

### 4. Diagrama de Interacción P-M
- Construir diagrama de interacción $\phi P_n$ vs $\phi M_n$
- Verificar que el punto ($P_u$, $M_u$) quede dentro del diagrama
- Puntos clave: compresión pura, punto balanceado, flexión pura

### 5. Verificación de Diseño

| Verificación | Criterio | Referencia |
|-------------|----------|-----------|
| Capacidad axial-flexural | ($P_u$, $M_u$) dentro del diagrama φPn-φMn | §10.5, §22.4 |
| Cuantía de refuerzo | $0.01 \leq \rho_g \leq 0.08$ | §10.6 |
| Espaciado de estribos | $s \leq \min(16d_b, 48d_{estribo}, b)$ | §10.7.6.2 |
| Recubrimiento libre | $c \geq 40$ mm (exposición normal) | Tabla 20.6.1.3 |
| Número mínimo de barras | 4 (estribos rectangulares) | §10.7 |

### 6. Diseño de Refuerzo Transversal
- Estribos cerrados Ø10 o Ø12 mínimo
- Espaciado según §10.7.6.2
- En zonas sísmicas: confinar con estribos adicionales (Cap. 18)

### 7. Biaxialidad (si aplica)
- Verificar con método de Bresler (punto de carga recíproco):

$$\frac{1}{P_{ni}} = \frac{1}{P_{nx}} + \frac{1}{P_{ny}} - \frac{1}{P_0}$$

## Verificaciones Finales
- [ ] ($P_u$, $M_u$) dentro del diagrama de interacción
- [ ] Cuantía entre 1% y 8%
- [ ] Esbeltez verificada o momentos amplificados
- [ ] Estribos con espaciado correcto
- [ ] Recubrimiento adecuado para clase de exposición
- [ ] Compatibilidad con conexiones (vigas, losas)

## Fórmulas Utilizadas
- [ACI 318 Cap. 10 — Columnas](../formulas/ACI-318-Ch10-Columns.md)

## Referencias
- [Propiedades del Concreto](../materials/Concrete-Properties.md)
- [Acero de Refuerzo](../materials/Steel-Reinforcing.md)
- [Combinaciones de Carga](../concepts/Load-Combinations.md)
```

- [x] Copy and paste code below into `wiki/applications/Tank-Seismic-Analysis.md`:

```markdown
---
title: "Workflow: Análisis Sísmico de Estanque Cilíndrico"
type: application
domain: "estanques"
standards_used: ["API-650", "NCh2369-2025"]
tags: [estanques, sismo, workflow, API-650, NCh2369]
related:
  - ../standards/API-650.md
  - ../standards/NCh2369-2025.md
  - ../formulas/API-650-Seismic.md
  - ../formulas/NCh2369-Spectrum.md
  - ../concepts/Seismic-Design-Principles.md
created: 2026-04-09
updated: 2026-04-09
---

# Workflow: Análisis Sísmico de Estanque Cilíndrico

## Objetivo
Evaluar la respuesta sísmica de un estanque cilíndrico de acero soldado apoyado sobre suelo, combinando el modelo de masas de API 650 con el espectro sísmico de NCh2369.

## Estándares Aplicables
- [API 650](../standards/API-650.md): Annex E — modelo de masas equivalentes, alturas, períodos
- [NCh2369-2025](../standards/NCh2369-2025.md): espectro de diseño, factores R y ξ para estanques

## Datos de Entrada

| Parámetro | Símbolo | Descripción |
|-----------|---------|------------|
| Diámetro | $D$ | Diámetro interior del estanque |
| Altura del líquido | $h_{liq}$ | Nivel operacional del líquido |
| Espesor de pared | $e_{pared}$ | Espesor del primer anillo (o promedio ponderado) |
| Densidad del líquido | $\rho_{liq}$ | Agua: 1,000 kg/m³; petróleo: 800–950 kg/m³ |
| Zona sísmica | — | Zona 1, 2 o 3 (NCh2369) |
| Tipo de suelo | — | A, B, C, D o E (NCh2369) |
| Factor de importancia | $I$ | 1.0 (normal) o 1.2 (esencial) |

## Procedimiento

### 1. Cálculo de Masas Equivalentes
Determinar la relación $h_{liq}/D$ y calcular:

$$m_i = M_{liq} \cdot \frac{\tanh(0.866\,D/h_{liq})}{0.866\,D/h_{liq}}$$

$$m_c = M_{liq} \cdot 0.23 \cdot \frac{\tanh(3.68\,h_{liq}/D)}{h_{liq}/D}$$

Verificar: $m_i + m_c \approx M_{liq}$

### 2. Alturas de Aplicación
Calcular $h_i$, $h_c$ (sin presión sobre fondo) y $h'_i$, $h'_c$ (con presión sobre fondo):
- Ver fórmulas en [API 650 — Sismo](../formulas/API-650-Seismic.md)

### 3. Períodos de Vibración

$$T_c = \frac{2\pi}{\sqrt{3.68\,\tanh(3.68\,h_{liq}/D)}} \cdot \sqrt{\frac{D}{g}}$$

$T_i$ requiere el coeficiente $C_i$ de tablas API 650 Fig. E-1.

### 4. Espectro de Diseño (NCh2369)
Para cada componente, calcular el espectro con los parámetros apropiados:

| Componente | ξ | R | η |
|-----------|---|---|---|
| Impulsiva | 2% | 4 | 1.45 |
| Convectiva | 0.5% | 1.5 | 2.51 |

$$S_{a,i} = \frac{I \cdot 1.45 \cdot S_a^{bruta}(T_i)}{R_i^*}$$

$$S_{a,c} = \frac{I \cdot 2.51 \cdot S_a^{bruta}(T_c)}{R_c^*}$$

Ver: [NCh2369 — Espectro](../formulas/NCh2369-Spectrum.md)

### 5. Corte Basal y Momento Volcante

**Corte basal:**
$$V = m_i \cdot S_{a,i} \cdot g + m_c \cdot S_{a,c} \cdot g + m_{cuerpo} \cdot S_{a,i} \cdot g$$

O bien por combinación SRSS:
$$V = \sqrt{V_i^2 + V_c^2}$$

**Momento volcante en base del cuerpo:**
$$M_{base} = \sqrt{(m_i \cdot S_{a,i} \cdot g \cdot h_i)^2 + (m_c \cdot S_{a,c} \cdot g \cdot h_c)^2}$$

**Momento volcante en fundación (incluyendo presión sobre fondo):**
$$M_{fund} = \sqrt{(m_i \cdot S_{a,i} \cdot g \cdot h'_i)^2 + (m_c \cdot S_{a,c} \cdot g \cdot h'_c)^2}$$

### 6. Verificaciones

| Verificación | Criterio |
|-------------|----------|
| Estabilidad al volcamiento | $M_{resistente} / M_{volcante} \geq 1.5$ |
| Esfuerzos en cuerpo | σ compresión + σ flexión ≤ σ admisible |
| Anclaje | Si se requiere: diseño de pernos de anclaje |
| Oleaje (sloshing) | $d_s$ = altura libre al techo; verificar que no impacte |
| Conexiones | Nozzles y tuberías deben acomodar desplazamientos |

### 7. Oleaje (Sloshing Height)

$$d_s = 0.84 \cdot D \cdot S_{a,c}$$

La distancia libre entre el nivel del líquido y el techo debe ser $\geq d_s$ para evitar impacto.

## Verificaciones Finales
- [ ] Masas impulsiva + convectiva ≈ masa total del líquido
- [ ] $T_c$ en rango esperado (3–10 s para estanques típicos)
- [ ] Espectro calculado con ξ y R correctos para cada componente
- [ ] Momento volcante vs resistente verificado
- [ ] Altura de oleaje compatible con borde libre del estanque
- [ ] Esfuerzos en pared dentro de admisibles

## Fórmulas Utilizadas
- [API 650 — Sismo](../formulas/API-650-Seismic.md)
- [NCh2369 — Espectro](../formulas/NCh2369-Spectrum.md)

## Referencias
- [Principios de Diseño Sísmico](../concepts/Seismic-Design-Principles.md)
- [API 650](../standards/API-650.md)
- [NCh2369-2025](../standards/NCh2369-2025.md)
```

- [x] Actualizar `wiki/index.md` — reemplazar las secciones Materials, Concepts y Applications con:

```markdown
## Materials
| Página | Descripción |
|--------|------------|
| [Concrete-Properties](materials/Concrete-Properties.md) | Propiedades del concreto: f'c, Ec, fr, clases de exposición, factor λs |
| [Steel-Reinforcing](materials/Steel-Reinforcing.md) | Acero de refuerzo: grados, áreas de barras, restricciones sísmicas |

## Concepts
| Página | Descripción |
|--------|------------|
| [Seismic-Design-Principles](concepts/Seismic-Design-Principles.md) | Principios de diseño sísmico: zonificación, suelos, espectro, amortiguamiento |
| [Load-Combinations](concepts/Load-Combinations.md) | Combinaciones de carga ACI 318-25 §5.3, LRFD, aplicación industrial |

## Applications
| Página | Descripción |
|--------|------------|
| [Column-Design-Workflow](applications/Column-Design-Workflow.md) | Workflow de diseño de columnas de hormigón armado según ACI 318-25 |
| [Tank-Seismic-Analysis](applications/Tank-Seismic-Analysis.md) | Análisis sísmico de estanques cilíndricos con API 650 + NCh2369 |
```

##### Step 4 Verification Checklist
- [x] `wiki/materials/Concrete-Properties.md` tiene tabla de grados con $f'_c$, $E_c$, $f_r$ y fórmula de $E_c = 4700\sqrt{f'_c}$
- [x] `wiki/materials/Concrete-Properties.md` incluye tabla de $\lambda_s$ por espesor efectivo $d$
- [x] `wiki/materials/Steel-Reinforcing.md` tiene tabla de áreas de barras y grados ASTM A615
- [x] `wiki/concepts/Seismic-Design-Principles.md` explica zonificación, clasificación de suelo, amortiguamiento, factores R
- [x] `wiki/concepts/Seismic-Design-Principles.md` contiene referencias a NCh2369 y ACI 318
- [x] `wiki/concepts/Load-Combinations.md` lista las 7 combinaciones de ACI 318-25 §5.3
- [x] `wiki/applications/Column-Design-Workflow.md` tiene procedimiento completo sin links a notebooks
- [x] `wiki/applications/Tank-Seismic-Analysis.md` combina API 650 (masas) + NCh2369 (espectro)
- [x] `wiki/index.md` secciones Materials, Concepts, Applications están actualizadas con links

#### Step 4 STOP & COMMIT
**STOP & COMMIT:** Agent must stop here and wait for the user to test, stage, and commit the change.

---

#### Step 5: Referencias Cruzadas y Documentación

- [x] Copy and paste code below into `wiki/references/Standards-by-Domain.md`:

```markdown
---
title: "Estándares por Dominio"
type: reference
tags: [referencias, dominios, estándares, mapa]
related:
  - ../standards/ACI-318-25.md
  - ../standards/API-650.md
  - ../standards/NCh2369-2025.md
created: 2026-04-09
updated: 2026-04-09
---

# Estándares por Dominio

Mapa de dominios de ingeniería a estándares aplicables y páginas de la wiki.

---

## Hormigón Armado

| Estándar | Alcance | Página Wiki |
|----------|---------|------------|
| [ACI 318-25](../standards/ACI-318-25.md) | Diseño y construcción de concreto estructural | ✅ |
| ACI 350 | Estructuras de contención de líquidos (concreto) | ⬜ Pendiente |
| ACI 360R | Losas sobre terreno | ⬜ Pendiente |

**Fórmulas disponibles:**
- [Diseño de Columnas (Cap. 10)](../formulas/ACI-318-Ch10-Columns.md)

**Aplicaciones:**
- [Diseño de Columnas — Workflow](../applications/Column-Design-Workflow.md)

---

## Estanques de Almacenamiento

| Estándar | Alcance | Página Wiki |
|----------|---------|------------|
| [API 650](../standards/API-650.md) | Estanques atmosféricos de acero soldado | ✅ |
| API 620 | Estanques criogénicos y de baja presión | ⬜ Pendiente |
| API 653 | Inspección, reparación, alteración de estanques | ⬜ Pendiente |

**Fórmulas disponibles:**
- [Análisis Sísmico de Estanques (Annex E)](../formulas/API-650-Seismic.md)

**Aplicaciones:**
- [Análisis Sísmico de Estanques — Workflow](../applications/Tank-Seismic-Analysis.md)

---

## Diseño Sísmico

| Estándar | Alcance | Página Wiki |
|----------|---------|------------|
| [NCh2369-2025](../standards/NCh2369-2025.md) | Sismo para estructuras industriales (Chile) | ✅ |
| NCh433 | Sismo para edificios (Chile) | ⬜ Pendiente |
| ASCE 7-22 | Cargas mínimas de diseño (USA) | ⬜ Pendiente |
| [ACI 318-25 Cap. 18](../standards/ACI-318-25.md) | Diseño sísmico de hormigón armado | ✅ |

**Fórmulas disponibles:**
- [Espectro Sísmico NCh2369](../formulas/NCh2369-Spectrum.md)

**Conceptos:**
- [Principios de Diseño Sísmico](../concepts/Seismic-Design-Principles.md)

---

## Dominios Futuros (Pendientes)

| Dominio | Estándares principales | Estado |
|---------|----------------------|--------|
| Estructuras de Acero | AISC 360, AISC 341 | ⬜ |
| Geotecnia | NCh3206, AASHTO | ⬜ |
| Cargas y Combinaciones | ASCE 7-22 | ⬜ |
| Puentes | AASHTO LRFD | ⬜ |
| Tuberías | ASME B31.3, API 570 | ⬜ |
```

- [x] Copy and paste code below into `wiki/references/Chile-Specific-Variants.md`:

```markdown
---
title: "Variantes Específicas de Chile"
type: reference
tags: [chile, normativa, NCh, variantes, comparación]
related:
  - ../standards/NCh2369-2025.md
  - ../standards/ACI-318-25.md
  - ../standards/API-650.md
  - ../concepts/Seismic-Design-Principles.md
created: 2026-04-09
updated: 2026-04-09
---

# Variantes Específicas de Chile

Documentación de diferencias entre la práctica chilena y los estándares internacionales de origen.

---

## Diseño Sísmico: NCh2369 vs ASCE 7

| Aspecto | NCh2369 (Chile) | ASCE 7 (USA) |
|---------|-----------------|---------------|
| Zonas sísmicas | 3 zonas (0.20g–0.40g) | Mapas de peligro continuo ($S_S$, $S_1$) |
| Clasificación de suelos | A–E (basada en $V_s$, similar) | A–F (basada en $V_s$, similar) |
| Forma espectral | Ecuación paramétrica con $r$, $T_0$, $p$, $q$ | Plataforma + decaimiento $1/T$ |
| Amortiguamiento base | 5% (ajustable con η) | 5% (fijo en espectro) |
| Factor de reducción período corto | $R^*$ interpolado linealmente a 1.5 en $T=0$ | $C_s$ con límites inferior/superior |

> **Diferencia clave:** NCh2369 reduce $R^*$ a 1.5 en período cero (interpolación lineal), mientras que ASCE 7 usa $C_s = S_{DS}/(R/I_e)$ con un piso mínimo. El enfoque chileno es más conservador para estructuras rígidas.

## Estanques: Práctica Chilena vs API 650

| Aspecto | Práctica Chilena | API 650 Puro |
|---------|-----------------|-------------|
| Espectro sísmico | NCh2369-2025 | ASCE 7 (Annex E) |
| Factor R impulsivo | $R_i = 4$ (NCh2369 §11.2.1.6) | $R_{wi} = 4$ (Tabla E-4) |
| Factor R convectivo | $R_c = 1.5$ (NCh2369 §11.2.1.7) | $R_{wc} = 2$ (Tabla E-4) |
| Amortiguamiento impulsivo | ξ = 2% | ξ = 5% (implícito en espectro ASCE) |
| Amortiguamiento convectivo | ξ = 0.5% | ξ = 0.5% |
| Combinación de componentes | SRSS | SRSS |

> **Diferencia clave:** La práctica chilena usa ξ = 2% para la componente impulsiva (más conservador que el 5% implícito de API/ASCE), compensando con el factor η = 1.45.

## Hormigón Armado: Práctica Chilena con ACI 318

| Aspecto | Práctica Chilena | ACI 318 Original (USA) |
|---------|-----------------|----------------------|
| Código base | ACI 318 adoptado como referencia principal | Código propio |
| Complemento sísmico | NCh433 (edificios) + NCh2369 (industrial) | ASCE 7 + ACI 318 Cap. 18 |
| Grado de acero estándar | Gr. 60 (420 MPa) — ASTM A615 | Gr. 60 o Gr. 80 |
| Concreto típico | H30–H40 (f'c = 30–40 MPa) | 4000–6000 psi (28–41 MPa) |
| Recubrimiento | Según ACI 318 Tabla 20.6.1.3 (50 mm exposición severa) | Igual |
| Unidades | SI (MPa, mm, kN) | US Customary (psi, in, kip) o SI |

> **Nota:** Chile no tiene un código propio de hormigón armado equivalente al ACI 318. Se adopta directamente la edición SI del ACI 318, complementada con las normas sísmicas nacionales.

## Particularidades de la Ingeniería Chilena

### Alta Sismicidad
- Chile es uno de los países con mayor actividad sísmica del mundo (subducción Nazca-Sudamericana)
- La práctica de diseño es inherentemente conservadora en el ámbito sísmico
- Se diseña para sismos de subducción (larga duración, muchos ciclos)

### Minería
- Sector minero domina la ingeniería industrial en Chile
- Requiere diseño para cargas extremas (camiones de 400–850 t GVW)
- Estándares internacionales (ACI, API, AISC) se combinan con normativa sísmica chilena

### Normativa Local Relevante

| Norma | Tema |
|-------|------|
| NCh433 | Diseño sísmico de edificios |
| NCh2369 | Diseño sísmico de instalaciones industriales |
| NCh3171 | Diseño estructural — disposiciones generales y combinaciones de carga |
| NCh427 | Diseño de estructuras de acero (basada en AISC) |
| NCh2745 | Análisis y diseño de edificios con aislación sísmica |
```

- [x] Copy and paste code below into `README_WIKI.md` (en la raíz del proyecto):

```markdown
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
```

- [x] Reemplazar el contenido completo de `wiki/index.md` con la versión final:

```markdown
---
title: "Índice de la Wiki de Ingeniería"
type: index
created: 2026-04-09
updated: 2026-04-09
---

# Índice — Wiki de Ingeniería Estructural

Catálogo de todas las páginas de la wiki, organizado por categoría.

---

## Standards
| Página | Descripción |
|--------|------------|
| [ACI-318-25](standards/ACI-318-25.md) | Código de construcción para concreto estructural — edición 2025 SI |
| [API-650](standards/API-650.md) | Estanques cilíndricos de acero soldado para almacenamiento |
| [NCh2369-2025](standards/NCh2369-2025.md) | Diseño sísmico de estructuras e instalaciones industriales (Chile) |

## Formulas
| Página | Descripción |
|--------|------------|
| [ACI-318-Ch10-Columns](formulas/ACI-318-Ch10-Columns.md) | Diseño de columnas — flexo-compresión, esbeltez, refuerzo |
| [API-650-Seismic](formulas/API-650-Seismic.md) | Análisis sísmico de estanques — masas, alturas, períodos |
| [NCh2369-Spectrum](formulas/NCh2369-Spectrum.md) | Espectro sísmico de diseño — forma espectral, amortiguamiento, R* |

## Materials
| Página | Descripción |
|--------|------------|
| [Concrete-Properties](materials/Concrete-Properties.md) | Propiedades del concreto: f'c, Ec, fr, clases de exposición, factor λs |
| [Steel-Reinforcing](materials/Steel-Reinforcing.md) | Acero de refuerzo: grados, áreas de barras, restricciones sísmicas |

## Concepts
| Página | Descripción |
|--------|------------|
| [Seismic-Design-Principles](concepts/Seismic-Design-Principles.md) | Principios de diseño sísmico: zonificación, suelos, espectro, amortiguamiento |
| [Load-Combinations](concepts/Load-Combinations.md) | Combinaciones de carga ACI 318-25 §5.3, LRFD, aplicación industrial |

## Applications
| Página | Descripción |
|--------|------------|
| [Column-Design-Workflow](applications/Column-Design-Workflow.md) | Workflow de diseño de columnas de hormigón armado según ACI 318-25 |
| [Tank-Seismic-Analysis](applications/Tank-Seismic-Analysis.md) | Análisis sísmico de estanques cilíndricos con API 650 + NCh2369 |

## References
| Página | Descripción |
|--------|------------|
| [Standards-by-Domain](references/Standards-by-Domain.md) | Mapa de dominios → estándares aplicables |
| [Chile-Specific-Variants](references/Chile-Specific-Variants.md) | Diferencias entre práctica chilena y estándares internacionales |
```

- [x] Reemplazar el contenido completo de `wiki/log.md` con la primera entrada:

```markdown
---
title: "Log de Operaciones"
type: log
created: 2026-04-09
---

# Log de Operaciones — Wiki de Ingeniería

Registro cronológico append-only de operaciones sobre la wiki.

---

## [2026-04-09] setup | Estructura inicial de la wiki

**Operación:** Creación de la estructura base de la wiki de ingeniería.

**Páginas creadas:**
- Standards: ACI-318-25, API-650, NCh2369-2025
- Formulas: ACI-318-Ch10-Columns, API-650-Seismic, NCh2369-Spectrum
- Materials: Concrete-Properties, Steel-Reinforcing
- Concepts: Seismic-Design-Principles, Load-Combinations
- Applications: Column-Design-Workflow, Tank-Seismic-Analysis
- References: Standards-by-Domain, Chile-Specific-Variants

**Fuentes utilizadas:**
- `docs/Hormigon/ACI_318-25_SI_indice.md` — índice completo ACI 318-25
- `docs/Hormigon/losa_camiones_mineros.md` — diseño de losas para camiones mineros
- `notebooks/Sismo/espectro_sismico_NCh2369.ipynb` — fórmulas espectro NCh2369
- `notebooks/Estanques/estanque_cilindrico_API650.ipynb` — fórmulas API 650 Annex E
- `notebooks/Hormigon/columna_ACI318.ipynb` — template columna ACI 318

**Archivos de soporte:**
- `CLAUDE.md` — esquema operativo del agente
- `README_WIKI.md` — documentación de la wiki
- `wiki/index.md` — catálogo completo de páginas
```

##### Step 5 Verification Checklist
- [x] `wiki/references/Standards-by-Domain.md` mapea 3 dominios (hormigón, estanques, sismo) a sus códigos
- [x] `wiki/references/Chile-Specific-Variants.md` documenta diferencias NCh vs códigos internacionales en tablas comparativas
- [x] `README_WIKI.md` existe en la raíz y tiene secciones: Estructura, Operaciones (Ingest/Query/Lint), Convenciones de Idioma, Relación con Notebooks, Configuración
- [x] `wiki/index.md` lista TODAS las páginas (3 standards, 3 formulas, 2 materials, 2 concepts, 2 applications, 2 references = 14 páginas)
- [x] `wiki/log.md` tiene la entrada: `## [2026-04-09] setup | Estructura inicial de la wiki`
- [x] Todos los links relativos en frontmatter `related:` apuntan a archivos que existen
- [x] Todos los links en el cuerpo de las páginas son rutas relativas válidas

#### Step 5 STOP & COMMIT
**STOP & COMMIT:** Agent must stop here and wait for the user to test, stage, and commit the change.

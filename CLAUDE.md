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

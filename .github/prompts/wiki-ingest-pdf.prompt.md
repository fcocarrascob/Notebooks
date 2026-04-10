---
description: Ingesta un PDF normativo en la wiki de ingeniería estructural usando pypdf. Extrae secciones clave, lee el contenido y crea/actualiza páginas wiki según el schema de CLAUDE.md.
---

# Ingesta de PDF normativo → Wiki

Sigue el workflow de §4.1 de [CLAUDE.md](../../CLAUDE.md) para ingestar el archivo PDF indicado.

## Paso 1 — Verificar el PDF

Corre este script para ver el total de páginas y el índice de contenidos (TOC):

```python
from pypdf import PdfReader
reader = PdfReader(r'raw/pdf/NOMBRE_ARCHIVO.pdf')
print(f"Total páginas PDF: {len(reader.pages)}")

# Ver primeras páginas (portada + índice)
for i in range(min(10, len(reader.pages))):
    text = reader.pages[i].extract_text()
    if text and text.strip():
        print(f"\n=== PDF pág {i} ===")
        print(text[:800])
```

Con esto determina:
- El **offset** entre número de página del documento y el índice PDF (ej: doc p.1 = PDF idx 7 → offset = +6)
- Las secciones más importantes (números de capítulo y rangos de páginas)

## Paso 2 — Extraer secciones a archivos .txt

Extrae los capítulos técnicos más relevantes. Ajustar `sections` según el TOC encontrado en Paso 1.

```python
from pypdf import PdfReader
import os

reader = PdfReader(r'raw/pdf/NOMBRE_ARCHIVO.pdf')
OFFSET = 6  # ← Ajustar según Paso 1: doc_page + OFFSET = pdf_index

# Definir secciones a extraer: {nombre: (pdf_idx_start, pdf_idx_end)}
# Convertir páginas del documento: pdf_idx = doc_page + OFFSET
sections = {
    'Cap_X_NombreCapitulo': (doc_inicio + OFFSET, doc_fin + OFFSET),
    # Agregar más secciones según TOC
}

codigo = 'CODIGO'  # ej: 'NCh2369', 'ACI318', 'API650'
output_dir = f'raw/references/{codigo}_extract'
os.makedirs(output_dir, exist_ok=True)

for name, (start, end) in sections.items():
    pages = []
    for n in range(start, end + 1):
        text = reader.pages[n].extract_text()
        pages.append(f'--- PDF idx {n} (doc p.{n - OFFSET}) ---\n{text}')
    content = '\n\n'.join(pages)
    filepath = f'{output_dir}/{name}.txt'
    with open(filepath, 'w', encoding='utf-8') as f:
        f.write(content)
    print(f'{name}: {end - start + 1} páginas → {filepath}')
```

**Secciones prioritarias a buscar según tipo de norma:**

| Tipo de norma | Secciones clave |
|--------------|-----------------|
| Sísmica (NCh, ASCE) | Espectro, zonas sísmicas, tipos de suelo, Tabla R/ξ, combinaciones de carga |
| Concreto (ACI) | Materiales, flexión, corte, columnas, factores φ, detalles sísmicos |
| Estanques (API) | Diseño del manto, fondo, techo, sismo, viento, anclajes |
| Acero (AISC, NCh) | Perfiles, uniones, arriostramiento, pandeo |

## Paso 3 — Leer y analizar los archivos extraídos

Lee los archivos `.txt` usando `read_file`. Para archivos grandes, leer en bloques de ~300 líneas.

Identifica en cada archivo:
- **Fórmulas** con sus variables, condiciones y restricciones
- **Tablas** con valores numéricos (parámetros de diseño, factores)
- **Definiciones** de términos técnicos nuevos
- **Referencias cruzadas** a otras normas

## Paso 4 — Leer páginas wiki existentes

Antes de crear o editar, leer:
1. `wiki/index.md` — ver qué páginas existen
2. Las páginas de wiki relacionadas con la norma ingestada
3. Identificar discrepancias entre la wiki y el PDF verificado

## Paso 5 — Crear o actualizar páginas wiki

Seguir el schema de [CLAUDE.md](../../CLAUDE.md):

- **Idioma:** Español para contenido, Inglés para nombres de archivo e IDs
- **Frontmatter YAML obligatorio** en todas las páginas
- **Cross-references bidireccionales:** la norma lista sus fórmulas; cada fórmula linkea a su norma
- **Correcciones críticas:** marcar con ⚠️ o nota visible si se corrige un dato previamente erróneo

Tipos de páginas a crear/actualizar:
- `wiki/standards/` — página resumen de la norma
- `wiki/formulas/` — páginas de fórmulas por capítulo
- `wiki/materials/` — si hay propiedades de materiales nuevas
- `wiki/concepts/` — si hay conceptos técnicos nuevos

## Paso 6 — Actualizar index.md y log.md

**index.md:** Agregar filas a las tablas correspondientes.

**log.md:** Agregar entrada al inicio con este formato:
```markdown
## [FECHA] ingest | NOMBRE_ARCHIVO.pdf — descripción breve

**Páginas creadas:**
- `ruta/pagina.md` — descripción

**Páginas actualizadas:**
- `ruta/pagina.md` — descripción de cambios (incluir CORRECCIONES si aplica)

**Archivos de extracción intermedios:**
- `raw/references/CODIGO_extract/Cap_X.txt` — N páginas
```

---

## Notas operacionales

- **Texto mal extraído:** pypdf a veces une palabras sin espacios en PDFs escaneados. Si el texto es ilegible, indicarlo al usuario.
- **Tablas:** pypdf no extrae tablas con formato visual — reconstruirlas leyendo el texto y las notas de columnas.
- **Fórmulas:** Los caracteres matemáticos se extraen como texto plano. Reconstruir con notación LaTeX correcta.
- **Figuras e imágenes:** No se extraen. Notar su existencia pero no intentar describir su contenido.
- **Offset incorrecto:** Si los números de página no coinciden, ajustar el offset iterando con `reader.pages[i].extract_text()` hasta encontrar una página conocida.

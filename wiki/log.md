---
title: "Log de Operaciones"
type: log
created: 2026-04-09
---

# Log de Operaciones — Wiki de Ingeniería

Registro cronológico append-only de operaciones sobre la wiki.

---

## [2026-04-10] ingest | NCh2369-2025.pdf — Ingesta del estándar completo

**Operación:** Ingesta del PDF oficial `raw/pdf/NCh2369-2025.pdf` (248 páginas, edición 2025).

**Páginas creadas:**
- `formulas/NCh2369-Ch4-LoadCombinations.md` — Cap. 4: combinaciones ASD/LRFD, simultaneidad 100%+30%+30%, categorías I-IV

**Páginas actualizadas con correcciones CRÍTICAS:**
- `standards/NCh2369-2025.md` — **CORRECCIONES:** estructura de capítulos reescrita (§4=General, §5=Análisis, no §5/6/7); tabla de zonas sísmicas corregida (A0 ≠ Ar: Ar=1.4·A0); Tabla 6 parámetros de suelo completamente reemplazada (valores erróneos en versión anterior); agregada Tabla 7 resumen R/ξ y parámetros de estanques
- `formulas/NCh2369-Spectrum.md` — **CORRECCIONES:** advertencia A0 vs Ar; Tabla 6 parámetros de suelo correctos (todos los valores cambiaron); fórmulas de espectro horizontal y vertical verificadas contra §5.4.2; factor η recalculado correctamente; ejemplo actualizado con datos correctos

**Archivos de extracción intermedios:**
- `raw/references/NCh2369_extract/Cap4_General.txt` — Cap. 4 (9 pág.)
- `raw/references/NCh2369_extract/Cap5_Analisis.txt` — Cap. 5 (50 pág.) — espectros, AEE, AME, Tablas 2-7
- `raw/references/NCh2369_extract/Cap7_Equipos.txt` — Cap. 7 (5 pág.) — Tabla 8: Rp
- `raw/references/NCh2369_extract/Cap11_Estanques.txt` — Cap. 11 (18 pág.) — estanques cilíndricos, anclajes
- `raw/references/NCh2369_extract/AnnexB_Cargas.txt` — Anexo B (4 pág.) — filosofía ASD/LRFD

---

## [2026-04-10] ingest | ACI 318-25_SI.pdf — Ingesta del estándar completo

**Operación:** Ingesta del PDF oficial `raw/pdf/ACI 318-25_SI.pdf` (702 páginas, primera edición junio 2025).

**Páginas creadas:**
- `formulas/ACI-318-Ch9-Beams.md` — Cap. 9: profundidades mínimas, flexión, cortante Vc/Vs, torsión, límites de refuerzo
- `formulas/ACI-318-Ch21-PhiFactors.md` — Cap. 21: tabla completa de factores φ, zonas de transición, interpolación
- `formulas/ACI-318-Ch22-SectionalStrength.md` — Cap. 22: bloque equivalente β₁, Po, Vc/Vs, λs, punzonamiento

**Páginas actualizadas:**
- `formulas/ACI-318-Ch10-Columns.md` — Agregada fórmula Po = 0.85f'c(Ag−Ast)+fy·Ast (§22.4.2.2); Pn,max expresado en función de Po; cross-refs nuevas páginas
- `concepts/Load-Combinations.md` — **CORRECCIÓN CRÍTICA:** factores de nieve corregidos según Tabla 5.3.1 de ACI 318-25/ASCE 7-22 (nieve a nivel de resistencia): S en Eq.5.3.1b=0.3 (antes 0.5), S en Eq.5.3.1c=1.0 (antes 1.6), S en Eq.5.3.1d=0.3 (antes 0.5), S en Eq.5.3.1e=0.15 (antes 0.2)
- `materials/Concrete-Properties.md` — Agregada tabla de β₁ vs f'c (§22.2.2.4.3)
- `standards/ACI-318-25.md` — Fórmulas relacionadas expandidas con los 3 nuevos capítulos documentados

**Archivos de extracción intermedios** (para referencia, no forman parte de la wiki):
- `raw/references/ACI318_extract/Cap5.txt` — Cap. 5 Cargas
- `raw/references/ACI318_extract/Cap9.txt` — Cap. 9 Vigas
- `raw/references/ACI318_extract/Cap10.txt` — Cap. 10 Columnas
- `raw/references/ACI318_extract/Cap18.txt` — Cap. 18 Sismoresistencia
- `raw/references/ACI318_extract/Cap19.txt` — Cap. 19 Materiales
- `raw/references/ACI318_extract/Cap21.txt` — Cap. 21 Factores φ
- `raw/references/ACI318_extract/Cap22.txt` — Cap. 22 Resistencia seccional

**Fuente:** `raw/pdf/ACI 318-25_SI.pdf` — ACI CODE-318-25, ISBN 978-1-64195-321-4, primera impresión junio 2025.

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

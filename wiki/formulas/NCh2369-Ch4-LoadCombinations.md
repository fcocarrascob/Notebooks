---
title: "NCh2369-2025 — Combinaciones de Carga y Categorías de Importancia"
type: formula
standard_ref: "NCh2369-2025"
chapter: "4"
section: "§4.3, §4.5"
variables: [I, D, L, SO, SA, E]
units: "SI"
tags: [sismo, combinaciones, NCh2369, chile, importancia, ASD, LRFD]
related:
  - ../standards/NCh2369-2025.md
  - ../formulas/NCh2369-Spectrum.md
  - ../concepts/Load-Combinations.md
created: 2026-04-10
updated: 2026-04-10
---

# NCh2369-2025 — Combinaciones de Carga y Categorías de Importancia

## Fuente
[NCh2369-2025](../standards/NCh2369-2025.md): Capítulo 4 (Disposiciones generales), §4.3 (Categorías), §4.5 (Combinaciones)

---

## Categorías de Importancia (§4.3.2, Tabla 1)

| Categoría | $I$ | Descripción | Ejemplos |
|-----------|-----|-------------|---------|
| I | 0.80 | Estructuras menores o provisionales | Estructuras temporales, almacenamiento de bajo riesgo |
| II | 1.00 | Estructuras normales | Uso general industrial estándar |
| III | 1.20 | Instalaciones críticas, alta consecuencia | Instalaciones con fluidos peligrosos, servicios públicos |
| IV | ≥ 1.20 | Esenciales, alto riesgo, difícil reemplazo | Instalaciones nucleares, plantas estratégicas |

**Nota:** Para categorías III/IV se debe evaluar si se requieren valores $I$ mayores según las consecuencias específicas del proyecto.

---

## Combinaciones de Carga ASD (§4.5.1)

**Cargas parciales:**

$$D + 0.75\,\alpha L + 0.75\,SO + 0.75\,SA + 0.70\,E$$

$$D + 0.75\,SA + 0.70\,E$$

**Variables:**
| Símbolo | Descripción |
|---------|------------|
| $D$ | Carga permanente (peso propio, peso de equipos en operación) |
| $L$ | Sobrecarga de uso |
| $SO$ | Sobrecarga de operación |
| $SA$ | Sobrecarga ambiental (nieve, granizo, hielo) |
| $E$ | Acción sísmica (incluye efectos en las 3 direcciones según §4.5.2) |
| $\alpha$ | Fracción de sobrecarga de uso (ver §4.5.1) |

---

## Combinaciones de Carga LRFD (§4.5.1)

$$1.2\,D + \alpha L + SO + SA + E$$

$$0.9\,D + SA + E$$

**Nota:** La combinación $0.9D$ aplica cuando la carga sísmica puede aliviar la carga gravitacional (verificación de volcamiento, tracción en anclajes).

---

## Simultaneidad Direccional (§4.5.2)

La acción sísmica $E$ en las combinaciones anteriores se determina con cualquiera de las siguientes reglas:

$$E = \pm 1.0\,E_x \pm 0.3\,E_y \pm 0.3\,E_z$$

$$E = \pm 0.3\,E_x \pm 1.0\,E_y \pm 0.3\,E_z$$

$$E = \pm 0.3\,E_x \pm 0.3\,E_y \pm 1.0\,E_z$$

donde $E_x$, $E_y$ son las dos componentes horizontales de análisis, y $E_z$ es la componente vertical (calculada según §5.7).

**Condición de aplicación:** Los signos (±) se deben combinar para maximizar el efecto en cada elemento particular de diseño.

---

## Nivel de Amenaza Sísmica

La acción sísmica de diseño corresponde a una probabilidad de excedencia del **10% en 50 años** (período de retorno ≈ 475 años), consistente con NCh433 y ASCE 7.

---

## Masa Sísmica para Análisis (§5.1.2)

La masa sísmica incluye cargas permanentes más una fracción de sobrecargas:

| Uso | Fracción de sobrecarga a incluir |
|-----|----------------------------------|
| Bodegas, archivos, acopios baja rotación | 50% |
| Zonas de uso normal, plataformas operación | 25% |
| Sin estimación específica | ≥ 25% |

---

## Compatibilidad con Otras Normas

- **ASD vs LRFD:** NCh2369 reconoce ambos métodos. El factor de equivalencia ASD↔LRFD a nivel de resistencia es ≈ 1.5 (Anexo B).
- **API 650 (estanques):** Las combinaciones de §4.5 prevalecen sobre criterios de API 650 cuando sean más exigentes.
- **Factores $\alpha$ (sobrecarga de uso):** Se coordinan con NCh433 y ASCE 7.

## Ejemplo de Uso

Estanque de almacenamiento de hidrocarburos (Categoría III, $I = 1.20$) en Zona 2, Suelo B, sin anclaje dúctil ($R = 2.5$).

Combinación de diseño LRFD crítica para verificar manto en compresión:

$$1.2\,D + E_x + 0.3\,E_z$$

donde $E_x$ y $E_z$ se calculan con el espectro de diseño de §5.4 usando $I = 1.20$, $R^* = 2.5$.

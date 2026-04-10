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

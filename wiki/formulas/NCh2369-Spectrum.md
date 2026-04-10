---
title: "NCh2369-2025 — Espectro Sísmico de Diseño"
type: formula
standard_ref: "NCh2369-2025"
chapter: "5"
section: "§5.4"
variables: [S_aH, S_aV, A_0, A_r, S, T_0, r, p, q, eta, xi, I, R, R_star, T_H, T_V]
units: "SI"
tags: [sismo, espectro, NCh2369, chile, amortiguamiento]
related:
  - ../standards/NCh2369-2025.md
  - ../concepts/Seismic-Design-Principles.md
  - ../applications/Tank-Seismic-Analysis.md
created: 2026-04-09
updated: 2026-04-10
---

# NCh2369-2025 — Espectro Sísmico de Diseño

## Fuente
[NCh2369-2025](../standards/NCh2369-2025.md): Capítulo 5 (Análisis sísmico), §5.4 (Espectros normativos)

---

## ⚠️ Aclaración: A₀ vs Aᵣ

El espectro de referencia usa $A_r$ (aceleración de referencia), **no** $A_0$ (aceleración efectiva máxima).

$$A_r = 1.4 \cdot A_0$$

| Zona | $A_0$ [g] | $A_r$ [g] |
|------|-----------|-----------|
| 1 | 0.20 | 0.28 |
| 2 | 0.30 | 0.42 |
| 3 | 0.40 | 0.56 |

---

## Espectro de Referencia Horizontal (ec. 3)

$$S_{aH}(T_H) = A_r \cdot S \cdot \frac{1 + r\left(\dfrac{T_H}{T_0^*}\right)^p}{1 + \left(\dfrac{T_H}{T_0^*}\right)^q}$$

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $S_{aH}$ | Espectro de referencia horizontal (ξ = 5%) | g |
| $A_r$ | Aceleración máxima de referencia del suelo (Tabla 3) | g |
| $S$ | Amplificación por tipo de suelo | — |
| $T_H$ | Período del modo horizontal considerado | s |
| $T_0^*$ | Período de esquina del suelo | s |
| $r$ | Factor de amplitud del plateau espectral | — |
| $p, q$ | Exponentes de forma espectral | — |

---

## Espectro de Referencia Vertical (ec. 4)

$$S_{aV}(T_V) = 0.7\, A_r \cdot S \cdot \frac{1 + r\left(\dfrac{1.7\, T_V}{T_0^*}\right)^p}{1 + \left(\dfrac{1.7\, T_V}{T_0^*}\right)^q}$$

El factor 0.7 reduce la amplitud y el factor 1.7 comprimen el eje temporal (el espectro alcanza su plateau a $T_V = T_0^*/1.7$).

---

## Espectro de Diseño (ec. 1a)

$$S_a(T_H) = \frac{I \cdot \eta \cdot S_{aH}(T_H)}{R^*}$$

donde el factor de corrección de amortiguamiento es:

$$\eta = \left(\frac{0.05}{\xi}\right)^{0.4}$$

Válido para $0.02 \leq \xi \leq 0.05$. Para ξ > 0.05 en dirección vertical requiere justificación racional.

| Caso | $\xi$ | $\eta$ |
|------|-------|--------|
| Estructura convencional de acero | 2% | 1.32 |
| Estructura HA (sin estanque) | 5% | 1.00 |
| Estanque — modo impulsivo | 2% | 1.32 |
| Estanque — modo convectivo | 0.5% | 2.02 |

**Variables adicionales:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $I$ | Coeficiente de importancia (Tabla 1) | — |
| $R^*$ | Factor de reducción efectivo (§5.4.1 ec. 1b) | — |

---

## Factor R* Efectivo (§5.4.1, ec. 1b)

Para $T^* \geq C_r \cdot T_1^*$:

$$R^* = R$$

Para $T^* < C_r \cdot T_1^*$:

$$R^* = 1 + (R-1) \cdot \frac{T^*}{C_r \cdot T_1^*}$$

donde $C_r = 0.16\, R$, $T^*$ es el período del modo con mayor masa traslacional equivalente, y $T_1^*$ es el período fundamental.

**Condiciones de aplicación:** $R^*$ se calcula independientemente para cada modelo (estructura, subestructura, equipo o componente).

---

## Parámetros de Suelo (Tabla 6)

| Suelo | Descripción | $S$ | $r$ | $T_0^*$ [s] | $p$ | $q$ |
|-------|------------|-----|-----|------------|-----|-----|
| A | Roca, suelo cementado | 0.90 | 4.50 | 0.15 | 1.85 | 3.00 |
| B | Roca blanda / suelo muy denso | 1.00 | 4.50 | 0.30 | 1.60 | 3.00 |
| C | Suelo denso o firme | 1.05 | 4.50 | 0.40 | 1.50 | 3.00 |
| D | Suelo medianamente denso | 1.00 | 3.50 | 0.60 | 1.00 | 2.50 |
| E | Suelo de compacidad media | 1.00 | 3.00 | 1.20 | 1.00 | 2.70 |

> **NOTA:** Estos valores difieren significativamente de los publicados informalmente antes de la edición 2025. Usar siempre Tabla 6 de la norma vigente.

---

## Corrección de Amortiguamiento para Desplazamientos

Para estimación de desplazamientos se usa el espectro de referencia (sin R*), corregido por amortiguamiento y ponderado con $I$.

---

## Corte Basal Mínimo (§5.12)

$$Q_{0,\min} = 0.25 \cdot \frac{I \cdot A_r \cdot S}{g} \cdot P$$

## Corte Basal Máximo (§5.13)

$$Q_{0,\max} = \frac{0.42 \cdot I \cdot A_r \cdot S}{g\,(0.1 + R^* - 0.1\,\xi)} \cdot P$$

---

## Acción Sísmica Vertical Simplificada (§5.7.1)

$$F_V = \pm C_V \cdot P$$

donde el coeficiente sísmico vertical $C_V$ es:

| Tipo de suelo | $C_V$ |
|--------------|-------|
| A, B, C | $1.2\, I \cdot A_r \cdot S / g$ |
| D | $1.1\, I \cdot A_r \cdot S / g$ |
| E | $I \cdot A_r \cdot S / g$ |

---

## Ejemplo de Uso

Estanque en **Zona 3, Suelo C**, modo impulsivo ($\xi = 2\%$, $R = 4$, $I = 1.2$):

$$A_0 = 0.40g \Rightarrow A_r = 0.56g$$

$$\eta = (0.05/0.02)^{0.4} = 1.32$$

Parámetros suelo C: $S = 1.05$, $T_0^* = 0.40$ s, $r = 4.50$, $p = 1.50$, $q = 3.00$

Para $T_H = 0.15$ s (modo impulsivo típico):

$$S_{aH} = 0.56 \times 1.05 \times \frac{1 + 4.50 \times (0.15/0.40)^{1.50}}{1 + (0.15/0.40)^{3.00}} = 0.588 \times \frac{1 + 1.069}{1 + 0.053} = 0.588 \times 1.966 = 1.156g$$

Asumiendo $R^* = R = 4$ (período suficientemente largo):

$$S_a = \frac{1.2 \times 1.32 \times 1.156}{4} = 0.458g$$

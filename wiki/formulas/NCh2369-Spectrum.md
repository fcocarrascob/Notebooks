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

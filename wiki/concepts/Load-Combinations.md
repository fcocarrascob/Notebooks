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

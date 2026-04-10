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

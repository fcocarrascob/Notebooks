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
updated: 2026-04-10
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

## Factor del Bloque de Compresión $\beta_1$ (ACI 318-25 §22.2.2.4.3)

Usado para calcular la profundidad del bloque equivalente $a = \beta_1 c$:

| $f'_c$ (MPa) | $\beta_1$ | Ecuación |
|-------------|-----------|----------|
| $17 \leq f'_c \leq 28$ | 0.85 | constante |
| $28 < f'_c < 55$ | $0.85 - \dfrac{0.05(f'_c-28)}{7}$ | lineal |
| $f'_c \geq 55$ | 0.65 | constante |

| $f'_c$ [MPa] | $\beta_1$ |
|-------------|----------|
| 20 / 25 / 28 | 0.85 |
| 30 | 0.836 |
| 35 | 0.800 |
| 40 | 0.764 |
| 45 | 0.729 |
| 50 | 0.693 |
| ≥55 | 0.65 |

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

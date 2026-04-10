---
title: "Workflow: Diseño de Columnas de Hormigón"
type: application
domain: "hormigón"
standards_used: ["ACI-318-25"]
tags: [columnas, diseño, workflow, hormigón]
related:
  - ../standards/ACI-318-25.md
  - ../formulas/ACI-318-Ch10-Columns.md
  - ../materials/Concrete-Properties.md
  - ../materials/Steel-Reinforcing.md
  - ../concepts/Load-Combinations.md
created: 2026-04-09
updated: 2026-04-09
---

# Workflow: Diseño de Columnas de Hormigón Armado

## Objetivo
Diseñar una columna de hormigón armado sometida a carga axial y momentos flectores según ACI 318-25.

## Estándares Aplicables
- [ACI 318-25](../standards/ACI-318-25.md): Cap. 10 (Columnas), Cap. 22 §22.4 (Resistencia combinada), Cap. 6 §6.6/6.7 (Esbeltez)

## Datos de Entrada

| Parámetro | Símbolo | Descripción |
|-----------|---------|------------|
| Cargas factorizadas | $P_u$, $M_{ux}$, $M_{uy}$ | De combinaciones de carga §5.3 |
| Dimensiones | $b$, $h$ | Sección transversal propuesta |
| Longitud no arriostrada | $\ell_u$ | Altura libre entre restricciones laterales |
| Condiciones de apoyo | $k$ | Factor de longitud efectiva |
| Resistencia del concreto | $f'_c$ | Grado seleccionado |
| Fluencia del acero | $f_y$ | Grado 60 (420 MPa) estándar |

## Procedimiento

### 1. Predimensionamiento
- Estimar sección por carga axial: $A_g \approx P_u / (0.45 f'_c)$ (primera aproximación)
- Relación b/h: usar 1:1 para cargas centradas, hasta 1:3 para momentos dominantes

### 2. Verificar Esbeltez (§6.2.5)
- Calcular $k\ell_u / r$ donde $r \approx 0.3h$ (rectangular)
- Si $k\ell_u / r \leq 22$ (arriostrada): se puede ignorar efecto de esbeltez
- Si $k\ell_u / r > 22$: amplificar momentos por efectos de segundo orden (§6.6.4)

### 3. Determinación de Refuerzo
- Seleccionar cuantía: $\rho_g$ entre 1% y 4% (recomendado)
- Calcular $A_{st} = \rho_g \times A_g$
- Seleccionar barras y distribución perimetral

### 4. Diagrama de Interacción P-M
- Construir diagrama de interacción $\phi P_n$ vs $\phi M_n$
- Verificar que el punto ($P_u$, $M_u$) quede dentro del diagrama
- Puntos clave: compresión pura, punto balanceado, flexión pura

### 5. Verificación de Diseño

| Verificación | Criterio | Referencia |
|-------------|----------|-----------|
| Capacidad axial-flexural | ($P_u$, $M_u$) dentro del diagrama φPn-φMn | §10.5, §22.4 |
| Cuantía de refuerzo | $0.01 \leq \rho_g \leq 0.08$ | §10.6 |
| Espaciado de estribos | $s \leq \min(16d_b, 48d_{estribo}, b)$ | §10.7.6.2 |
| Recubrimiento libre | $c \geq 40$ mm (exposición normal) | Tabla 20.6.1.3 |
| Número mínimo de barras | 4 (estribos rectangulares) | §10.7 |

### 6. Diseño de Refuerzo Transversal
- Estribos cerrados Ø10 o Ø12 mínimo
- Espaciado según §10.7.6.2
- En zonas sísmicas: confinar con estribos adicionales (Cap. 18)

### 7. Biaxialidad (si aplica)
- Verificar con método de Bresler (punto de carga recíproco):

$$\frac{1}{P_{ni}} = \frac{1}{P_{nx}} + \frac{1}{P_{ny}} - \frac{1}{P_0}$$

## Verificaciones Finales
- [ ] ($P_u$, $M_u$) dentro del diagrama de interacción
- [ ] Cuantía entre 1% y 8%
- [ ] Esbeltez verificada o momentos amplificados
- [ ] Estribos con espaciado correcto
- [ ] Recubrimiento adecuado para clase de exposición
- [ ] Compatibilidad con conexiones (vigas, losas)

## Fórmulas Utilizadas
- [ACI 318 Cap. 10 — Columnas](../formulas/ACI-318-Ch10-Columns.md)

## Referencias
- [Propiedades del Concreto](../materials/Concrete-Properties.md)
- [Acero de Refuerzo](../materials/Steel-Reinforcing.md)
- [Combinaciones de Carga](../concepts/Load-Combinations.md)

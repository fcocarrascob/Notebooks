---
title: "ACI 318-25 Cap. 10 — Diseño de Columnas"
type: formula
standard_ref: "ACI-318-25"
chapter: "10"
section: "10.3–10.7, 22.4, 22.5, 25.7"
variables: [P_u, M_u, V_u, phi, A_s, A_g, A_st, A_v, f_c, f_y, f_yt, rho_g, rho_w, a, d, lambda, N_u]
units: "SI"
tags: [columnas, flexo-compresión, corte, hormigón, ACI]
related:
  - ../standards/ACI-318-25.md
  - ../formulas/ACI-318-Ch22-SectionalStrength.md
  - ../formulas/ACI-318-Ch21-PhiFactors.md
  - ../materials/Concrete-Properties.md
  - ../materials/Steel-Reinforcing.md
  - ../applications/Column-Design-Workflow.md
created: 2026-04-09
updated: 2026-05-11
---

# ACI 318-25 Cap. 10 — Diseño de Columnas

## Fuente
[ACI CODE-318-25](../standards/ACI-318-25.md): Capítulo 10 (Columnas), Capítulo 22 §22.4 (Resistencia axial combinada), §22.5 (Resistencia al corte), Capítulo 25 §25.7 (Refuerzo transversal)

---

## Fórmulas

### Resistencia Axial Nominal ($P_o$) (§22.4.2.2)

Para miembros no preesforzados:

$$P_o = 0.85\,f'_c\,(A_g - A_{st}) + f_y\,A_{st}$$

> $f_y$ limitado a máx. 550 MPa para este cálculo.

### Resistencia Axial Nominal Máxima

Para columnas con estribos (§22.4.2, Tabla 22.4.2.1):

$$P_{n,max} = 0.80\,P_o$$

Para columnas con espiral (§22.4.2, Tabla 22.4.2.1):

$$P_{n,max} = 0.85\,P_o$$

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $P_{n,max}$ | Resistencia axial nominal máxima | N |
| $f'_c$ | Resistencia a compresión del concreto | MPa |
| $A_g$ | Área bruta de la sección transversal | mm² |
| $A_{st}$ | Área total de acero longitudinal | mm² |
| $f_y$ | Fluencia del acero de refuerzo | MPa |

### Resistencia a Flexión Nominal

$$M_n = A_s f_y \left( d - \frac{a}{2} \right)$$

$$a = \frac{A_s f_y}{0.85 f'_c b}$$

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $M_n$ | Momento nominal | N·mm |
| $A_s$ | Área de acero en tracción | mm² |
| $d$ | Altura efectiva (distancia al centroide del acero en tracción) | mm |
| $a$ | Profundidad del bloque de compresión equivalente | mm |
| $b$ | Ancho de la sección | mm |

### Verificación de Diseño

$$\phi P_n \geq P_u \quad \text{y} \quad \phi M_n \geq M_u$$

**Factores φ (§21.2):**
| Condición | $\phi$ |
|-----------|--------|
| Compresión controlada (estribos) | 0.65 |
| Compresión controlada (espiral) | 0.75 |
| Tracción controlada | 0.90 |
| Transición | Interpolar linealmente |

### Límites de Refuerzo (§10.6)

$$0.01 A_g \leq A_{st} \leq 0.08 A_g$$

Es decir:
$$1\% \leq \rho_g \leq 8\%$$

> **Práctica recomendada:** mantener $\rho_g$ entre 1% y 4% para facilitar la construcción y evitar congestión de acero.

### Refuerzo Mínimo por Flexión (§10.3.1)

$$A_{s,min} = \frac{0.25 \sqrt{f'_c}}{f_y} b_w d \geq \frac{1.4}{f_y} b_w d$$

### Esbeltez y Efectos de Segundo Orden (§6.6.4, §6.7)

Para columnas esbeltas ($k\ell_u / r > 22$ para arriostradas, $k\ell_u / r > 22$ para no arriostradas):

**Método de amplificación de momentos (§6.6.4):**

$$M_c = \delta_{ns} M_2$$

$$\delta_{ns} = \frac{C_m}{1 - P_u / (0.75 P_c)} \geq 1.0$$

$$P_c = \frac{\pi^2 (EI)_{eff}}{(k \ell_u)^2}$$

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $k$ | Factor de longitud efectiva | — |
| $\ell_u$ | Longitud no arriostrada | mm |
| $r$ | Radio de giro ($\approx 0.3h$ para rectangular) | mm |
| $C_m$ | Factor de equivalencia de momentos | — |
| $P_c$ | Carga crítica de Euler | N |
| $(EI)_{eff}$ | Rigidez efectiva reducida | N·mm² |

---

---

## Resistencia al Corte (§10.5.3, §22.5)

### Ecuación General (§22.5.1.1)

$$V_n = V_c + V_s$$

$$\phi V_n \geq V_u$$

### Dimensiones Efectivas para Columnas (§22.5.2.1)

El Código permite usar simplificaciones para el cálculo de $V_c$ y $V_s$:

| Sección | $d$ | $b_w$ |
|---------|-----|-------|
| Rectangular | $0.80\,h$ | ancho real $b$ |
| Circular sólida | $0.80\,D$ | $D$ (diámetro) |
| Circular hueca | $0.80\,D$ | $2\,t_w$ (doble espesor de pared) |

### Resistencia al Corte del Hormigón $V_c$ (§22.5.5, Tabla 22.5.5.1)

Para miembros no preesforzados con carga axial $N_u$:

**Con $A_v \geq A_{v,min}$** — usar cualquiera de las dos ecuaciones:

$$V_c = \left[0.17\,\lambda\sqrt{f'_c} + \frac{N_u}{6A_g}\right] b_w d \quad \text{(a) — simplificada}$$

$$V_c = \left[0.66\,\lambda\,(\rho_w)^{1/3}\sqrt{f'_c} + \frac{N_u}{6A_g}\right] b_w d \quad \text{(b) — con refuerzo longitudinal}$$

**Con $A_v < A_{v,min}$** — se debe usar (incluye factor de tamaño $\lambda_s$):

$$V_c = \left[0.66\,\lambda_s\,\lambda\,(\rho_w)^{1/3}\sqrt{f'_c} + \frac{N_u}{6A_g}\right] b_w d \quad \text{(c)}$$

**Factor de tamaño** $\lambda_s$ (§22.5.5.1.3):

$$\lambda_s = \sqrt{\frac{2}{1 + d/250}} \leq 1.0$$

**Límites de $V_c$ (§22.5.5.1.1):**

$$0.083\,\lambda\sqrt{f'_c}\,b_w d \leq V_c \leq 0.42\,\lambda\sqrt{f'_c}\,b_w d$$

> $V_c$ no puede ser menor que cero. El límite inferior no aplica si hay tracción axial neta ni en zonas sísmicas según §18.

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $V_c$ | Resistencia al corte del hormigón | N |
| $\lambda$ | Factor de modificación por peso unitario del hormigón (1.0 para normal) | — |
| $\lambda_s$ | Factor de efecto de tamaño | — |
| $\rho_w$ | Cuantía de refuerzo longitudinal en tracción $= A_s/(b_w d)$ | — |
| $N_u$ | Carga axial factorizada (+ compresión, − tracción) | N |
| $A_g$ | Área bruta de la sección | mm² |
| $f'_c$ | Resistencia a compresión del hormigón | MPa |
| $b_w$ | Ancho del alma (o diámetro para circular) | mm |
| $d$ | Altura efectiva | mm |

> **Nota:** El término $N_u/(6A_g)$ no puede superar $0.05\,f'_c$ (§22.5.5.1.2). La carga axial de compresión aumenta $V_c$; la tracción axial la disminuye o anula.

### Resistencia al Corte del Refuerzo $V_s$ (§22.5.8.5)

Para estribos, estribas o espirales perpendiculares al eje del miembro:

$$V_s = \frac{A_v f_{yt} d}{s}$$

Área requerida de refuerzo transversal:

$$\frac{A_v}{s} = \frac{V_u/\phi - V_c}{f_{yt}\,d}$$

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $V_s$ | Resistencia al corte del refuerzo transversal | N |
| $A_v$ | Área efectiva del refuerzo transversal por espaciado $s$ | mm² |
| $f_{yt}$ | Tensión de fluencia del refuerzo transversal (máx. 420 MPa para cálculo de $V_s$) | MPa |
| $s$ | Espaciado del refuerzo transversal | mm |

> Para estribo circular o espiral: $A_v = 2\,A_{barra}$ (§22.5.8.5.6).

### Límite Dimensional de la Sección (§22.5.1.2)

Para evitar falla por compresión diagonal:

$$V_u \leq \phi\left(V_c + 0.66\sqrt{f'_c}\,b_w d\right)$$

### Corte Biaxial (§22.5.1.10–11)

Si ambas componentes son significativas, verificar interacción:

$$\frac{V_{u,x}}{\phi V_{n,x}} + \frac{V_{u,y}}{\phi V_{n,y}} \leq 1.5$$

> Se puede ignorar la interacción si $V_{u,x}/(\phi V_{n,x}) \leq 0.5$ **o** $V_{u,y}/(\phi V_{n,y}) \leq 0.5$.

---

## Refuerzo Mínimo al Corte (§10.6.2)

Se requiere refuerzo mínimo de corte en toda región donde $V_u > 0.5\,\phi V_c$ (§10.6.2.1).

Cuando se requiere refuerzo, $A_{v,min}$ es el mayor de:

$$A_{v,min} = \frac{0.062\sqrt{f'_c}\,b_w\,s}{f_{yt}} \quad \text{(a)}$$

$$A_{v,min} = \frac{0.35\,b_w\,s}{f_{yt}} \quad \text{(b)}$$

---

## Detallado del Refuerzo Transversal

### Forma y Ubicación (§10.7.6.5)

El refuerzo de corte puede materializarse con **estribas (ties), aros (hoops) o espirales** (§10.7.6.5.1).

### Espaciado Máximo al Corte — Tabla 10.7.6.5.2

| Condición de $V_s$ | Columna no preesforzada — $s_{max}$ |
|--------------------|-------------------------------------|
| $V_s \leq 0.33\sqrt{f'_c}\,b_w d$ | menor de $d/2$ y $600$ mm |
| $V_s > 0.33\sqrt{f'_c}\,b_w d$ | menor de $d/4$ y $300$ mm |

> Cuando el corte es elevado ($V_s > 0.33\sqrt{f'_c}\,b_w d$), el espaciado se reduce a la mitad.

### Espaciado de Estribas — §25.7.2.1

El espaciado centro a centro no debe exceder el **menor** de:
- $16\,d_{b,long}$ (16 veces el diámetro del refuerzo longitudinal)
- $48\,d_{b,estribo}$ (48 veces el diámetro de la estriba)
- Dimensión menor de la sección transversal

Diámetro mínimo de estriba (§25.7.2.2):
- No. 10 para barras longitudinales ≤ No. 32
- No. 13 para barras longitudinales ≥ No. 36 o barras agrupadas

Arreglo de estribas (§25.7.2.3): toda barra de esquina y una barra intermedia de por medio deben tener soporte lateral (ángulo de gancho ≤ 135°); ninguna barra sin soporte puede estar a más de 150 mm de distancia libre de una barra soportada.

### Espiral Mínima — §25.7.3.3

$$\rho_s \geq 0.45\left(\frac{A_g}{A_{ch}} - 1\right)\frac{f'_c}{f_{yt}}$$

Claro entre espiras: 25 mm ≤ claro ≤ 75 mm. Diámetro mínimo de barra: 10 mm.

**Variables:**
| Símbolo | Descripción | Unidad |
|---------|------------|--------|
| $\rho_s$ | Cuantía volumétrica de espiral | — |
| $A_{ch}$ | Área del núcleo de la columna (medida a ejes exteriores de la espiral) | mm² |
| $f_{yt}$ | Fluencia del refuerzo transversal (máx. 690 MPa para espiral) | MPa |

---

## Ejemplo de Uso

Sección rectangular $b = 300$ mm, $h = 500$ mm, $f'_c = 28$ MPa, $f_y = 420$ MPa:

$$A_g = 300 \times 500 = 150{,}000 \text{ mm}^2$$

$$A_{st,min} = 0.01 \times 150{,}000 = 1{,}500 \text{ mm}^2$$

$$A_{st,max} = 0.08 \times 150{,}000 = 12{,}000 \text{ mm}^2$$

Usando $\rho_g = 2\%$: $A_{st} = 3{,}000$ mm² → 6 barras Ø25 ($A_s = 6 \times 491 = 2{,}946$ mm²)

$$P_{n,max} = 0.80 [0.85 \times 28 \times (150{,}000 - 2{,}946) + 420 \times 2{,}946] = 3{,}790 \text{ kN}$$

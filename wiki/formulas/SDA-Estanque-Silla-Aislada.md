---
title: "Estanque de Acero — Silla Aislada (API 650 + NCh2369)"
type: formula
standard_ref: "API-650"
chapter: "Anexo E + 3.5 + 3.6 + 3.9"
section: "E3–E6"
variables: [D, H, G, Fy, Ao, I, R, C1, C2, W1, W2, X1, X2, Tb, fa, Fa]
units: "Toneladas, metros, cm"
tags: [estanques, acero, API650, NCh2369, sismo, anclaje, silla-aislada]
related:
  - ../standards/API-650.md
  - ../standards/NCh2369-2025.md
  - ../formulas/API-650-Seismic.md
created: 2026-04-10
updated: 2026-04-10
---

# Estanque Anclado con Sillas Aisladas — API 650 + NCh 2369

> Fuente: `SDA - Estanques de Acero - Ejemplos 01 - A.xlsm`, hoja **Estanque Silla Aislada**  
> Ejemplo: Estanque ø10 m × 10 m de altura de manto, Zona III, Suelo III, Categoría C2

---

## Nomenclatura

| Símbolo | Descripción | Unidad |
|---------|-------------|--------|
| `D` | Diámetro interior del estanque | m |
| `H` | Altura del líquido | m |
| `Hs` | Altura del manto (H + r) | m |
| `r` | Revancha | m |
| `G` | Gravedad específica del líquido | — |
| `T°` | Temperatura de trabajo | °C |
| `Fy` | Tensión de fluencia del acero del manto | t/cm² |
| `Es` | Módulo de elasticidad del acero | t/cm² |
| `Sd` | Tensión admisible por tracción (diseño) | t/cm² |
| `St` | Tensión admisible por prueba hidrostática | t/cm² |
| `CAT`, `CAM`, `CAF` | Corrosión techo, manto, fondo | mm |
| `Ao` | Aceleración efectiva según zona sísmica | g |
| `I` | Factor de importancia (Categoría) | — |
| `R` | Factor de modificación de respuesta | — |
| `C1`, `C2` | Coeficiente sísmico impulsivo / convectivo | — |

---

## 1. Datos de Entrada

### 1.1 Dimensiones Generales

| Parámetro | Símbolo | Valor ejemplo | Fórmula Excel |
|-----------|---------|--------------|---------------|
| Diámetro | `D` | 10 m | dato |
| Altura líquido | `H` | 9 m | dato |
| Capacidad | — | 706.9 m³ | `=D²/4·π·H` |
| Revancha | `r` | 1 m | dato |
| Altura manto | `Hs` | 10 m | `=H + r` |
| Gravedad específica | `G` | 1.0 | dato |
| Temperatura | `T°` | 40 °C | dato |

**Fórmula capacidad:**
$$V = \frac{\pi \cdot D^2}{4} \cdot H$$

### 1.2 Espesores de Corrosión

| Zona | Símbolo | Valor | Categorías disponibles |
|------|---------|-------|----------------------|
| Techo | `CAT` | 2 mm | C1=1.2, C2=1.0, C3=0.8 mm |
| Manto | `CAM` | 2 mm | — |
| Fondo | `CAF` | 2 mm | — |

### 1.3 Parámetros Sísmicos (NCh 2369-2003)

| Parámetro | Símbolo | Valor ejemplo | Fórmula Excel |
|-----------|---------|--------------|---------------|
| Zona sísmica | — | III | dato (I/II/III) |
| Aceleración | `Ao` | 0.4 g | `=LOOKUP(zona, tabla_zonas)` |
| Categoría | — | C2 | dato (C1/C2/C3) |
| Factor importancia | `I` | 1.0 | `=LOOKUP(cat, tabla_cat)` |
| Factor respuesta | `R` | 4 | dato |
| Amortiguamiento | `ξ` | 2% | dato |
| Tipo de suelo | — | III | dato (I/II/III/IV) |
| Período característico | `T'` | 0.62 s | `=LOOKUP(suelo, tabla_suelo)` |
| Exponente espectro | `n` | 1.8 | `=LOOKUP(suelo, tabla_suelo)` |

**Tablas NCh 2369 integradas en la hoja:**

| Zona | Nº | Ao (g) | Cmax |
|------|----|--------|------|
| I | 1 | 0.20 | 0.16 |
| II | 2 | 0.30 | 0.24 |
| III | 3 | 0.40 | 0.32 |

| Suelo | Nº | T' (s) | n |
|-------|----|--------|---|
| I | 1 | 0.20 | 1.00 |
| II | 2 | 0.35 | 1.33 |
| III | 3 | 0.62 | 1.80 |
| IV | 4 | 1.35 | 1.80 |

**Coeficiente sísmico impulsivo:**
$$C_1 = \text{LOOKUP}(\text{Zona}, M_{tabla}) = C_{max} = 0.32 \quad (\text{Zona III})$$

### 1.4 Sobrecargas

| Parámetro | Fórmula | Valor ejemplo |
|-----------|---------|--------------|
| SC mínima techo (API 650-3.10.2.1, 122 kg/m²) | `=π·D²/4·0.122` | 9.58 t |
| SC techo especificada | dato | 1.0 t |
| SC techo de cálculo | `=MAX(SC_min, SC_espec)` | 9.58 t |
| SC manto | dato | 1.0 t |

---

## 2. Propiedades de Materiales

### 2.1 Tabla de Aceros (t/cm² a temperatura de trabajo)

| Acero | Nº | Fy | Fu | St | Sd | E |
|-------|----|----|----|----|-----|---|
| A36 | 1 | 2.528 | 4.077 | 1.747 | 1.631 | 2028.5 |
| A 516 Gr 55 | 2 | 2.090 | 3.874 | 1.567 | 1.393 | 2028.5 |
| A 516 Gr 60 | 3 | 2.243 | 4.230 | 1.682 | 1.495 | 2028.5 |
| A 516 Gr 65 | 4 | 2.446 | 4.587 | 1.835 | 1.631 | 2028.5 |
| A 516 Gr 70 | 5 | 2.650 | 4.944 | 1.988 | 1.767 | 2028.5 |
| A37-24ES | 6 | 2.400 | 3.700 | 1.586 | 1.480 | 2028.5 |
| A42-27ES | 7 | 2.700 | 4.200 | 1.800 | 1.680 | 2028.5 |
| A52-34ES | 8 | 3.400 | 5.200 | 2.229 | 2.080 | 2028.5 |
| SS 304 | 9 | 2.090 | 5.250 | 1.896 | 1.580 | 1967.4 |
| SS 304L | 10 | 1.733 | 4.944 | 1.580 | 1.478 | 1967.4 |
| SS 316 | 11 | 2.090 | 5.250 | 1.896 | 1.580 | 1967.4 |
| SS 316L | 12 | 1.733 | 4.944 | 1.580 | 1.478 | 1967.4 |

> Interpolación a temperatura via función VBA `LinInterp()` (ver sección de Macros)

**Fórmulas de propiedades:**
- `Sd = LOOKUP(acero, tabla) * IF(Chek1, F1, 1)` — El factor F1 permite reducir las admisibles
- `St = LOOKUP(acero, tabla_St) * IF(Chek1, F1, 1)`
- `Es = LOOKUP(acero, tabla_E_vs_T)` — interpolado a temperatura de trabajo

---

## 3. Espesores del Manto

### 3.1 Espesor Mínimo según API 650-3.6.1

| Diámetro | Espesor mínimo |
|----------|---------------|
| D < 15 m | 5 mm |
| 15 < D < 36 m | 6 mm |
| 36 < D < 60 m | 8 mm |
| D > 60 m | 10 mm |

```
emin = IF(D<15, 5, IF(D<36, 6, IF(D<60, 8, 10)))  [mm]
```

### 3.2 Tensiones Admisibles según API 650-3.6.2

- Eficiencia de juntas: `E = 90%`
- `Sd` = admisible por diseño (t/cm²) — función del acero y temperatura
- `St` = admisible por prueba hidrostática (t/cm²)

### 3.3 Espesores Calculados — Método de 1 Pie (API 650-3.6.3)

Cada capa `i` (de `n` tramos desde la cúpula hacia el fondo):

**Espesor por diseño (carga de diseño):**
$$t_d = \frac{4.9 \cdot D \cdot (y_i - r - 0.3) \cdot G}{S_d \cdot E \cdot 100} + CAM \quad [mm]$$

**Espesor por prueba hidrostática:**
$$t_t = \frac{4.9 \cdot D \cdot (y_i - r - 0.3)}{S_t \cdot E \cdot 100} \quad [mm]$$

**Espesor de cálculo:**
$$t_i = \max(t_d,\ t_t,\ e_{min}) \quad [mm]$$

Donde:
- $y_i$ = cota del punto de diseño desde el fondo [m], evaluada al pie de la capa `i` multiplicando altura por fracción
- El punto de diseño se toma a 0.3 m (1 pie) sobre el fondo de cada capa

**Fórmula Excel (ejemplo capa 1):**
```excel
D158 = 4.9*$H$86*(C158-$H$89-0.3)*$H$91/($E$149*E148)+$H$99
E158 = 4.9*$H$86*(C158-$H$89-0.3)/($E$150*E148)
F158 = MAX(D158, E158, $E$144)
```

**Peso por capa:**
$$W_{s,i} = \pi \cdot D \cdot t_i \cdot \Delta y_i \cdot 7.85 \times 10^{-3} \quad [t]$$

**Centro de gravedad del manto:**
$$\bar{X}_s = \frac{\sum W_{s,i} \cdot \bar{y}_i}{\sum W_{s,i}}$$

### 3.4 Espesores Comerciales Disponibles

4, 5, 6, 8, 10, 12, 14, 16, 18, 20, 22, 25, 28, 32, 35, 40 mm

> La macro `Genera_Capas()` reescribe dinámicamente las filas de la tabla cuando se cambia el número de tramos (`D154`).

---

## 4. Techo Cónico Autosoportado

### 4.1 Parámetros

| Parámetro | Símbolo | Valor ej. | Fórmula Excel |
|-----------|---------|-----------|---------------|
| Altura techo | `hr` | 2 m | dato |
| Ángulo de inclinación | `θ` | 21.8° | `=ATAN(2·hr/D)·180/π` |
| Verificación API 650-3.10.5.1 | — | OK | `9.5° < θ < 37°` |

### 4.2 Espesor Mínimo (API 650-3.10.5)

$$e_{techo,\min} = \frac{D [ft]}{400 \cdot \sin\theta} + CAT \quad [mm]$$

(con corrección si SC > 0.22·π·D²/4)

```excel
H128 = IF(hr=0, "X",
       IF((SC+W_planchas)/π/D²·4 < 0.22,
          D/0.3048/400/SIN(θ·π/180)·25.4 + CAT,
          D/0.3048/400/SIN(θ·π/180)·25.4·√((SC+Wp)/π/D²·4/0.22) + CAT))
```

### 4.3 Peso del Techo

$$W_{techo} = \frac{\pi D^2 / 4}{\cos\theta} \cdot e_{techo} \cdot 7.85 \times 10^{-3} \quad [t]$$

> La macro `Techo()` activa/desactiva el bloque de cálculo del techo y reescribe las fórmulas de altura total y peso sísmico del techo según el tipo seleccionado.

---

## 5. Pesos y Centros de Gravedad

| Variable | Símbolo | Fórmula Excel | Valor ej. |
|----------|---------|---------------|-----------|
| Peso del manto | `Ws` | `=SUM(H158:H167)` | 13.32 t |
| Peso del techo | `Wr` | `=π·D²/4/cos(θ)·e·7.85e-3` | 5.31 t |
| Peso del líquido | `WT` | `=π·D²/4·H·G` | 706.9 t |
| Peso total sobre NIPB | `Wtotal` | `Ws + Wr + WT` | — |
| Centro gravedad manto | `Xs` | `=SUM(J158:J167)/Ws` | — |
| Centro gravedad techo | `Xr` | `=Hs + hr/3` (aprox.) | — |
| Centro gravedad líquido | `XL` | `=H/2` | — |

---

## 6. Análisis Sísmico (API 650 Anexo E + NCh 2369-2003)

### 6.1 Relación geométrica

$$\frac{D}{H} = \frac{10}{9} = 1.11$$

### 6.2 Masas Participantes (API 650 Figuras E-2, E-3)

**Fracción impulsiva (API 650 Figura E-2):**
$$\frac{W_1}{W_T} = \frac{\tanh(0.866 \cdot D/H)}{0.866 \cdot D/H}$$

**Fracción convectiva (API 650 Figura E-2):**
$$\frac{W_2}{W_T} = 0.23 \cdot \frac{D}{H} \cdot \tanh\!\left(\frac{3.68 \cdot H}{D}\right)$$

**Alturas de las fuerzas (API 650 Figura E-3):**

$$\frac{X_1}{H} = \begin{cases} 0.5 - 0.09375 \cdot D/H & \text{si } D/H < 4/3 \\ 0.375 & \text{si } D/H \geq 4/3 \end{cases}$$

$$\frac{X_2}{H} = 1 - \frac{\cosh(3.68 \cdot H/D) - 1}{(3.68 \cdot H/D)\cdot\sinh(3.68 \cdot H/D)}$$

**Parámetro k (API 650 Figura E-4):**
$$k = \frac{2\pi}{\sqrt{118.4 \cdot \tanh(3.68 \cdot H/D)}}$$

**Período modo convectivo (API 650 E.3.3.2):**
$$T^* = 1.81 \cdot k \cdot \sqrt{D} \quad [s]$$

**Ejemplo (D=10 m, H=9 m):**

| Variable | Valor |
|----------|-------|
| W1/WT | 0.7745 |
| W2/WT | 0.2549 |
| X1/H | 0.3958 → X1 = 3.56 m |
| X2/H | 0.7193 → X2 = 6.47 m |
| T* | 3.31 s |
| W1 | 547.5 t |
| W2 | 180.2 t |

### 6.3 Coeficientes Sísmicos (NCh 2369-2003)

**Modo impulsivo:**
$$C_1 = C_{max} = \frac{A_o \cdot I}{R} \cdot f(\xi) \quad \text{(valor máximo del espectro)}$$

En la planilla: `C1 = LOOKUP(Zona, tabla_C_max)` → para Zona III: **C1 = 0.32**

**Modo convectivo:**
$$C_2 = \max\!\left(\frac{2.75 \cdot A_o}{4} \cdot \left(\frac{T'}{T^*}\right)^n \cdot \left(\frac{0.05}{0.005}\right)^{0.4},\ 0.1 \cdot A_o\right)$$

```excel
H206 = MAX(2.75*Ao/4*(T'/T*)^n*(0.05/0.005)^0.4, 0.1*Ao)
```

### 6.4 Altura de Ola Seicha (modo convectivo)

$$\delta_{max} = 0.3426 \cdot I \cdot C_2 \cdot (T^*)^2 \cdot \tanh\!\left(\frac{4.77}{\sqrt{D/H}}\right) \quad [m]$$

> Verificación: `δmax < r` (revancha) → debe cumplirse. Si no: **Aumentar Revancha**

### 6.5 Solicitaciones en la Base (NIPB)

**Momento volcante sísmico:**
$$M_{sísmico} = I \cdot \left(C_1 \cdot W_r \cdot X_r + C_1 \cdot W_s \cdot X_s + C_1 \cdot W_1 \cdot X_1 + C_2 \cdot W_2 \cdot X_2\right)$$

**Corte basal sísmico:**
$$V_{sísmico} = I \cdot \left(C_1 \cdot W_r + C_1 \cdot W_s + C_1 \cdot W_1 + C_2 \cdot W_2\right)$$

**Ejemplo:** M = 712.6 t·m, V = 188.7 t

---

## 7. Estabilidad al Volcamiento

### 7.1 Verificación de necesidad de anclaje (API 650-E5.1)

**Carga muerta distribuida:**
$$w_t = \frac{W_r + W_s + N_{adicional}}{\pi \cdot D} \quad [t/m]$$

**Resistencia del líquido a volcamiento (API 650-E4.1):**
$$w_L = \frac{\min\!\left(196 \cdot G \cdot H \cdot D,\; 99 \cdot \min(t_{manto}, 6) \cdot \sqrt{F_y \cdot G \cdot H}\right)}{10000} \quad [t/m]$$

**Criterio de anclaje:**
$$\frac{M_{total}}{D^2 \cdot (w_t + w_L)} \begin{cases} < 1.57 & \text{→ No anclar} \\ \geq 1.57 & \text{→ Anclar} \end{cases}$$

En el ejemplo: ratio = 2.98 → **Anclar Estanque**

### 7.2 Verificación al Deslizamiento

$$FS_{deslizamiento} = \frac{\mu \cdot N_{total}}{V_{total}} > 1$$

Con μ = factor de roce acero–base = 0.4

En el ejemplo: FS = 1.54 → **No desliza**

---

## 8. Tensiones en el Manto (API 650 E5)

### 8.1 Tensión de compresión axial (API 650-E5.1)

**Carga lineal máxima en compresión:**
$$b = w_t + \frac{4}{\pi} \cdot \frac{M_{total}}{D^2} \quad [t/m]$$

**Tensión de compresión axial:**
$$f_a = \frac{b}{10 \cdot (t_{manto} - CAM)} \quad [t/cm^2]$$

**Tensión admisible de pandeo (API 650-E5.3):**

$$F_a = \min\!\left(f_{pandeo},\; 0.5 \cdot F_y\right)$$

$$f_{pandeo} = \begin{cases} \dfrac{83 \cdot (t-CAM)}{D} & \text{si } \dfrac{G H D^2}{t^2} \geq 44 \\[6pt] \dfrac{83(t-CAM)}{2.5 D} + 7.5\sqrt{G H} & \text{si } \dfrac{G H D^2}{t^2} < 44 \end{cases} \quad [t/cm^2] \cdot \frac{1}{100}$$

> Factor de Uso: FU = fa / Fa ≤ 1.0 → en el ejemplo: FU = 0.38 ✓

---

## 9. Anillos Rigidizadores de Manto para Viento (API 650-3.9.7)

**Máxima distancia entre anillos:**
$$H_1 = 9.47 \cdot t \cdot \left(\frac{t}{D}\right)^3 \quad [m]$$

**Cantidad de anillos necesarios:**
$$N_{anillos} = \lceil H_s / H_1 \rceil$$

**Módulo de sección mínimo de cada anillo (API 650-3.9.7.6):**
$$Z = \frac{D^2 \cdot H_a}{17} \quad [cm^3]$$

---

## 10. Pernos de Anclaje (API 650)

### 10.1 Diseño de pernos

**Fuerza mínima por unidad de longitud (API 650-E6.1):**
$$w_{pernos} = \frac{4}{\pi} \cdot \frac{M_{total}}{D^2} - w_t \quad [t/m]$$

**Diámetro del círculo de pernos:**
$$D_{cp} = D + \frac{2(a + t_{manto})}{1000} \quad [m]$$

Donde `a` = distancia del borde del estanque al eje del perno

**Cantidad mínima de pernos:**
$$N_{min} = \begin{cases} \pi D_{cp} / 1.8 & \text{si } D_{cp} < 15 \text{ m} \\ \pi D_{cp} / 3.0 & \text{si } D_{cp} \geq 15 \text{ m} \end{cases}$$

**Separación entre pernos:**
$$L_b = \frac{\pi \cdot D_{cp}}{N_{pernos}} \quad [m]$$

**Fuerza de tracción por perno:**
$$T_b = \frac{w_{pernos} \cdot \pi \cdot D}{N_{pernos}} \quad [t]$$

**Ejemplo (N=16 pernos, ø1½"):** Tb = 16.59 t, σ_perno = 1.82 t/cm² < 2.02 t/cm² ✓

### 10.2 Tablas de pernos disponibles

| Perno | d (in) | Ab (cm²) | Ae (cm²) |
|-------|--------|----------|----------|
| 1" | 1.000 | — | — |
| 1¼" | 1.250 | — | — |
| 1½" | 1.500 | 11.40 | 9.10 |
| 1¾" | 1.750 | — | — |
| 2" | 2.000 | — | — |

> Aceros disponibles: ASTM A36, A307, A325, A490, A37-24ES, A42-27ES, A52-34ES

### 10.3 Verificación bajo corte + tracción combinados

Cuando se activa "Tomar Corte con Pernos" (`J313=TRUE`):

**Esfuerzo de corte (1/3 de los pernos resiste corte):**
$$V_{perno} = \frac{V_{sísmico}}{N_{pernos}/3}$$

$$\tau_{perno} = \frac{V_{perno}}{A_b} \quad [t/cm^2]$$

**Admisible de corte con interacción (AISC):**
```excel
tadm = MAX(MIN(0.57*Fy - 1.4*τ, 0.33*Fu*1.33), 0)
p_adm = MAX(MIN(0.57*Fy - 1.4*τ, 1.33*44/√2.15/14.22), 0)   ← ASTM A325
```

---

## 11. Sistema de Anclaje — Silla Aislada

### 11.1 Diseño del Soporte del Perno (6.1)

La silla aislada consiste en una placa de ancho `d` que soporta el perno independientemente del manto.

| Parámetro | Símbolo | Valor ej. | Notas |
|-----------|---------|-----------|-------|
| Distancia borde estanque → eje perno | `a` | 80 mm | dato |
| Ancho del soporte | `d` | 160 mm | dato |
| Distancia entre atiesadores | `c` | 160 mm | dato |
| Altura total soporte | `hp` | 400 mm | dato |
| Dist. eje perno → borde externo | `b` | `d - a` = 80 mm | calculado |
| Perforación para perno | `da` | `(ø+1/16")·25.4` | calculado |
| Constante de tensiones | `j` | 1.25 | — |

**Espesor de la placa del soporte (ta o tg):**
- Si hay anillo rigidizador como soporte: `ta = LOOKUP(espesor_anillo, lista_comercial)`
- Si hay placa aislada: `tg = LOOKUP(espesor_placa, lista_comercial)`

**Fuerza de tracción de diseño:**
$$P = \begin{cases} T_b & \text{según tracción de fluencia} \\ 0.8 \cdot F_y \cdot A_e & \text{según tracción admisible perno} \end{cases}$$
```excel
H334 = IF(J335=1, Tb, IF(J335=2, 0.8*Fy*Ae, Fy*Ab))
```

**Tensión de flexión en la placa del soporte:**
$$\sigma_{f,placa} = \frac{j \cdot P \cdot c}{t_a^2 \cdot (2 \cdot \min(a,b) - d_a)} \quad [t/cm^2]$$

$$FU_1 = \frac{\sigma_{f,placa}}{F_y} \leq 1.0$$

### 11.2 Espesor Placa Base Anular (6.2)

**Ancho de contacto efectivo:**
$$e_{ef} = t_{manto} + 3 \cdot t_b - 1.5 \cdot CAF - CAM \quad [mm]$$

**Tensión de compresión sobre hormigón:**
$$\sigma_c = f_a \cdot \frac{t_{manto}}{e_{ef}} \quad [t/cm^2]$$

**Admisible hormigón:**
$$F_{c,adm} = 0.65 \cdot f'_c \quad [t/cm^2]$$

| Hormigón | f'c (kg/cm²) | Ec (t/cm²) |
|----------|-------------|-----------|
| H20 | 160 | 188 |
| H25 | 200 | 210 |
| H30 | 250 | 235 |

### 11.3 Ancho de la Placa Base Anular (6.3 — API 650-3.5.2 y E4.2)

**Ancho interno mínimo (API 650-3.5.2):**
$$l_{int,min} = \max\!\left(\frac{215 \cdot t_b}{\sqrt{H \cdot G}},\ 600\right) \quad [mm]$$

**Ancho interno mínimo (API 650-E4.2):**
$$l_{int,E4.2} = 0.1745 \cdot \frac{w_L}{G \cdot H} \cdot 10000 \quad [mm]$$

**Proyección exterior mínima (API 650-3.5.2):** l1_min = 50 mm

**Ancho total de la placa:**
$$l_4 = l_{int,min} + d \quad [mm]$$

**Proyección exterior:**
$$l_1 = d \quad [mm]$$

### 11.4 Atiesadores Verticales (6.4)

| Parámetro | Fórmula | Valor ej. |
|-----------|---------|-----------|
| Ancho atiesador | `l = d/10` | 16 cm |
| Altura mínima | `hmin = MAX(25, 8·ø·2.54)` | 30.5 cm |
| Altura efectiva | `hst = (hp - tb - ta)/10` | 35.7 cm |
| Carga por atiesador | `P/2` o `P` según separación | 9.21 t |
| Espesor placa | dato (lista comercial) | 10 mm |

**Verificación como columna en compresión pura:**
$$f_c = \frac{P/2}{l \cdot t_s} \quad [t/cm^2]$$

**Radio de giro:**
$$i_x = \frac{t_s}{\sqrt{12}} \quad [cm]$$

**Esbeltez:**
$$\lambda_x = \frac{0.75 \cdot h_{st}}{i_x}$$

**Tensión admisible a compresión (AISC):**
$$F_c = \max\!\left(0,\ \frac{1.33 \cdot F_y \cdot \left(1 - \frac{1}{2}\left(\frac{\lambda_x}{C_c}\right)^2\right)}{\frac{5}{3} + \frac{3}{8}\frac{\lambda_x}{C_c} - \frac{1}{8}\left(\frac{\lambda_x}{C_c}\right)^3}\right) \quad [t/cm^2]$$

$$C_c = \sqrt{\frac{2\pi^2 \cdot 2100}{F_y}}$$

$$FU_{atiesador} = \frac{f_c}{F_c} \leq 1.0$$

---

## 12. Verificaciones Adicionales del Manto (Zona de Anclaje)

### 12.1 Tensión Circunferencial Hidrostática

$$\sigma_H = \frac{D \cdot (H - 0.3) \cdot G}{20 \cdot (t_{manto} - CAM)} \quad [t/cm^2]$$

### 12.2 Tensión de Tracción Circunferencial Membranal

**Mitad del ángulo entre pernos:**
$$\phi = \frac{180°}{N_{pernos}}$$

**Factor de amplificación:**
$$\gamma = \min\!\left(2.2,\ \max\!\left(1,\ 0.3 \cdot (\phi+1) \cdot \left(\frac{H}{D}\right)^{1/3}\right)\right)$$

$$\sigma_{cm,t} = \max\!\left[\left(\frac{T}{2 \sin\phi \cdot (t-CAM) \cdot \sqrt{D \cdot (t-CAM)}}\right) \cdot \gamma \cdot 0.79 + 0.29,\ \sigma_H\right]$$

$$FU_1 = \frac{\sigma_{cm,t}}{1.5 \cdot S_d} \leq 1.0$$

Donde la fuerza T (reacción del par sobre el manto):
$$T = \frac{T_b \cdot a}{h_{st} - t_b/2 - t_a/2}$$

### 12.3 Tensión de Tracción Longitudinal (API 650 + Local)

$$\sigma_{l,t} = \frac{T_{b,tracción}}{10 \cdot (t_{manto}-CAM)} \quad \text{(API 650)}$$

$$\sigma_{lm,t} = \max\!\left(\frac{T_b}{(c + t_s + 7(t-CAM)) \cdot (t-CAM)/100} \cdot 0.715 - 0.165,\ \sigma_{l,t}\right)$$

$$\sigma_{lf,t} = \frac{T}{(c + t_s)(t-CAM)/100} \cdot 4.16 + 1.43$$

$$FU_2 = \frac{\sigma_{lm,t} + \sigma_{lf,t}}{2.5 \cdot S_t} \leq 1.0$$

### 12.4 Intensidad de Compresión en Zona Comprimida

$$C_{st} = f_a \cdot \frac{2 \cdot A_{st} \cdot A_{Lb}}{2 A_{st} + A_{Lb}}$$

$$\sigma_c = \frac{110 \cdot C_{st}}{(c + t_s - 5(t-CAM))(t-CAM)} + 2.97 \cdot \sigma_H$$

$$FU_3 = \frac{\sigma_c}{2.5 \cdot S_t} \leq 1.0$$

### 12.5 Verificación de Pandeo Local (Tensión Amplificada)

$$FU_4 = \frac{1.2 \cdot f_a}{F_a} \leq 1.0$$

---

## 13. Cálculo de Fundaciones

### 13.1 Dimensiones

La fundación es anular (zapata circular hueca):

| Parámetro | Símbolo | Valor ej. |
|-----------|---------|-----------|
| Sello de fundación | Df | 1.5 m |
| Alto pedestal | hp | 0 m |
| Ancho pedestal | bp | 0.5 m |
| Alto zapata | hz | 0.5 m |
| Ancho zapata | bz | 2.0 m |
| Pestaña | b_pestaña | 0.75 m |
| Radio exterior | r | `D/2 + t_manto + d + b_pestaña` |
| Radio interior | r1 | `r - bz` |

$$A_f = \pi (r^2 - r_1^2)$$
$$W_f = \frac{\pi (r^4 - r_1^4)}{4 r}$$

### 13.2 Cargas en el Sello

**Condición estática:**
$$W_{sello} = W_e + W_f$$

**Condición sísmica (API 650):**
$$W_{sello,sis} = W_e \cdot \left(1 - \frac{2}{3} \cdot I \cdot A_o\right) + W_f$$

**Excentricidad:**
$$e = \frac{M_o}{W_{sello,sis}}$$

$$\frac{e}{r} = \frac{e}{r_{exterior}}$$

### 13.3 Presiones en el Suelo (Beton Kalender)

Las presiones máximas se obtienen de tablas en función de $e/r$ y $r_1/r$ via interpolación `LinInterp`:

$$q_{máx} = \left(\frac{q_{máx}}{q_{medio}}\right) \cdot q_{medio} = \left(\frac{W_{sello}}{A_f}\right) \cdot \left(\frac{q_{máx}}{q}\right)_{tabla}$$

**Presión admisible estática:** $q_{adm,est}$ (dato, ej. 20 t/m²)

**Presión admisible sísmica:** $q_{adm,sis} = 1.5 \cdot q_{adm,est}$

**Área apoyada como fracción del total:**
$$\frac{A_{apoyada}}{A_f} \geq 0.8 \quad \text{(mínimo recomendado)}$$

---

## 14. Macros VBA

El archivo `.xlsm` contiene un módulo VBA (`Módulo.bas`) con las siguientes funciones:

### 14.1 `LinInterp(xdata, ydata, xvalue)` — Interpolación lineal

```vba
Function LinInterp(xdata, ydata, xvalue)
    ' Interpola linealmente en arrays xdata/ydata para el valor xvalue
    ' Respeta límites: si xvalue <= xdata(1) → retorna ydata(1)
    '                  si xvalue >= xdata(n) → retorna ydata(n)
    
    data_number = xdata.Count
    If xvalue <= xdata(1) Then
        LinInterp = ydata(1)
    ElseIf xvalue >= xdata(data_number) Then
        LinInterp = ydata(data_number)
    Else
        I = 1
        While xdata(I) < xvalue : I = I + 1 : Wend
        LinInterp = ydata(I-1) + (ydata(I) - ydata(I-1)) / _
                    (xdata(I) - xdata(I-1)) * (xvalue - xdata(I-1))
    End If
End Function
```

**Equivalente Python:**
```python
import numpy as np
def lin_interp(xdata, ydata, xvalue):
    return float(np.interp(xvalue, xdata, ydata))
```

### 14.2 `Redondear_Lista(xdata, xvalue)` — Redondeo a valor superior en lista

```vba
Function Redondear_Lista(xdata, xvalue)
    ' Retorna el primer valor de xdata que sea >= xvalue
    ' Usado para estandarizar espesores a los valores comerciales disponibles
    data_number = xdata.Count
    If xvalue <= xdata(1) Then
        Redondear_Lista = xdata(1)
    ElseIf xvalue >= xdata(data_number) Then
        Redondear_Lista = xdata(data_number)
    Else
        I = 1
        While xdata(I) < xvalue : I = I + 1 : Wend
        Redondear_Lista = xdata(I)
    End If
End Function
```

**Equivalente Python:**
```python
import bisect
def redondear_lista(lista, valor):
    i = bisect.bisect_left(lista, valor)
    return lista[min(i, len(lista)-1)]
```

### 14.3 `Genera_Capas()` — Generación dinámica de tabla de manto

Borra el rango `B158:H167` y escribe fórmulas de espesor por hidrostática para `n` tramos:

```vba
Sub Genera_Capas()
    no_capas = Range("D154")   ' Número de capas (1-10)
    H = Range("H90")           ' Altura del manto

    Range("B158:H167").Formula = ""
    For I = 1 To no_capas
        Cells(157+I, 3).Formula = "=H90*" & I & "/" & no_capas        ' cota yi
        Cells(157+I, 4).Formula = "=4.9*$H$86*(C" & (157+I) & _
            "-$H$89-0.3)*$H$91/($E$149*E148)+$H$99"                   ' td
        Cells(157+I, 5).Formula = "=4.9*$H$86*(C" & (157+I) & _
            "-$H$89-0.3)/($E$150*E148)"                                ' tt
        Cells(157+I, 6).Formula = "=MAX(D" & (157+I) & ",E" & _
            (157+I) & ",$E$144)"                                       ' t_calc
    Next I
End Sub
```

### 14.4 `Techo()` — Activación/desactivación del bloque de techo cónico

- Si `J124 = 1` (techo cónico): activa filas 117-131, asigna fórmulas dinámicas
- Si `J124 = 2` (sin techo o externo): oculta resultados del techo, fija pesos = 0

### 14.5 `resuelve_equilibrio()` — Solver para equilibrio de volcamiento

Llama al Solver de Excel para resolver `H273 = 0` (tensión de pandeo nula) variando `H266`.

### 14.6 `Cambia_Estanque(tipo)` — Copia de parámetros entre hojas

Copia 26 variables de entrada (dimensiones, materiales, parámetros sísmicos) desde la hoja activa hacia la hoja destino, luego ejecuta `Techo()` y `Genera_Capas()`.

---

## 15. Flujo General de Cálculo

```
1. DATOS DE ENTRADA
   └─ Dimensiones (D, H, r, G, T°)
   └─ Material manto (Acero → Fy, Sd, Es a T°)
   └─ Corrosiones (CAT, CAM, CAF)
   └─ Parámetros sísmicos (Zona, Categoría, Suelo, R)
   └─ Sobrecargas (SC techo/manto)

2. GEOMETRÍA Y PESOS
   └─ Techo cónico (θ, etecho, Wr)
   └─ Capas del manto (td, tt, ti_comercial, Ws, Xs)
   └─ Placa fondo (efondo)

3. ANÁLISIS SÍSMICO (API 650 Anexo E + NCh 2369)
   └─ Masas W1, W2 y alturas X1, X2
   └─ Período T*, coeficientes C1, C2
   └─ Revancha requerida vs disponible
   └─ Momento volcante M y corte V en base

4. ESTABILIDAD
   └─ Volcamiento (wt, wL, ratio M/D²(wt+wL) vs 1.57)
   └─ Deslizamiento (FS = μN/V > 1)
   └─ Tensiones en manto (fa vs Fa)

5. PERNOS DE ANCLAJE
   └─ N° mínimo, fuerza Tb por perno
   └─ Tensión perno tracción + interacción corte

6. SISTEMA DE SOPORTE (SILLA AISLADA)
   └─ Placa soporte: sf,placa vs Fy
   └─ Placa base anular: σc vs 0.65f'c
   └─ Ancho placa base: API 650-3.5.2 y E4.2
   └─ Atiesadores: fc vs Fc (AISC)

7. VERIFICACIONES MANTO EN ZONA DE ANCLAJE
   └─ FU1: σcm,t ≤ 1.5Sd
   └─ FU2: σlm,t + σlf,t ≤ 2.5St
   └─ FU3: σc ≤ 2.5St
   └─ FU4: 1.2fa ≤ Fa

8. FUNDACIONES
   └─ Geometría anular, pesos
   └─ Excentricidad e/r
   └─ Presiones suelo (tablas Beton Kalender + LinInterp)
   └─ qmax estático ≤ qadm ; qmax sísmico ≤ 1.5qadm
```

---

## 16. Resumen de Resultados del Ejemplo

| Verificación | FU / Resultado | Estado |
|---|---|---|
| Volcamiento | 2.98 → Anclar | — |
| Deslizamiento | FS = 1.54 | OK |
| Pandeo manto (fa/Fa) | 0.38 | OK |
| Perno tracción | 0.90 | OK |
| Perno corte | 3.36 | **No Cumple** |
| Placa soporte perno (FU1) | 0.99 | OK |
| Placa base anular (tb) | 0.29 | OK |
| Atiesador (FU) | 0.44 | OK |
| Manto FU1 (circ. membranal) | 0.57 | OK |
| Manto FU2 (long. membranal+flex.) | 0.93 | OK |
| Manto FU3 (compresión) | 0.66 | OK |
| Manto FU4 (pandeo amplificado) | 0.46 | OK |
| Suelo estático (qmax/qadm) | OK | OK |
| Suelo sísmico (qmax/qadm,sis) | OK | OK |
| Área apoyada ≥ 80% | 95.6% | OK |

> El único fallo en el ejemplo es corte en pernos — en la práctica se aumenta el número de pernos o se activa la opción "Tomar Corte con Pernos" (`J313`).

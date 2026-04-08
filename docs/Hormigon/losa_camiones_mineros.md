# Diseño de Losa de Concreto para Camiones Mineros
> **Norma principal:** ACI CODE-318-25 (SI)  
> **Contexto:** Losas industriales pesadas — talleres, rampas, canchas de volteo, patios de mantenimiento  
> **Camiones típicos:** Cat 793/797, Komatsu 830E/930E, Liebherr T 264/284 (carga útil 220–360 t)

---

## 1. Caracterización de Cargas

### 1.1 Carga de rueda — datos clave

| Parámetro | Valor orientativo |
|-----------|------------------|
| Peso bruto total (GVW) | 400–850 t (vehículo + carga) |
| Número de ejes | 2 (1 delantero + 1 trasero) |
| Ruedas por eje trasero | 4 (dos duales por lado) |
| Carga por llanta trasera | 100–180 t |
| Área de contacto por llanta | 0.35 × 0.60 m aprox. (varía por presión de inflado) |
| Separación eje trasero–eje delantero | 7–9 m |

> **Regla práctica:** Dimensionar con la carga individual por llanta más desfavorable, no el peso total del camión. El eje trasero es el crítico.

### 1.2 Factor de amplificación dinámico (DAF)

Para vehículos de minería que circulan a baja velocidad (< 15 km/h):

| Condición | DAF recomendado |
|-----------|----------------|
| Superficie lisa, transición suave | 1.10 – 1.20 |
| Juntas o badenes a baja velocidad | 1.25 – 1.40 |
| Frenado de emergencia o arranque | 1.30 – 1.50 |
| Impacto en rampas con desnivel | 1.50 – 2.00 |

Aplicar como factor sobre la carga estática: $P_{din} = DAF \times P_{estática}$

### 1.3 Carga distribuida equivalente

Para análisis simplificado con losa sobre suelo (Westergaard / PCA):

$$q_{eq} = \frac{P_{llanta}}{A_{contacto}} \quad [\text{kPa}]$$

Valores típicos: 800–1500 kPa bajo la llanta.

### 1.4 Combinaciones de carga (ACI 318-25, §5.3)

Para cargas de camiones (carga viva vehicular pesada = L):

$$U_1 = 1.4D$$
$$U_2 = 1.2D + 1.6L + 0.5(S \text{ ó } R)$$
$$U_3 = 1.2D + 1.6W + L + 0.5(S \text{ ó } R)$$

> En minería, la combinación $U_2$ con $L$ amplificado por DAF suele ser la gobernante.

---

## 2. Tipología Estructural Recomendada

### 2.1 Alternativas según contexto

| Tipo | Cuándo usarla | Limitaciones |
|------|--------------|--------------|
| **Losa sobre terreno (slab-on-grade)** | Suelo competente (k ≥ 30 MPa/m), sin sótano | Depende fuertemente de la preparación de la subrasante |
| **Losa maciza elevada** | Sobre vigas o pilares, suelo deficiente o con excavaciones | Mayor costo, punzonamiento crítico |
| **Losa nervada / waffle** | Grandes luces con cargas moderadas | No recomendada para cargas concentradas muy pesadas |
| **Losa maciza con capiteles o drop panels** | Cargas muy concentradas, luces > 6 m | Costo moderado, mejor comportamiento a punzonamiento |

**Recomendación general:** Para taller de mantenimiento de camiones ultraclass (cargo > 200 t), preferir **losa sobre terreno con espesor 400–700 mm** o **losa maciza elevada con vigas invertidas** si el suelo es problemático.

### 2.2 Espesor de losa — estimación inicial

#### Sobre terreno (Westergaard):
$$h = \sqrt{\frac{3P(1-\mu^2)}{f_r \pi}} \approx 350\text{–}700 \text{ mm}$$

donde $f_r = 0.62\lambda\sqrt{f'_c}$ (módulo de ruptura, ACI 318-25 §19.2.3).

#### Losa elevada (limitación por deflexiones, ACI 318-25 Tabla 8.3.1.2):
$$h_{min} = \frac{\ell_n}{33} \text{ (losa simple)} \quad;\quad h_{min} = \frac{\ell_n}{30} \text{ (continua)}$$

Para luces típicas de 6–9 m y cargas mineras: $h \geq 400$ mm suele ser necesario.

---

## 3. Modelamiento y Análisis

### 3.1 Modelo para losa sobre terreno

- Usar modelo de **Placa sobre fondación elástica (Winkler)** con módulo de reacción del suelo $k_s$.
- Software recomendado: SAP2000 (Shell + Link spring), SAFE, PLAXIS 2D/3D.
- Calcular $k_s$ con ensayo de placa (ASTM D1195) o correlación con CBR/E_suelo.

**Correlación orientativa:**

| Tipo de suelo compactado | $k_s$ [MPa/m] |
|--------------------------|--------------|
| Arcilla blanda | 10 – 25 |
| Arcilla compactada | 25 – 50 |
| Grava bien gradada compactada | 80 – 150 |
| Roca meteorizada / relleno estructural | 150 – 400 |

> **Importante:** La subrasante de un taller minero debe tener $k_s \geq 50$ MPa/m para que el espesor de losa sea viable. Mejorar con sub-base granular de 300–500 mm.

### 3.2 Modelo para losa elevada

- Modelo de **losa Shell** (elementos FEM) con apoyos en columnas o vigas.
- Incluir rigidez de columnas y condiciones de borde reales.
- Verificar deformaciones bajo carga de servicio (L sin amplificar).

### 3.3 Posición crítica de carga

Evaluar las siguientes posiciones de rueda:

1. **Centro de panel** → máximo momento positivo central
2. **Borde de losa libre** → máximo momento de borde (30–40% mayor que centro)
3. **Esquina de losa** → peor escenario de curling térmico + carga
4. **Junto a junta o borde de apoyo** → concentración de tensiones

> Siempre analizar la carga como **huella de contacto real** (área rectangular o elíptica), no como carga puntual, especialmente para losas sobre terreno.

### 3.4 Efectos adicionales a considerar

- **Gradiente térmico** (curling): Losas expuestas al sol o en zonas con variaciones > 20°C presentan alabeo (warping) que reduce el apoyo efectivo y aumenta las tensiones de borde.
- **Retracción y contracción plástica:** Usar juntas de contracción cada 4–6 m máximo para losas sobre terreno.
- **Cargas de fatiga:** Los camiones circulan miles de ciclos; aplicar reducción de resistencia por fatiga si $N > 10^5$ ciclos (ver ACI 215R).

---

## 4. Verificaciones de Diseño — ACI 318-25

### 4.1 Flexión (Capítulo 7 / 8 / 22)

$$\phi M_n \geq M_u$$

$$M_n = A_s f_y \left(d - \frac{a}{2}\right), \quad a = \frac{A_s f_y}{0.85 f'_c b}$$

**Límites de refuerzo (ACI 318-25, §8.6.1):**

$$A_{s,min} = \frac{0.25\sqrt{f'_c}}{f_y} b_w d \geq \frac{1.4}{f_y} b_w d$$

$$\rho_{max} = 0.75 \rho_{bal} \quad \text{(recomendado para ductilidad)}$$

### 4.2 Punzonamiento / Corte Bidireccional ⚠️ CRÍTICO (ACI 318-25, §22.6)

La carga de llanta actúa como carga concentrada → *punzonamiento gobernante en losas elevadas*.

**Sección crítica:** Perímetro $b_o$ a $d/2$ del borde del área cargada.

$$V_u \leq \phi V_c$$

$$V_c = \min\left[ 0.33\lambda_s\lambda\sqrt{f'_c}\,b_o d \;;\; \left(0.17 + \frac{0.33}{\beta}\right)\lambda_s\lambda\sqrt{f'_c}\,b_o d \;;\; \left(0.083\alpha_s\frac{d}{b_o}+0.17\right)\lambda_s\lambda\sqrt{f'_c}\,b_o d \right]$$

donde:
- $\lambda_s = \sqrt{2/(1 + d/250)} \leq 1.0$ — factor de efecto de tamaño (nuevo en ACI 318-19/25)
- $\beta$ = relación largo/corto del área cargada
- $\alpha_s$ = 40 (columna interior), 30 (borde), 20 (esquina)

> **Efecto de tamaño:** Para losas gruesas ($d > 300$ mm) el factor $\lambda_s$ reduce significativamente $V_c$. Verificar siempre con el valor correcto; no copiar tablas de losas convencionales.

**Si $V_u > \phi V_c$:** Agregar refuerzo de corte (studs o estribos) según §8.7.6–8.7.7.

### 4.3 Corte Unidireccional (ACI 318-25, §22.5)

Para losas sobre vigas o losas bidireccionales cerca de apoyos lineales:

$$V_u \leq \phi V_c = \phi \left[A_w f_{yv} + 0.66(\rho_w)^{1/3}\lambda_s\lambda(f'_c)^{1/3} b_w d\right]$$

(Tabla 22.5.5.1, ecuación general)

Para losas sin refuerzo al corte, usar la expresión simplificada:

$$V_c = k \cdot 0.66\lambda(\rho_w)^{1/3}(f'_c)^{1/3} b_w d$$

### 4.4 Deflexiones (ACI 318-25, §24.2)

| Condición | Límite ACI |
|-----------|-----------|
| Deflexión inmediata bajo L | $\ell/360$ |
| Deflexión total (L + diferida) | $\ell/240$ |
| Con muros u elementos frágiles | $\ell/480$ |

> Para cargas de camiones: controlar la deflexión diferida (multiplicador $\lambda_\Delta = 2.0$ para 5 años, según §24.2.4). Losas gruesas industriales suelen controlar por resistencia, no por deflexión.

### 4.5 Módulo de Ruptura — resistencia a tracción del concreto (§19.2.3)

$$f_r = 0.62\lambda\sqrt{f'_c} \quad [\text{MPa}]$$

Crítico para losas sobre terreno (tensión en cara inferior).

### 4.6 Control de Fisuración (ACI 318-25, §24.3)

Para exposición industrial / minera (humedad, aceites, abrasión):

$$s_{max} = \min\left[\frac{380(280/f_s)}{} - 2.5c_c \;;\; \frac{300(280/f_s)}{}\right] \quad [\text{mm}]$$

Usar barras de diámetro mayor y mayor cuantía para reducir abertura de fisuras.

---

## 5. Materiales Recomendados

| Material | Especificación recomendada | Justificación |
|---------|--------------------------|---------------|
| Concreto | $f'_c \geq 35$ MPa (H35) | Resistencia al punzonamiento, durabilidad |
| Concreto exposición abrasiva | $f'_c \geq 40$ MPa + endurecedor de piso | Tráfico intenso de neumáticos macizos |
| Acero de refuerzo | ASTM A615 Gr. 60 ($f_y = 420$ MPa) | Estándar minería |
| Fibras de acero (opcional) | 30–50 kg/m³ (ASTM A820) | Aumenta $V_c$ un ~30%, reduce agrietamiento |
| Aditivos | Reductor de agua, inhibidor de retracción | Durabilidad en ambiente minero |
| Cubrimiento | $c_{min} \geq 50$ mm (exposición severa) | ACI 318-25 Tabla 20.6.1.3 |

### Clases de exposición ACI 318-25 (Capítulo 19)

| Clase | Condición | Requisito |
|-------|-----------|-----------|
| W2 | Agua en contacto frecuente | $f'_c \geq 35$ MPa, $w/c \leq 0.45$ |
| C2 | Moderada exposición a cloruros (agua subterránea) | Recubrimiento especial o acero galvanizado |
| F1 | Ciclos de hielo (>3000 masnm) | $f'_c \geq 31$ MPa + aire incorporado |

---

## 6. Diseño de Juntas (Slab-on-Grade)

| Tipo de junta | Espaciado | Detalle |
|--------------|-----------|---------|
| Contracción (serrada) | 4–5 m (< 25× espesor) | Profundidad $\geq h/3$, sellada con poliuretano |
| Aislamiento | En columnas, muros y cambios de espesor | Junta completa con poliestireno |
| Construcción | Cada colada | Pasadores Ø25–32 mm @ 300 mm |

> Para tráfico de camiones mineros: preferir **juntas con pasadores** en vez de juntas libres. Los pasadores transfieren carga y reducen el diferencial de deflexión que rompe los bordes.

---

## 7. Verificaciones Adicionales para Minería

### 7.1 Carga de impacto en rampas

En rampas con pendiente > 5% o con quiebre de pendiente, la carga dinámica puede duplicarse. Usar:

$$P_{diseño} = 2.0 \times P_{estática} \quad \text{(en quiebres de rampa)}$$

### 7.2 Resistencia a la abrasión

- Agregar **endurecedor de cuarzo o corindón** en superficie (3–5 kg/m²) aplicado al fresco.
- Resistencia mínima al desgaste: abrasión Taber < 3 mm de pérdida en 400 ciclos (ASTM C779).

### 7.3 Elevación con gato hidráulico (jack load)

Los camiones mineros se levantan con gatos de 50–150 t en puntos específicos. Verificar la losa bajo carga de gato como **carga concentrada pura** en el área mínima de apoyo (típico 0.30 × 0.30 m).

### 7.4 Derrame de combustibles e hidrocarburos

- Especificar recubrimiento epóxico o concreto con adición de humo de sílice para resistencia química.
- Diseñar pendientes ≥ 1.5% hacia sumideros de aceite.

---

## 8. Lista de Verificaciones de Diseño

### Pre-diseño
- [ ] Definir tipo y peso bruto del camión más pesado (GVW máximo)
- [ ] Obtener área de contacto de llanta del fabricante
- [ ] Caracterizar el suelo: CBR, módulo de reacción (ensayo de placa)
- [ ] Definir clase de exposición (ACI 318-25 Cap. 19)
- [ ] Definir si la losa es sobre terreno o elevada

### Análisis
- [ ] Modelo de fundación elástica con $k_s$ ajustado por tamaño de placa (corrección por $k_s$ de placa vs. prototipo)
- [ ] Evaluar posición de carga en borde y esquina de losa
- [ ] Incluir gradiente térmico de ± 10–15°C entre cara superior e inferior
- [ ] Verificar combinaciones U2 y U3 del ACI 318-25 §5.3

### Diseño estructural
- [ ] Flexión: $\phi M_n \geq M_u$ (cara superior e inferior)
- [ ] Punzonamiento: $\phi V_c \geq V_u$ con $\lambda_s$ por espesor (ACI 318-25 §22.6)
- [ ] Corte unidireccional: $\phi V_c \geq V_u$ (ACI 318-25 §22.5)
- [ ] Deflexiones: inmediata + diferida ≤ límites ACI Tabla 24.2.2
- [ ] Control de fisuración: espaciado de barras (ACI 318-25 §24.3)
- [ ] Verificar $A_{s,min}$ por temperatura y retracción (ACI 318-25 §24.4)
- [ ] Refuerzo mínimo de temperatura: $\rho_t \geq 0.0018$ (Gr. 420)

### Detalle / Constructivo
- [ ] Cubrimiento mínimo = 50 mm (exposición severa)
- [ ] Diseño de juntas con pasadores de transferencia de carga
- [ ] Espaciado de juntas ≤ 25× espesor de losa
- [ ] Especificar endurecedor de piso para tráfico pesado
- [ ] Plan de curado (mínimo 7 días) — crítico en losas gruesas

---

## 9. Referencias

| Referencia | Uso |
|-----------|-----|
| ACI CODE-318-25 Cap. 8 | Losas bidireccionales, punzonamiento |
| ACI CODE-318-25 Cap. 22 §22.5–22.6 | Corte unidireccional y punzonamiento |
| ACI CODE-318-25 Cap. 19 | Durabilidad y materiales |
| ACI CODE-318-25 Cap. 24 | Deflexiones y control de fisuras |
| ACI 360R-10 | Guide to Design and Construction of Concrete Slabs-on-Ground |
| ACI 215R-74 (reaf. 92) | Consideraciones de fatiga en concreto |
| PCA "Thickness Design for Concrete Highway and Street Pavements" | Diseño por Westergaard |
| Terzaghi (1955) | Módulo de reacción del suelo $k_s$ |

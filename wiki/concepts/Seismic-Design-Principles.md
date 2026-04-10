---
title: "Principios de Diseño Sísmico"
type: concept
domain: "sismo"
level: "fundamental"
tags: [sismo, diseño-sísmico, espectro, amortiguamiento, zonificación]
related:
  - ../standards/NCh2369-2025.md
  - ../standards/ACI-318-25.md
  - ../formulas/NCh2369-Spectrum.md
  - ../formulas/API-650-Seismic.md
  - ../applications/Tank-Seismic-Analysis.md
  - ../references/Chile-Specific-Variants.md
created: 2026-04-09
updated: 2026-04-09
---

# Principios de Diseño Sísmico

## Definición

El diseño sísmico busca garantizar que las estructuras resistan terremotos con un nivel de desempeño aceptable: protección de la vida (estado límite último) y limitación de daño (serviciabilidad). Se basa en definir la demanda sísmica mediante un espectro de diseño y verificar que la capacidad de la estructura sea suficiente.

## Principios Fundamentales

### 1. Zonificación Sísmica

El territorio se divide en zonas según el nivel de peligro sísmico. Cada zona tiene una aceleración efectiva de diseño ($A_r$).

- **Chile (NCh2369):** 3 zonas — Zona 1 (0.20g), Zona 2 (0.30g), Zona 3 (0.40g)
- Las zonas de mayor peligro se concentran en la costa del Pacífico (subducción de las placas de Nazca y Sudamericana)

### 2. Clasificación de Suelos

El tipo de suelo amplifica o atenúa las ondas sísmicas. La clasificación se basa en la velocidad de ondas de corte ($V_s$) o ensayos geotécnicos.

| Tipo | Descripción | Efecto |
|------|------------|--------|
| A (Roca dura) | $V_s > 900$ m/s | Mínima amplificación |
| B (Roca) | 360 < $V_s$ ≤ 760 m/s | Amplificación leve |
| C (Suelo denso) | 180 < $V_s$ ≤ 360 m/s | Amplificación moderada |
| D (Suelo blando) | $V_s$ ≤ 180 m/s | Amplificación significativa |
| E (Suelo muy blando) | Suelos especiales | Amplificación severa, posible licuefacción |

### 3. Espectro de Diseño

El espectro de respuesta describe la aceleración máxima esperada en función del período de la estructura. Se construye a partir de:

- **Aceleración de zona** ($A_r$): peligro sísmico base
- **Amplificación de suelo** ($S$): efecto local del suelo
- **Forma espectral**: depende de parámetros del suelo ($r$, $T_0$, $p$, $q$)
- **Amortiguamiento** ($\eta$): ajuste por disipación de energía
- **Factor de reducción** ($R^*$): reducción por ductilidad y redundancia
- **Importancia** ($I$): amplificación para estructuras esenciales

Ver: [NCh2369 — Espectro Sísmico](../formulas/NCh2369-Spectrum.md)

### 4. Amortiguamiento

El amortiguamiento mide la disipación de energía de la estructura. Valores típicos:

| Estructura | ξ | η |
|-----------|---|---|
| Edificios de hormigón | 5% | 1.00 |
| Estructuras de acero | 2% | 1.45 |
| Estanques — componente impulsiva | 2% | 1.45 |
| Estanques — componente convectiva | 0.5% | 2.51 |
| Equipos rígidos | 2–3% | 1.26–1.45 |

$$\eta = \left(\frac{0.05}{\xi}\right)^{0.4}$$

> Un amortiguamiento menor que 5% **incrementa** la demanda sísmica (η > 1).

### 5. Factores de Reducción de Respuesta

El factor $R$ reduce la demanda elástica al reconocer la capacidad dúctil de la estructura:

| Sistema | R típico |
|---------|---------|
| Marco de momento especial (hormigón) | 7–8 |
| Marco de momento ordinario | 3 |
| Muro especial de hormigón | 5–6 |
| Estanque sobre suelo (impulsivo) | 4 |
| Estanque sobre suelo (convectivo) | 1.5 |

> El factor $R$ para la componente convectiva es bajo (1.5) porque el sloshing es esencialmente elástico y no disipa energía por deformación inelástica.

### 6. Factor de Amplificación Dinámica (DAF)

Para cargas de impacto y vehículos (no sísmico, pero relacionado con diseño dinámico):

$$P_{din} = DAF \times P_{estática}$$

| Condición | DAF |
|-----------|-----|
| Superficie lisa, baja velocidad | 1.10–1.20 |
| Juntas o badenes | 1.25–1.40 |
| Frenado de emergencia | 1.30–1.50 |
| Impacto en rampas | 1.50–2.00 |

## Estándares Relacionados

- [NCh2369-2025](../standards/NCh2369-2025.md) — Diseño sísmico industrial (Chile)
- [ACI 318-25](../standards/ACI-318-25.md) — Cap. 18: diseño sísmico de hormigón
- [API 650](../standards/API-650.md) — Annex E: diseño sísmico de estanques

## Fórmulas Relacionadas

- [NCh2369 — Espectro](../formulas/NCh2369-Spectrum.md)
- [API 650 — Sismo](../formulas/API-650-Seismic.md)

## Referencias

- NCh2369-2025, Capítulos 5–7
- ACI 318-25, Capítulo 18
- API 650, Annex E

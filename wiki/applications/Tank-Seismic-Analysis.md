---
title: "Workflow: Análisis Sísmico de Estanque Cilíndrico"
type: application
domain: "estanques"
standards_used: ["API-650", "NCh2369-2025"]
tags: [estanques, sismo, workflow, API-650, NCh2369]
related:
  - ../standards/API-650.md
  - ../standards/NCh2369-2025.md
  - ../formulas/API-650-Seismic.md
  - ../formulas/NCh2369-Spectrum.md
  - ../concepts/Seismic-Design-Principles.md
created: 2026-04-09
updated: 2026-04-09
---

# Workflow: Análisis Sísmico de Estanque Cilíndrico

## Objetivo
Evaluar la respuesta sísmica de un estanque cilíndrico de acero soldado apoyado sobre suelo, combinando el modelo de masas de API 650 con el espectro sísmico de NCh2369.

## Estándares Aplicables
- [API 650](../standards/API-650.md): Annex E — modelo de masas equivalentes, alturas, períodos
- [NCh2369-2025](../standards/NCh2369-2025.md): espectro de diseño, factores R y ξ para estanques

## Datos de Entrada

| Parámetro | Símbolo | Descripción |
|-----------|---------|------------|
| Diámetro | $D$ | Diámetro interior del estanque |
| Altura del líquido | $h_{liq}$ | Nivel operacional del líquido |
| Espesor de pared | $e_{pared}$ | Espesor del primer anillo (o promedio ponderado) |
| Densidad del líquido | $\rho_{liq}$ | Agua: 1,000 kg/m³; petróleo: 800–950 kg/m³ |
| Zona sísmica | — | Zona 1, 2 o 3 (NCh2369) |
| Tipo de suelo | — | A, B, C, D o E (NCh2369) |
| Factor de importancia | $I$ | 1.0 (normal) o 1.2 (esencial) |

## Procedimiento

### 1. Cálculo de Masas Equivalentes
Determinar la relación $h_{liq}/D$ y calcular:

$$m_i = M_{liq} \cdot \frac{\tanh(0.866\,D/h_{liq})}{0.866\,D/h_{liq}}$$

$$m_c = M_{liq} \cdot 0.23 \cdot \frac{\tanh(3.68\,h_{liq}/D)}{h_{liq}/D}$$

Verificar: $m_i + m_c \approx M_{liq}$

### 2. Alturas de Aplicación
Calcular $h_i$, $h_c$ (sin presión sobre fondo) y $h'_i$, $h'_c$ (con presión sobre fondo):
- Ver fórmulas en [API 650 — Sismo](../formulas/API-650-Seismic.md)

### 3. Períodos de Vibración

$$T_c = \frac{2\pi}{\sqrt{3.68\,\tanh(3.68\,h_{liq}/D)}} \cdot \sqrt{\frac{D}{g}}$$

$T_i$ requiere el coeficiente $C_i$ de tablas API 650 Fig. E-1.

### 4. Espectro de Diseño (NCh2369)
Para cada componente, calcular el espectro con los parámetros apropiados:

| Componente | ξ | R | η |
|-----------|---|---|---|
| Impulsiva | 2% | 4 | 1.45 |
| Convectiva | 0.5% | 1.5 | 2.51 |

$$S_{a,i} = \frac{I \cdot 1.45 \cdot S_a^{bruta}(T_i)}{R_i^*}$$

$$S_{a,c} = \frac{I \cdot 2.51 \cdot S_a^{bruta}(T_c)}{R_c^*}$$

Ver: [NCh2369 — Espectro](../formulas/NCh2369-Spectrum.md)

### 5. Corte Basal y Momento Volcante

**Corte basal:**
$$V = m_i \cdot S_{a,i} \cdot g + m_c \cdot S_{a,c} \cdot g + m_{cuerpo} \cdot S_{a,i} \cdot g$$

O bien por combinación SRSS:
$$V = \sqrt{V_i^2 + V_c^2}$$

**Momento volcante en base del cuerpo:**
$$M_{base} = \sqrt{(m_i \cdot S_{a,i} \cdot g \cdot h_i)^2 + (m_c \cdot S_{a,c} \cdot g \cdot h_c)^2}$$

**Momento volcante en fundación (incluyendo presión sobre fondo):**
$$M_{fund} = \sqrt{(m_i \cdot S_{a,i} \cdot g \cdot h'_i)^2 + (m_c \cdot S_{a,c} \cdot g \cdot h'_c)^2}$$

### 6. Verificaciones

| Verificación | Criterio |
|-------------|----------|
| Estabilidad al volcamiento | $M_{resistente} / M_{volcante} \geq 1.5$ |
| Esfuerzos en cuerpo | σ compresión + σ flexión ≤ σ admisible |
| Anclaje | Si se requiere: diseño de pernos de anclaje |
| Oleaje (sloshing) | $d_s$ = altura libre al techo; verificar que no impacte |
| Conexiones | Nozzles y tuberías deben acomodar desplazamientos |

### 7. Oleaje (Sloshing Height)

$$d_s = 0.84 \cdot D \cdot S_{a,c}$$

La distancia libre entre el nivel del líquido y el techo debe ser $\geq d_s$ para evitar impacto.

## Verificaciones Finales
- [ ] Masas impulsiva + convectiva ≈ masa total del líquido
- [ ] $T_c$ en rango esperado (3–10 s para estanques típicos)
- [ ] Espectro calculado con ξ y R correctos para cada componente
- [ ] Momento volcante vs resistente verificado
- [ ] Altura de oleaje compatible con borde libre del estanque
- [ ] Esfuerzos en pared dentro de admisibles

## Fórmulas Utilizadas
- [API 650 — Sismo](../formulas/API-650-Seismic.md)
- [NCh2369 — Espectro](../formulas/NCh2369-Spectrum.md)

## Referencias
- [Principios de Diseño Sísmico](../concepts/Seismic-Design-Principles.md)
- [API 650](../standards/API-650.md)
- [NCh2369-2025](../standards/NCh2369-2025.md)

---
title: "Variantes Específicas de Chile"
type: reference
tags: [chile, normativa, NCh, variantes, comparación]
related:
  - ../standards/NCh2369-2025.md
  - ../standards/ACI-318-25.md
  - ../standards/API-650.md
  - ../concepts/Seismic-Design-Principles.md
created: 2026-04-09
updated: 2026-04-09
---

# Variantes Específicas de Chile

Documentación de diferencias entre la práctica chilena y los estándares internacionales de origen.

---

## Diseño Sísmico: NCh2369 vs ASCE 7

| Aspecto | NCh2369 (Chile) | ASCE 7 (USA) |
|---------|-----------------|---------------|
| Zonas sísmicas | 3 zonas (0.20g–0.40g) | Mapas de peligro continuo ($S_S$, $S_1$) |
| Clasificación de suelos | A–E (basada en $V_s$, similar) | A–F (basada en $V_s$, similar) |
| Forma espectral | Ecuación paramétrica con $r$, $T_0$, $p$, $q$ | Plataforma + decaimiento $1/T$ |
| Amortiguamiento base | 5% (ajustable con η) | 5% (fijo en espectro) |
| Factor de reducción período corto | $R^*$ interpolado linealmente a 1.5 en $T=0$ | $C_s$ con límites inferior/superior |

> **Diferencia clave:** NCh2369 reduce $R^*$ a 1.5 en período cero (interpolación lineal), mientras que ASCE 7 usa $C_s = S_{DS}/(R/I_e)$ con un piso mínimo. El enfoque chileno es más conservador para estructuras rígidas.

## Estanques: Práctica Chilena vs API 650

| Aspecto | Práctica Chilena | API 650 Puro |
|---------|-----------------|-------------|
| Espectro sísmico | NCh2369-2025 | ASCE 7 (Annex E) |
| Factor R impulsivo | $R_i = 4$ (NCh2369 §11.2.1.6) | $R_{wi} = 4$ (Tabla E-4) |
| Factor R convectivo | $R_c = 1.5$ (NCh2369 §11.2.1.7) | $R_{wc} = 2$ (Tabla E-4) |
| Amortiguamiento impulsivo | ξ = 2% | ξ = 5% (implícito en espectro ASCE) |
| Amortiguamiento convectivo | ξ = 0.5% | ξ = 0.5% |
| Combinación de componentes | SRSS | SRSS |

> **Diferencia clave:** La práctica chilena usa ξ = 2% para la componente impulsiva (más conservador que el 5% implícito de API/ASCE), compensando con el factor η = 1.45.

## Hormigón Armado: Práctica Chilena con ACI 318

| Aspecto | Práctica Chilena | ACI 318 Original (USA) |
|---------|-----------------|----------------------|
| Código base | ACI 318 adoptado como referencia principal | Código propio |
| Complemento sísmico | NCh433 (edificios) + NCh2369 (industrial) | ASCE 7 + ACI 318 Cap. 18 |
| Grado de acero estándar | Gr. 60 (420 MPa) — ASTM A615 | Gr. 60 o Gr. 80 |
| Concreto típico | H30–H40 (f'c = 30–40 MPa) | 4000–6000 psi (28–41 MPa) |
| Recubrimiento | Según ACI 318 Tabla 20.6.1.3 (50 mm exposición severa) | Igual |
| Unidades | SI (MPa, mm, kN) | US Customary (psi, in, kip) o SI |

> **Nota:** Chile no tiene un código propio de hormigón armado equivalente al ACI 318. Se adopta directamente la edición SI del ACI 318, complementada con las normas sísmicas nacionales.

## Particularidades de la Ingeniería Chilena

### Alta Sismicidad
- Chile es uno de los países con mayor actividad sísmica del mundo (subducción Nazca-Sudamericana)
- La práctica de diseño es inherentemente conservadora en el ámbito sísmico
- Se diseña para sismos de subducción (larga duración, muchos ciclos)

### Minería
- Sector minero domina la ingeniería industrial en Chile
- Requiere diseño para cargas extremas (camiones de 400–850 t GVW)
- Estándares internacionales (ACI, API, AISC) se combinan con normativa sísmica chilena

### Normativa Local Relevante

| Norma | Tema |
|-------|------|
| NCh433 | Diseño sísmico de edificios |
| NCh2369 | Diseño sísmico de instalaciones industriales |
| NCh3171 | Diseño estructural — disposiciones generales y combinaciones de carga |
| NCh427 | Diseño de estructuras de acero (basada en AISC) |
| NCh2745 | Análisis y diseño de edificios con aislación sísmica |

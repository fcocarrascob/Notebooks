# Guía de uso: `forallpeople`

## 1. Environments disponibles

```python
si.environment('structural',   top_level=True)  # ← el más útil para cálculo estructural
si.environment('default',      top_level=True)
si.environment('electrical',   top_level=True)
si.environment('thermal',      top_level=True)
si.environment('us_customary', top_level=True)
```

Con `top_level=True` las unidades quedan en el namespace global (`kN` en lugar de `si.kN`).

El environment `structural` expone: `mm`, `N`, `kN`, `MN`, `Pa`, `kPa`, `MPa`, `GPa`, `Nm`, `N_m`, `N_m3`, `J`, `W` + unidades imperiales (lb, kip, psi, psf, etc.).

---

## 2. Definir unidades compuestas propias

El environment no incluye todas las combinaciones (ej. `kN/m³`). Defínelas una vez en la celda de importaciones:

```python
import forallpeople as si
si.environment('structural', top_level=True)
from math import pi, sqrt, tanh, cosh, sinh

# Unidades compuestas propias
kN_m3 = kN / m**3   # peso específico  →  en lugar de escribir kN / m**3 cada vez
kN_m2 = kN / m**2   # sinónimo de kPa
kNm   = kN * m      # kilonewton·metro (momento)
```

---

## 3. Conversión con `.to()`

```python
fuerza = 250 * kN
fuerza.to('lb')   # convierte la representación a lb (debe estar en el environment)
fuerza.to()       # imprime todas las unidades compatibles disponibles en el environment
```

---

## 4. Precisión con `.round(n)`

```python
result = (3.14159 * kN).round(2)   # muestra 2 decimales
# equivalente:
round(result, 2)
```

---

## 5. `.value` — extraer el número en unidades base SI

Útil cuando se necesita pasar a funciones de `math` / `numpy` que no aceptan `Physical`:

```python
h = 8.84 * m
D  = 12  * m

rel = h / D           # → float, porque las mismas dimensiones se cancelan ✓

# sqrt de una cantidad con unidades: extraer el valor numérico explícitamente
from math import sqrt
E = 200e9 * Pa
sqrt(E.value)         # sqrt de 200e9  (en Pa)
float(E)              # devuelve el valor en la representación prefijada (200.0, en GPa)
```

---

## 6. Cálculos dimensionalmente inconsistentes (`sqrt` de unidades)

Algunas fórmulas de código (hormigón, hidráulica) aplican `sqrt` a una magnitud con unidades
como si fuera adimensional. `forallpeople` permite manejarlo así:

```python
MPa = 1e6 * Pa
f_c = 35 * MPa

# float(f_c) devuelve 35.0  (valor en la representación prefijada, MPa)
# por lo que math.sqrt actúa sobre 35, no sobre 35e6
sqrt(f_c) * MPa       # → 5.916 MPa  ✓

# Verificación:
float(f_c)            # 35.0
f_c.value             # 35000000.0  (Pa, unidad base)
sqrt(f_c)             # 5.916  (sqrt de 35)
sqrt(f_c.value)       # 5916.08  (sqrt de 35e6)
```

---

## 7. Fórmulas con dimensiones inconsistentes (caso períodos sísmicos)

Fórmulas empíricas como la del período impulsivo `T_i` mezclan unidades de forma no
estrictamente dimensional. La forma más robusta es extraer `.value` donde sea necesario:

```python
# En lugar de parchear con * s / m al final:
C_i = 1 / (sqrt(float(h_liq/D)) * (0.46 - 0.3*float(h_liq/D) + 0.067*float(h_liq/D)**2))

T_i = (C_i * float(h_liq) * sqrt(float(gamma_agua) / float(g))
       / (sqrt(float(e_pared/D)) * sqrt(float(E)) * 1000)) * s
```

---

## 8. Crear un environment personalizado (JSON propio)

Formato del archivo JSON — vector `"Dimension"` → `[kg, m, s, A, cd, K, mol]`:

```json
{
    "kN_m": {
        "Dimension": [1, 0, -2, 0, 0, 0, 0],
        "Value": 1000,
        "Symbol": "kN/m"
    },
    "kNm": {
        "Dimension": [1, 2, -2, 0, 0, 0, 0],
        "Value": 1000,
        "Symbol": "kN·m"
    },
    "kN_m3": {
        "Dimension": [1, -2, -2, 0, 0, 0, 0],
        "Value": 1000,
        "Symbol": "kN/m³"
    }
}
```

Cargarlo pasando la ruta completa:

```python
si.environment(r'C:\ruta\mi_ambiente.json', top_level=True)
```

> **Nota de seguridad:** el campo `"Factor"` admite expresiones aritméticas (`"1/0.3048"`),
> las cuales son evaluadas internamente. Solo se permiten números y operadores — sin
> caracteres alfabéticos.

---

## 9. Resumen del environment `structural`

| Variable | Unidad | Dimensión SI |
|----------|--------|--------------|
| `mm` | milímetro | m |
| `N`, `kN`, `MN` | newton, kilonewton, meganewton | kg·m/s² |
| `Pa`, `kPa`, `MPa`, `GPa` | presiones | kg/(m·s²) |
| `Nm` | Newton·metro | kg·m²/s² |
| `N_m` | Newton/metro (carga distribuida) | kg/s² |
| `N_m3` | N/m³ (peso específico) | kg/(m²·s²) |
| `lb`, `kip` | libra-fuerza, kilopound | kg·m/s² |
| `psi`, `ksi`, `psf`, `ksf` | presiones imperiales | kg/(m·s²) |
| `lbft`, `kipft` | momentos imperiales | kg·m²/s² |
| `pcf`, `kcf` | peso específico imperial | kg/(m²·s²) |

---

## 10. Propiedades y métodos útiles de `Physical`

| Propiedad / Método | Descripción |
|--------------------|-------------|
| `.value` | Valor numérico en unidades base SI (float) |
| `.factor` | Factor de escala respecto a la unidad SI |
| `.dimensions` | Vector de dimensiones `[kg, m, s, A, cd, K, mol]` |
| `.latex` | Representación en LaTeX |
| `.html` | Representación en HTML |
| `.repr` | Repr. clásica legible por máquina |
| `.round(n)` | Nueva instancia con `n` decimales en pantalla |
| `.sqrt(n)` | Raíz n-ésima |
| `.split()` | Devuelve `(valor_float, Physical_unitario)` — útil con numpy |
| `.to('unidad')` | Convierte la representación a otra unidad del environment |

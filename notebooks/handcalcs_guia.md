# Guía de uso: `handcalcs`

## 1. Instalación e importación

```python
pip install handcalcs
```

```python
# Importación mínima — registra las magics %%render y %%tex
import handcalcs.render

# Para usar el decorador en funciones propias
from handcalcs.decorator import handcalc

# Importaciones habituales en cálculo estructural
import forallpeople as si
si.environment('structural', top_level=True)
from math import pi, sqrt, sin, cos, tan, tanh, cosh, sinh
```

---

## 2. Cell magic `%%render`

Coloca `%%render` al inicio de cualquier celda de Jupyter. `handcalcs` renderiza
automáticamente cada línea como → **fórmula simbólica → sustitución numérica → resultado**.

```python
%%render
a = 2
b = 3
c = 2*a + b/3
```

Renderiza como:

$$a = 2 \quad b = 3 \quad c = 2 \cdot a + \frac{b}{3} = 2 \cdot (2) + \frac{3}{3} = 5$$

---

## 3. Override tags

Se especifican como argumento de `%%render`. Solo se puede usar **uno por celda**,
pero se puede combinar con la precisión.

### `params` — solo resultados, en columnas

Ideal para bloques de datos de entrada. Muestra los valores sin la sustitución,
organizados en 3 columnas (configurable).

```python
%%render params
r_tanque = 6 * m
e_pared  = 5 * mm
h_pared  = 10.5 * m
gamma_acero = 78.5 * kN / m**3
```

### `long` — ecuación en múltiples líneas

Fuerza el formato multilínea aunque la ecuación sea corta.

```python
%%render long
T_i = (C_i * h_liq * sqrt(gamma_agua / g)) / (sqrt(e_pared / D) * sqrt(E) * 1000) * s / m
```

### `short` — ecuación en una sola línea

Fuerza todo en una línea aunque `handcalcs` lo juzgue largo.

```python
%%render short
c = 2*a + b/3
```

### `symbolic` — solo la fórmula, sin valores

Útil para mostrar la expresión matemática pura, o como encabezado de fórmula.

```python
%%render symbolic
T_c = C_c * sqrt(D / g)
```

### Ajustar precisión decimal

Se pone el número de decimales **directamente** después de `%%render`
(se puede combinar con un override tag):

```python
%%render 4          # 4 decimales
%%render params 2   # params con 2 decimales
%%render long 5     # long con 5 decimales
```

---

## 4. Cell magic `%%tex`

Genera solo el código LaTeX sin renderizarlo. Útil para copiar a un documento `.tex`.

```python
%%tex
a = 2 / 3 * sqrt(pi)
```

Salida:
```latex
\[
\begin{aligned}
a &= \frac{ 2 }{ 3 } \cdot \sqrt{ \pi } = \frac{ 2 }{ 3 } \cdot \sqrt{ 3.142 } &= 1.182
\end{aligned}
\]
```

---

## 5. Decorador `@handcalc()` — uso en funciones

Permite usar `handcalcs` fuera de Jupyter (p.ej. en Streamlit) o encapsular
cálculos repetibles.

```python
from handcalcs.decorator import handcalc

@handcalc()
def calcular_momento(V, h):
    M = V * h
    return M

latex_str, resultado = calcular_momento(100 * kN, 5 * m)
```

Parámetros del decorador:

| Parámetro | Tipo | Descripción |
|---|---|---|
| `override` | `str` | Tag de override: `"params"`, `"long"`, `"short"`, `"symbolic"` |
| `precision` | `int` | Decimales a mostrar (default: 3) |
| `left` / `right` | `str` | Strings antes/después del bloque LaTeX (ej. `"$"`) |
| `jupyter_display` | `bool` | Si `True`, muestra directamente en Jupyter y retorna solo `locals` |
| `record` | `bool` | Activa `HandcalcsCallRecorder` para recordar iteraciones previas |

```python
@handcalc(override='params', precision=2)
def inputs(r, h, gamma):
    r_tanque    = r
    h_pared     = h
    gamma_acero = gamma
```

---

## 6. Configuración global

Afecta todas las celdas de la sesión actual.

```python
import handcalcs

handcalcs.set_option("display_precision",  4)    # decimales por defecto
handcalcs.set_option("param_columns",      4)    # columnas en modo params
handcalcs.set_option("line_break",  "\\\\[20pt]") # espaciado entre líneas
handcalcs.set_option("decimal_separator",  ",")  # coma decimal (para ES)
handcalcs.set_option("use_scientific_notation", False)

# Guardar permanentemente en config.json
handcalcs.save_config()
```

Opciones disponibles con sus valores por defecto:

| Opción | Default | Descripción |
|---|---|---|
| `decimal_separator` | `"."` | Separador decimal |
| `display_precision` | `3` | Decimales mostrados |
| `param_columns` | `3` | Columnas en modo `params` |
| `line_break` | `"\\\\[10pt]"` | Espacio vertical entre líneas |
| `underscore_subscripts` | `True` | `_` genera subíndices |
| `use_scientific_notation` | `False` | Notación científica |
| `greek_exclusions` | `[]` | Variables a excluir del reemplazo griego |
| `custom_symbols` | `{}` | Símbolos LaTeX personalizados |

---

## 7. Símbolos personalizados

Para variables cuyo nombre no se convierte automáticamente al LaTeX deseado:

```python
handcalcs.set_option("custom_symbols", {
    "V_dot":  "\\dot{V}",
    "N_star": "N^{*}",
    "phi_v":  "\\phi_v",
})
```

También se puede usar para renombrar substrings de cualquier variable:

```python
handcalcs.set_option("custom_symbols", {"star": "^*"})
# Mstar_1 → M^*_{1}
```

---

## 8. Características automáticas

### Subíndices con `_`

```python
h_liq     # → h_{liq}
M_prime_i # → M'_{i}   (si 'prime' está en el nombre)
T_0       # → T_{0}
```

### Letras griegas automáticas

El nombre de variable solo necesita **contener** el nombre griego:

| Variable Python | LaTeX generado |
|---|---|
| `gamma_acero` | $\gamma_{acero}$ |
| `omega` | $\omega$ |
| `xi_i` | $\xi_{i}$ |
| `phi` | $\phi$ |
| `Delta_T` | $\Delta_{T}$ |
| `sigma_max` | $\sigma_{max}$ |

> **Excepción:** `psi` puede colisionar con el nombre de unidad en `forallpeople`.
> Añadirlo a `greek_exclusions` si causa problemas:
> ```python
> handcalcs.set_option("greek_exclusions", ["psi"])
> ```

### Funciones matemáticas

`sqrt` → radical $\sqrt{}$, `sin`/`cos`/`tan` → funciones trigonométricas,
`min`/`max` → operadores LaTeX.

### Comentarios en línea

```python
%%render
f_c = 30 * MPa          # resistencia característica del hormigón
phi = 0.9               # factor de reducción ACI
```

El comentario aparece renderizado junto a la ecuación.

### Omitir la sustitución (wrapping en paréntesis)

```python
%%render
(rel) = h_liq / D       # solo muestra: rel = 0.737
```

### Condicionales

```python
%%render
h_i = h_liq * (0.375 if h_liq/D <= 0.75 else (0.5 - 0.09375 / (h_liq/D)))
```

Se renderiza mostrando la condición evaluada.

### Mostrar variables sin cálculo

Basta con escribir el nombre de la variable sola en la celda:

```python
%%render
h_liq
D
E
```

Muestra los valores actuales de esas variables sin ninguna operación.

---

## 9. Limitaciones importantes (Gotchas)

- **No renderiza:** `for`, `while`, `with`, definiciones de función, `list`/`dict`.
- **Usa `tuple`** (no `list`) si necesitas colecciones en fórmulas: `sum((a, b, c))`.
- **Vectores numpy** de una dimensión son compatibles.
- Cada sentencia debe estar en **una sola línea**.
- Las celdas pueden ejecutarse fuera de orden → los valores de las variables
  corresponden al último estado del namespace, no al orden visual del notebook.
  **Siempre re-ejecutar de arriba a abajo antes de reportar.**
- `//` (división entera) no se renderiza; usar `math.floor(a/b)` como alternativa.

---

## 10. Exportar a PDF / HTML sin mostrar inputs

```python
pip install "handcalcs[exporters]"
# o bien:
pip install nb-hideinputs
```

Desde Jupyter: **File → Save and Export As →**
- `HTML_NoInput`
- `LaTeX_NoInput`
- `PDF_NoInput`

Estas opciones ocultan todas las celdas de código y muestran solo las salidas renderizadas.

---

## 11. Combinación `handcalcs` + `forallpeople`

`handcalcs` fue diseñado específicamente para usarse con `forallpeople`.
Las unidades se renderizan junto al valor de forma automática:

```python
%%render params
r_tanque    = 6 * m         # → r_{tanque} = 6.000 m
e_pared     = 5 * mm        # → e_{pared}  = 5.000 mm
gamma_acero = 78.5 * kN/m**3
```

```python
%%render
PP_pared = (2 * pi * r_tanque) * e_pared * h_pared * gamma_acero
M_pared  = PP_pared / g
```

Renderiza mostrando la fórmula simbólica, luego los valores con sus unidades
y finalmente el resultado con sus unidades.

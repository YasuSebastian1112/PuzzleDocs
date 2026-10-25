La clase utilitaria `Math` provee la capa de abstracción aritmética de bajo nivel en **Puzzle Engine**. Concentra operaciones trigonométricas optimizadas (`inline noexcept`), utilidades de comparación numérica con umbrales de tolerancia, funciones de interpolación y curvas de control esenciales para cinemática, renderizado y simulación física.

  

Esta clase está declarada con constructor eliminado (`Math() = delete;`), actuando como un espacio de nombres estático sin instanciación.


---

## Trigonometría y Funciones Hiperbólicas

La clase encapsula las primitivas estándar de `<cmath>` asegurando compatibilidad `noexcept` y resolución directa sin sobrecarga de llamadas virtuales.

### Trigonometría Directa e Inversa

| `sin` | `static inline double sin(double p_x) noexcept` | Seno de $p\_x$ (en radianes). |

| `cos` | `static inline double cos(double p_x) noexcept` | Coseno de $p\_x$ (en radianes). |

| `tan` | `static inline double tan(double p_x) noexcept` | Tangente de $p\_x$ (en radianes). |

| `asin` | `static inline double asin(double p_x) noexcept` | Arcoseno principal en el rango $[-\frac{\pi}{2}, \frac{\pi}{2}]$. |

| `acos` | `static inline double acos(double p_x) noexcept` | Arcocoseno principal en el rango $[0, \pi]$. |

| `atan` | `static inline double atan(double p_x) noexcept` | Arcotangente principal en el rango $(-\frac{\pi}{2}, \frac{\pi}{2})$. |

| `atan2` | `static inline double atan2(double p_y, double p_x) noexcept` | Ángulo polar $\theta \in (-\pi, \pi]$ formado por las coordenadas $(p\_x, p\_y)$. |

### Funciones Hiperbólicas

| `sinh` / `asinh` | `sinh(p_x)` / `asinh(p_x)` | $\sinh(x) = \frac{e^x - e^{-x}}{2}$ y su inversa. |

| `cosh` / `acosh` | `cosh(p_x)` / `acosh(p_x)` | $\cosh(x) = \frac{e^x + e^{-x}}{2}$ y su inversa. |

| `tanh` / `atanh` | `tanh(p_x)` / `atanh(p_x)` | $\tanh(x) = \frac{\sinh(x)}{\cosh(x)}$ y su inversa. |

---
## Conversión Angular

Transformación bidireccional entre la escala sexagesimal tradicional y las magnitudes angulares en radianes empleadas por el pipeline trigonométrico:

```cpp

double rad = Math::deg_to_rad(180.0); // Retorna PI

double deg = Math::rad_to_deg(MathDefs::PI); // Retorna 180.0

```

* **`deg_to_rad(p_deg)`**:

  $$\text{rad} = p\_deg \cdot \frac{\pi}{180}$$

* **`rad_to_deg(p_rad)`**:

  $$\text{deg} = p\_rad \cdot \frac{180}{\pi}$$

  

---
## Aritmética Base y Módulo Euclidiano

* **`sqrt(p_x)`**: Raíz cuadrada principal $\sqrt{x}$.

* **`pow(p_x, p_y)`**: Potenciación $x^y$.

* **`log(p_x)`**: Logaritmo natural en base $e$ ($\ln x$).

* **`exp(p_x)`**: Función exponencial natural $e^x$.

* **`abs(p_x)`**: Valor absoluto $|x|$.

* **`fmod(p_x, p_y)`**: Residuo de punto flotante de la división truncada (comportamiento estándar C++).

* **`fposmod(p_x, p_y)`**: Módulo euclidiano que garantiza que el residuo conserve el signo del divisor:

  $$\text{val} \in [0, p\_y) \quad (\text{para } p\_y > 0)$$

  Evita anomalías de indexación cíclica con coordenadas o índices negativos.

---

##  Precisión y Utilidades Numéricas

Las comparaciones directas de igualdad (`==`) sobre tipos de coma flotante suelen generar falsos negativos debido a errores de redondeo en aritmética binaria IEEE 754.

### Comparación con Tolerancia

```cpp

static bool is_equal_approx(double p_a, double p_b, double p_tolerance = MathDefs::CMP_EPSILON);

static bool is_zero_approx(double p_val, double p_tolerance = MathDefs::CMP_EPSILON);

```

* **`is_equal_approx`**: Evalúa si $|p\_a - p\_b| \le p\_tolerance$.

* **`is_zero_approx`**: Evalúa si $|p\_val| \le p\_tolerance$.
* 
### Análisis de Signo y Diferencia Angular

* **`sign(p_x)`**:

  $$

  \text{sign}(x) = \begin{cases}

  1.0 & \text{si } x > 0 \\

  -1.0 & \text{si } x < 0 \\

  0.0 & \text{si } x = 0

  \end{cases}

  $$

  

* **`angle_diference(p_from, p_to)`**: Calcula la distancia angular orientada más corta entre dos orientaciones en radianes dentro del rango $[-\pi, \pi]$:

  $$\Delta\theta = \text{fmod}(2 \cdot (\text{fmod}(p\_to - p\_from, 2\pi)), 2\pi) - \text{diff}$$

---

## Curvas, Interpolación y Control de Rango

### 1. Interpolaciones Lineales y Remapeo

* **`lerp(p_from, p_to, p_weight)`**:

  $$f(t) = p\_from + (p\_to - p\_from) \cdot p\_weight$$

* **`inverse_lerp(p_from, p_to, p_val)`**: Calcula el factor normalizado $t$ que representa la posición de `p_val` dentro del intervalo:

  $$t = \frac{p\_val - p\_from}{p\_to - p\_from}$$

* **`remap(p_val, p_in_from, p_in_to, p_out_from, p_out_to)`**: Transfiere proporcionalmente un valor desde un intervalo de entrada hacia un intervalo de destino combinando `inverse_lerp` y `lerp`.

  

### 2. Rotación Suave (`lerp_angle`)

Interpola linealmente orientaciones respetando la trayectoria circular mínima calculada por `angle_diference`, evitando giros completos indeseados al cruzar la discontinuidad $-\pi \leftrightarrow \pi$.

  

### 3. Delimitación y Modulación

* **`clamp(p_val, p_min, p_max)`**: Acota el valor dentro del rango cerrado $[p\_min, p\_max]$.

* **`smoothstep(p_from, p_to, p_val)`**: Interpolación cúbica Hermite con primera derivada nula en las cotas extremas:

  $$S(t) = 3t^2 - 2t^3 \quad \text{donde } t = \text{clamp}\left(\frac{p\_val - p\_from}{p\_to - p\_from}, 0.0, 1.0\right)$$

* **`snapped(p_val, p_step)`**: Cuantiza el escalar al múltiplo exacto más próximo de `p_step`.

* **`wrapf(p_val, p_min, p_max)`**: Mapeo circular continuo dentro del intervalo $[p\_min, p\_max)$.

* **`ease(p_n, p_c)`**: Generación de curvas de aceleración y desaceleración controladas mediante el factor exponencial o sigmoide $p\_c$.

---
##  Plantillas de Utilidad (`Templates`)

Para simplificar comparaciones sobre cualquier tipo primitivo aritmético o estructurado que implemente operadores relacionales:

```cpp

template <typename T>

static constexpr T min(T a, T b);

  

template <typename T>

static constexpr T max(T a, T b);

  

template <typename T>

static constexpr T clamp(T value, T min_val, T max_val);

```

Todas las funciones plantilla están declaradas como `constexpr`, permitiendo resolución en tiempo de compilación.

---

## Relaciónes:
- [[Definiciones Matemáticas]]

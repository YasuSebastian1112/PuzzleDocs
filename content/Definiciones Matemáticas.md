El espacio de nombres `MathDefs` centraliza las constantes fundamentales y los umbrales de precisión numérica utilizados a lo largo de **Puzzle Engine**. Todas las definiciones están declaradas como `inline constexpr`, lo que asegura evaluación en tiempo de compilación, inmutabilidad y resolución directa sin coste en tiempo de ejecución ni duplicación de símbolos en los binarios.

Requiere la cabecera estándar `<limits>` para la obtención de valores límite del hardware según el estándar IEEE 754.

---
## Constantes Matemáticas Fundamentales

Valores numéricos de alta precisión con representación en punto flotante de doble precisión (`double`).

| Identificador | Valor Aproximado | Representación Teórica | Descripción |

| `PI` | `3.14159265358979323846` | $\pi$ | Relación entre la longitud de una circunferencia y su diámetro. |

| `TAU` | `6.28318530717958647692` | $\tau = 2\pi$ | Medida angular de una revolución completa en radianes. |

| `HALF_PI` | `1.57079632679489661923` | $\frac{\pi}{2}$ | Ángulo recto en radianes ($90^\circ$). |

| `SQRT2` | `1.41421356237309504880` | $\sqrt{2}$ | Raíz cuadrada de dos (diagonal del cuadrado unitario). |

| `E` | `2.71828182845904523536` | $e$ | Base de los logaritmos naturales (número de Euler). |

---
## Umbrales de Tolerancia y Comparación (Epsilon)

En cálculos con tipos de coma flotante (`float` y `double`), los errores de redondeo aritmético impiden comparaciones directas de igualdad estricta (`==`). Los siguientes valores definen los márgenes máximos admisibles de diferencia absoluta para operaciones como `Math::is_equal_approx` e `is_zero_approx`.

| `CMP_EPSILON` | `double` | $0.00001$ ($10^{-5}$) | Tolerancia estándar para operaciones en doble precisión. |

| `CMP_EPSILON2` | `double` | $\text{CMP\_EPSILON}^2 = 10^{-10}$ | Tolerancia al cuadrado para comparaciones de distancias cuadradas euclidianas sin extraer raíz. |

| `CMP_EPSILON_F` | `float` | $0.0001f$ ($10^{-4}$) | Tolerancia para operaciones con precisión simple (`float`). |

---
## Valores Límite y Sentinelas IEEE 754

Constantes obtenidas mediante `std::numeric_limits` para modelar condiciones de desbordamiento, divergencia o indeterminación numérica.


| `INF_DOUBLE` | `double` | `std::numeric_limits<double>::infinity()` | Infinito positivo en doble precisión ($+\infty$). |

| `INF_FLOAT` | `float` | `std::numeric_limits<float>::infinity()` | Infinito positivo en precisión simple ($+\infty$). |

| `NAN_DOUBLE` | `double` | `std::numeric_limits<double>::quiet_NaN()` | Valor indeterminado no numérico (Quiet NaN). |

  

---
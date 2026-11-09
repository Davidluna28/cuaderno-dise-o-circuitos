# Semana 3 (unificada) · Karnaugh, condiciones indiferentes y NAND/NOR

Reúne la **Sesión 1** (mapas de Karnaugh) y la **Sesión 2** (indiferentes y NAND/NOR).

---
# Parte A · Mapas de Karnaugh (Sesión 1)

**Objetivo:** obtener la expresión mínima en SOP y POS de funciones de 3 y 4 variables, justificando cada agrupación.

## 1. La clave: adyacencia
`X·Y + X·Y' = X`: si dos minitérminos difieren en **una sola variable**, esa variable desaparece.
Ejemplo: `m4 = AB'C'` y `m5 = AB'C` se unen en `AB'`.

## 2. ¿Por qué el orden 00, 01, 11, 10?
Es **código Gray**: entre celdas vecinas cambia un solo bit (y la última es vecina de la primera). En binario normal, 01 → 10 cambia dos bits.

## 3. Mapas
**3 variables** (`A \ BC`, columnas 00 01 11 10):
```
A\BC | 00  01  11  10
 0   |  0   1   3   2
 1   |  4   5   7   6
```
Atención: m3 va antes que m2.

**4 variables** (`AB \ CD`, ambas en Gray):
```
AB\CD | 00  01  11  10
 00   |  0   1   3   2
 01   |  4   5   7   6
 11   | 12  13  15  14
 10   |  8   9  11  10
```

## 4. Reglas para agrupar
1. **Tamaño 2ᵏ** (1, 2, 4, 8, 16 celdas); un grupo de 2ᵏ elimina k variables.
2. **Rectangular:** sin formas en L ni diagonales.
3. **Lo más grande posible:** grupos grandes dan términos más cortos.
4. **El mapa se enrolla:** bordes opuestos y esquinas son vecinos (como un toroide).

**Leer un grupo:** se conservan solo las variables que no cambian. Constante en 1 → `A`; constante en 0 → `A'`; si cambia → desaparece.

## 5. Implicantes
| Término | Significado |
|---|---|
| Implicante | cualquier grupo válido de 1 (o X) |
| Implicante primo | grupo que ya no se puede agrandar |
| Primo esencial | el único primo que cubre algún 1; debe estar en la solución |

**Solución mínima = primos esenciales + los primos mínimos necesarios para cubrir el resto.** No siempre es única.

**Procedimiento SOP mínima:** colocar 1, 0 y X → agrupar los más grandes → elegir esenciales → cubrir los 1 restantes con pocos grupos → leer un término por grupo.

## 6. Ejemplos resueltos
**Reto inicial:** `F(A,B,C) = Σm(0,2,4,5,6)`
- `C'` agrupa m0, m2, m4, m6 (A y B cambian, C = 0).
- `AB'` agrupa m4, m5 (A = 1, B = 0, C cambia).
- **`F = C' + AB'`**

**Esquinas:** los 4 extremos del mapa (m0, m2, m8, m10) dan `B'D'`.

**Ejemplo guiado:** `F = Σm(0,1,2,5,8,9,10)`
- `B'C'` → m0, m1, m8, m9 (bordes superior e inferior)
- `B'D'` → m0, m2, m8, m10 (4 esquinas)
- `A'C'D` → m1, m5
- **SOP: `F = B'C' + B'D' + A'C'D`** (3 términos, 7 literales)

**La misma función en POS** (se agrupan los 0 y se lee invertido: 0 → `A`, 1 → `A'`):
- `(A' + B')` fila AB = 11 · `(C' + D')` columna CD = 11 · `(B' + D)` B=1, D=0 (bordes izquierdo y derecho)
- **POS: `F = (A'+B')(B'+D)(C'+D')`** (6 literales)

**¿SOP o POS?** Se construyen ambas y se elige la de menor costo (literales, compuertas, CI). Aquí gana POS: 6 contra 7 literales.

---
# Parte B · Indiferentes y NAND/NOR (Sesión 2)

**Objetivo:** minimizar una función de 4 variables con condiciones indiferentes e implementarla solo con NAND, cuantificando el ahorro en compuertas y CI.

## 7. Condiciones indiferentes (X)
En BCD solo se usan 10 de las 16 combinaciones (0000–1001); 1010 a 1111 **nunca ocurren**, así que la salida ahí «es lo que más convenga».

| Uso de la X | Regla |
|---|---|
| Como 1 | si agranda un grupo, se incluye (el término queda más corto) |
| Como 0 | si no ayuda, se ignora; ninguna X está obligada a quedar cubierta |
| Nunca sola | un grupo formado solo por X agrega un término inútil |

Notación: `F = Σm(minitérminos) + d(indiferentes)`.

**Ejemplo guiado: dígito BCD ≥ 5** — `F = Σm(5,6,7,8,9) + d(10,11,12,13,14,15)`
| Caso | Expresión | Literales |
|---|---|---|
| Sin usar las X | `AB'C' + A'BD + A'BC` | 9 |
| Con las X | **`A + BD + BC`** | 5 |

## 8. Universalidad de NAND y NOR
| Función | Solo NAND | Solo NOR |
|---|---|---|
| NOT | `(A·A)'` | `(A + A)'` |
| AND | `((AB)')'` | `(A' + B')'` |
| OR | `(A'·B')'` | `((A + B)')'` |

- Si NOT, AND y OR se construyen con NAND, cualquier función también (igual con NOR).
- **SOP mínima → NAND-NAND** · **POS mínima → NOR-NOR**.

## 9. De SOP a NAND-NAND en cuatro pasos
```
1) F = A + BD + BC                     SOP mínima
2) F = ((A + BD + BC)')'               doble negación
3) F = (A' · (BD)' · (BC)')'           De Morgan sobre la negación interna
4) F = NAND( A', NAND(B,D), NAND(B,C) ) cada paréntesis es una NAND
```
- Un literal solo (como `A`) entra **complementado** (una NAND usada como inversor): por eso A va complementada y B, C, D no.
- Son dos niveles, con el mismo retardo que AND-OR. Todo sale de chips 7400 y 7410.

## 10. Costo en circuitos integrados
| Versión | Chips | CI |
|---|---|---|
| Sin usar las X | 7404 + 7411 + 7432 | 3 |
| Con X, AND-OR | 7408 + 7432 | 2 |
| Con X, solo NAND | 7400 + 7410 | 2 |

El ahorro vino de las X; la NAND no agrega chips y deja una sola familia en el inventario.


## Resumen de la semana
Gray hace visibles las adyacencias · grupos 2ᵏ rectangulares y enrollados · SOP agrupa 1, POS agrupa 0 · las X valen 1 si ayudan · NAND/NOR son universales · se compara el costo en literales y chips.


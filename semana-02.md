# Semana 2 · Álgebra de Boole y formas canónicas

**Meta:** pasar de un enunciado a la tabla de verdad, a la forma canónica y a un circuito simplificado.

## 1. Circuito = expresión
| Circuito | Expresión |
|---|---|
| Cable de entrada | Variable (A, B, C) |
| AND | Producto `·` |
| OR | Suma `+` |
| NOT | Complemento `'` |
| Salida | Valor de la expresión |

**Precedencia: NOT → AND → OR.** Ejemplo: `Y = A·B + C'`.

## 2. Del enunciado a la especificación
«La sirena suena cuando se activan dos sensores» admite dos lecturas:
- **Exactamente dos:** `F = Σm(3, 5, 6)` (con los tres activos no suena).
- **Al menos dos:** `F = Σm(3, 5, 6, 7)` (la que pide el cliente).

## 3. Postulados de Huntington (conjunto {0, 1})
| Postulado | Forma OR | Forma AND (dual) |
|---|---|---|
| Identidad | `X + 0 = X` | `X · 1 = X` |
| Complemento | `X + X' = 1` | `X · X' = 0` |
| Conmutativa | `X + Y = Y + X` | `XY = YX` |
| Distributiva | `X(Y + Z) = XY + XZ` | `X + YZ = (X + Y)(X + Z)` |

**Dualidad:** al intercambiar `+ ↔ ·` y `0 ↔ 1` (sin complementar variables), la identidad sigue siendo válida.

## 4. Teoremas (tabla de consulta)
| Teorema | Forma OR | Forma AND |
|---|---|---|
| Elemento nulo | `X + 1 = 1` | `X · 0 = 0` |
| Idempotencia | `X + X = X` | `X · X = X` |
| Involución | `(X')' = X` | |
| Absorción | `X + XY = X` | `X(X + Y) = X` |
| Absorción del complemento | `X + X'Y = X + Y` | `X(X' + Y) = XY` |
| Adyacencia | `XY + XY' = X` | `(X + Y)(X + Y') = X` |
| Consenso | `XY + X'Z + YZ = XY + X'Z` | `(X+Y)(X'+Z)(Y+Z) = (X+Y)(X'+Z)` |

**La distributiva que sorprende:** `A + BC = (A + B)(A + C)`
```
(A + B)(A + C) = AA + AC + AB + BC      distributiva
               = A + AC + AB + BC       idempotencia (AA = A)
               = A(1 + C + B) + BC      factor común A
               = A + BC                 elemento nulo
```

## 5. Leyes de De Morgan
**Rompe la barra, cambia el operador.**
- `(AB)' = A' + B'`
- `(A + B)' = A'B'`
- Generaliza: `(A + B + C)' = A'B'C'`

## 6. Minitérminos y maxitérminos
| i | A B C | mᵢ (vale 1) | Mᵢ (vale 0) |
|---|---|---|---|
| 0 | 0 0 0 | A'B'C' | A + B + C |
| 1 | 0 0 1 | A'B'C | A + B + C' |
| 2 | 0 1 0 | A'BC' | A + B' + C |
| 3 | 0 1 1 | A'BC | A + B' + C' |
| 4 | 1 0 0 | AB'C' | A' + B + C |
| 5 | 1 0 1 | AB'C | A' + B + C' |
| 6 | 1 1 0 | ABC' | A' + B' + C |
| 7 | 1 1 1 | ABC | A' + B' + C' |

- **mᵢ** es un producto: variable en 1 → sin complemento; en 0 → complementada.
- **Mᵢ** es una suma: variable en 0 → sin complemento; en 1 → complementada.
- `Mᵢ = (mᵢ)'`.

## 7. SOP y POS
Ejemplo (sirena, al menos 2 sensores):
- **SOP** (mira los 1, une con OR): `F = Σm(3, 5, 6, 7) = A'BC + AB'C + ABC' + ABC`
- **POS** (mira los 0, une con AND): `F = ΠM(0, 1, 2, 4) = (A+B+C)(A+B+C')(A+B'+C)(A'+B+C)`
- Los índices de ΠM son los que faltan en Σm.

**¿SOP o POS?** Pocos 1 → SOP. Pocos 0 → POS. Solo NAND → SOP (NAND-NAND, 7400). Solo NOR → POS (NOR-NOR, 7402). El criterio final es el costo: compuertas, circuitos integrados y niveles lógicos.

## 8. Alarma de 3 sensores (puerta A, ventana B, movimiento C)
```
F = Σm(3,5,6,7) = A'BC + AB'C + ABC' + ABC
A'BC + ABC = BC     AB'C + ABC = AC     ABC' + ABC = AB
F = AB + AC + BC          (POS: (A + B)(A + C)(B + C))
```
Se usa idempotencia (`ABC = ABC + ABC + ABC`) para que un término sirva a tres pares.

| Versión | Circuitos integrados |
|---|---|
| Canónica | 7404 + 2×7411 + 7432 = **4 CI** |
| Simplificada | 7408 + 7432 = **2 CI** |

## 9. Simplificación algebraica: procedimiento
1. Pares adyacentes: `XY + XY' = X`
2. Duplicar si hace falta: `X = X + X`
3. Absorción: `X + XY = X`
4. Absorción del complemento: `X + X'Y = X + Y`
5. Consenso: `XY + X'Z + YZ = XY + X'Z`

No hay algoritmo que garantice el mínimo: por eso llegan los mapas de Karnaugh.

**Ejemplo:** `F = A + A'B + AB'C`
```
= (A + B) + AB'C     absorción del complemento
= A + AB'C + B       conmutativa
= A + B              absorción
```
De 3 términos (5 literales) a una OR de 2 entradas.

## Resumen
Circuito = expresión · la tabla manda (dos expresiones son equivalentes si tienen la misma tabla) · la canónica siempre existe y es única · simplificar = elegir por costo.

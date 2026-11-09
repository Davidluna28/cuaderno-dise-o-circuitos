# Semana 1 · Sistemas numéricos, códigos y compuertas lógicas

**R1:** diseña e implementa circuitos combinacionales básicos con técnicas de diseño clásico.

## 1. Representación binaria
- El computador usa 0 y 1 porque un transistor es un interruptor controlado por voltaje: con dos estados es fácil distinguirlos aunque la señal traiga ruido.
- Un «1» no es un voltaje exacto, es un **rango**. Familia 74LS:

| Nivel | Salida (promete) | Entrada (exige) |
|---|---|---|
| 1 | ≥ 2,7 V | ≥ 2,0 V |
| 0 | ≤ 0,5 V | ≤ 0,8 V |
| Zona indefinida | | entre 0,8 V y 2,0 V: puede leer 0 o 1 |

- **Margen de ruido alto** = 2,7 − 2,0 = 0,7 V. **Margen de ruido bajo** = 0,8 − 0,5 = 0,3 V.
- Causa típica de lecturas erráticas: una entrada sin conectar.

## 2. Sistemas numéricos
- Sistema posicional: cada posición vale una potencia de la base.
- Binario: pesos 128, 64, 32, 16, 8, 4, 2, 1. Bit = dígito binario; byte = 8 bits.
- `1011₂ = 8 + 0 + 2 + 1 = 11`

**Decimal a binario:** dividir entre 2 y leer los restos de abajo hacia arriba. `25 = 11001₂` (comprobación: 16 + 8 + 1).

**Hexadecimal:** grupos de 4 bits desde la derecha. **Octal:** grupos de 3 bits.
`157 = 10011101₂ = 9D₁₆ = 235₈`

| Dec | 10 | 11 | 12 | 13 | 14 | 15 |
|---|---|---|---|---|---|---|
| Hex | A | B | C | D | E | F |

## 3. Aritmética binaria
Suma con acarreo: `0+0=0`, `0+1=1`, `1+1=10` (escribo 0, llevo 1), `1+1+1=11`.

```
acarreo   1 1 1
            1 0 1 1  (11)
          + 0 1 1 0  ( 6)
          ---------
          1 0 0 0 1  (17)
```

## 4. Números con signo: complemento a dos
- Un contador que retrocede desde 000 muestra 999; en binario, `0000 − 1 = 1111`. Se acuerda que `1111` significa −1.
- Con 4 bits: positivos `0000`–`0111` (0 a 7); negativos `1000`–`1111` (−8 a −1). Rango: **−8 a +7** (con 8 bits: −128 a +127).
- **Obtener el negativo:** invertir bits y sumar 1. `3 = 0011 → 1100 → 1101 = −3`.
- **Restar = sumar el negativo**, con el mismo sumador. El bit que sobra a la izquierda se descarta.

```
5 − 3 = 5 + (−3)
  0101
+ 1101
------
1 0010 → 0010 = 2 ✓
```

- **Desbordamiento (overflow):** `0111 + 0001 = 1000 → −8 ✗`. Dos positivos dan un negativo: el resultado no cabe. En Java, `int` es de 32 bits en complemento a dos: `2147483647 + 1` da `-2147483648`.

## 5. Códigos
| Código | Idea | Uso |
|---|---|---|
| **BCD** | cada dígito decimal en 4 bits (`157 → 0001 0101 0111`); 1010–1111 no se usan | displays, relojes, calculadoras |
| **Gray** | entre vecinos cambia un solo bit; método del espejo | encoders, perillas, **mapas de Karnaugh** (00, 01, 11, 10) |
| **ASCII** | tabla acordada de caracteres a números (7 bits = 128) | texto; `A = 65 = 0x41 = 01000001` |

La ñ y las tildes no están en ASCII: para eso existe UTF-8.

## 6. Compuertas lógicas
| Compuerta | Expresión | Sale 1 cuando… |
|---|---|---|
| NOT | `Y = A'` | la entrada es 0 |
| AND | `Y = A·B` | **todas** valen 1 |
| OR | `Y = A+B` | **al menos una** vale 1 |
| NAND | `Y = (A·B)'` | no todas valen 1 (0 solo si todas son 1) |
| NOR | `Y = (A+B)'` | ninguna vale 1 |
| XOR | `Y = A ⊕ B` | las entradas son **distintas** |
| XNOR | `Y = (A ⊕ B)'` | las entradas son **iguales** |

- El círculo en la salida significa «negado».
- Con solo NAND (o solo NOR) se puede construir cualquier circuito (se retoma en la semana 3).
- Ejemplo cotidiano de XOR: luz de escalera con dos interruptores.
- Normas de dibujo: ANSI (la del curso) e IEC (rectángulos).

## 7. Circuito integrado 74LS08
- Encapsulado **DIP-14**: muesca arriba, pin 1 arriba a la izquierda, se cuenta en sentido antihorario.
- 4 compuertas AND de 2 entradas. **Pin 14 = +5 V (VCC), pin 7 = tierra (GND).**
- Código impreso: `SN 74 LS 08 N` = fabricante (Texas Instruments) · rango comercial · familia LS (TTL) · función (4 AND) · encapsulado DIP plástico.
- Un `74HC08` es la versión CMOS: misma función y mismos pines.

## Resumen
Dos estados (rangos de voltaje) · bases 2, 8, 16 · complemento a dos (negativo = invertir + 1; restar = sumar el negativo) · códigos BCD, Gray y ASCII · 7 compuertas · lectura del 74LS08.

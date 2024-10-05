## Primitivos

Los tipos primitivos son los componentes más básicos. Estos son: _enteros con signo y sin signo_, _booleanos_ y _principales_.

### Enteros con signo

`int`, abreviatura de _entero (con signo)_. Son números de 128 bits que pueden ser positivos o negativos. El valor mínimo es -2^127 y el valor máximo es
2^127 - 1. Algunos ejemplos: `0`, `5000`, `-45`.

### Enteros sin signo

`uint`, abreviatura de _entero sin signo_. Son números de 128 bits que solo pueden ser positivos. Por lo tanto, el valor mínimo es 0 y el valor máximo es
2^128 - 1. **Los números enteros sin signo siempre tienen como prefijo el carácter `u`.** Algunos
ejemplos: `u0`, `u40935094534`.

Clarity tiene muchas funciones integradas que aceptan números enteros con o sin signo.

Suma:

```Clarity
(+ u2 u3)
```

Resta:

```Clarity
(- 5 10)
```

Multiplicación:

```Clarity
(* u2 u16)
```

División:

```Clarity
(/ 100 4)
```

Como ya habrás notado, los números enteros son siempre números enteros; no hay puntos decimales. Es algo que debes tener en cuenta al escribir tu código.

```Claridad
(/ u10 u3)
```

Si ingresas lo anterior en una calculadora, probablemente obtendrás `3.3333333333...`.
¡No con números enteros! La expresión anterior se evalúa como `u3`, los decimales se
eliminan.

Hay muchas más funciones que toman números enteros como entradas. Llegaremos al resto más adelante en el libro.

### Booleanos

`bool`, abreviatura de _boolean_. Un valor booleano es `true` o `false`. Se
usan para verificar si una determinada condición se cumple o no (verdadera o falsa). Algunas
funciones integradas que aceptan booleanos:

`not` (invierte un booleano):

```Clarity
(not true)
```

`and` (devuelve `true` si todas las entradas son `true`):

```Clarity
(and true true true)
```

`or` (devuelve `true` si al menos una entrada es `true`):

```Clarity
(or false true false)
```

### Principales

Un principal es un tipo especial en Clarity y representa una dirección de Stacks en
la cadena de bloques. Es un identificador único que se puede equiparar aproximadamente a una dirección de correo electrónico o un número de cuenta bancaria, ¡aunque definitivamente no es lo mismo! Es posible que también hayas oído el término _dirección de billetera_. Clarity admite dos tipos diferentes de principales: _principales estándar_ y _principales de contrato_. Los principales
estándar están respaldados por una clave privada correspondiente, mientras que los principales de contrato
apuntan a un contrato inteligente. Los principales siguen una estructura específica y siempre
comienzan con los caracteres `SP` para la red principal de Stacks y `ST` para la red de prueba
y mocknet[^1].

Un valor principal literal tiene como prefijo una comilla simple (`'`) en Clarity. Observe
que no hay comilla simple de cierre.

```Clarity
'ST1HTBVD3JG9C05J7HBJTHGR0GGW7KXW28M5JS8QE
```

Los principales de contrato son un compuesto del principal estándar que implementó el
contrato y el nombre del contrato, delimitado por un punto:

```Clarity
'ST1HTBVD3JG9C05J7HBJTHGR0GGW7KXW28M5JS8QE.my-awesome-contract
```

Usará el tipo principal a menudo al escribir Clarity. Se utiliza para comprobar
quién está llamando al contrato, registrar información sobre diferentes principales,
llamadas de funciones en todos los contratos y mucho más.

Para recuperar el saldo actual de STX de un principal, podemos pasarlo a la función
`stx-get-balance`.

```Clarity
(stx-get-balance 'ST1HTBVD3JG9C05J7HBJTHGR0GGW7KXW28M5JS8QE)
```

Ambos tipos de principales pueden tener tokens, por lo que también podemos comprobar el saldo de
un contrato.

```Clarity
(stx-get-balance 'ST1HTBVD3JG9C05J7HBJTHGR0GGW7KXW28M5JS8QE.my-contract)
```

Los saldos cero son un poco aburridos, así que enviemos algunos STX a un principal:

```Clarity,{"setup":["::mint_stx ST000000000000000000002AMW42H 1000000"]}
(stx-transfer? u500 tx-sender 'ST1HTBVD3JG9C05J7HBJTHGR0GGW7KXW28M5JS8QE)
```

---

Al conocer los primitivos y el hecho de que los tipos nunca se pueden mezclar, ahora está claro
por qué el ejemplo de la sección anterior no funciona. Dado que el primer número es
un _entero con signo_ y el siguiente es un _entero sin signo_ (observe la `u`), el
analizador rechaza el código por considerarlo inválido. En su lugar, deberíamos proporcionarle dos enteros con signo o dos enteros sin signo.

Incorrecto:

```Clarity
(+ 2 u3)
```

Correcto:

```Clarity
(+ u2 u3)
```

[^1]: Más información sobre los diferentes tipos de redes en un capítulo posterior.

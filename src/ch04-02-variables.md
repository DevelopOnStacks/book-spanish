## Variables

Las variables son miembros de datos que pueden cambiarse con el tiempo. Solo son
modificables por el contrato inteligente actual. Las variables tienen un tipo predefinido y
un valor inicial.

```Clarity,{"nonplayable":true}
(define-data-var var-name var-type initial-value)
```

Donde `var-type` es una _firma de tipo_ y `initial-value` un valor válido para
el tipo especificado. Aunque puede nombrar una variable prácticamente de cualquier manera,
debe tener en cuenta las [palabras clave integradas](ch03-00-keywords.md). No use
palabras clave como nombres de variables.

Las variables se pueden leer usando la función `var-get` y se pueden cambiar usando `var-set`.

```Clarity
;; Define una variable de datos entera sin signo con un valor inicial de u0.
(define-data-var my-number uint u0)

;; Imprime el valor inicial.
(print (var-get my-number))

;; Cambia el valor.
(var-set my-number u5000)

;; Imprime el nuevo valor.
(print (var-get my-number))
```

¿Observas el `uint`? Esa es la firma del tipo.

## Firmas de tipo

El [capítulo sobre tipos](ch02-00-types.md) cubrió cómo expresar un valor de un
tipo específico. Las firmas de tipo, por otro lado, definen el tipo admitido para un
argumento de variable o función. Veamos cómo se ven las firmas.

| Tipo | Firma |
| ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [Entero con signo](ch02-01-primitive-types.md#signed-integers) | `int` |
| [Entero sin signo](ch02-01-primitive-types.md#unsigned-integers) | `uint` |
| [Booleano](ch02-01-primitive-types.md#booleans) | `bool` |
| [Principal](ch02-01-primitive-types.md#principals) | `principal` |
| [Búfer](ch02-02-sequence-types.md#buffers) | `(buff max-len)`, donde `max-len` es un número que define la longitud máxima. |
| [Cadena ASCII](ch02-02-sequence-types.md#strings) | `(string-ascii max-len)`, donde `max-len` es un número que define la longitud máxima. |
| [Cadena UTF-8](ch02-02-sequence-types.md#strings) | `(string-utf8 max-len)`, donde `max-len` es un número que define la longitud máxima. |
| [Lista](ch02-02-sequence-types.md#lists) | `(list max-len element-type)`, donde `max-len` es un número que define la longitud máxima y `element-type` una firma de tipo. Ejemplo: `(list 10 principal)`. |
| [Opcional](ch02-03-composite-types.md#optionals) | `(optional some-type)`, donde `some-type` es una firma de tipo. Ejemplo: `(optional principal)`. |
| [Tupla](ch02-03-composite-types.md#tuples) | `{key1: entry-type, key2: entry-type}`, donde `entry-type` es una firma de tipo. Cada clave puede tener su propio tipo. Ejemplo: `{sender: principal, amount: uint}`. |
| [Respuesta](ch02-03-composite-types.md#responses) | `(response ok-type err-type)`, donde `ok-type` es el tipo de los valores `ok` devueltos y `err-type` es el tipo de los valores `err` devueltos. Ejemplo: `(response bool uint)`. |

Podemos ver que algunos tipos indican una _longitud máxima_. No hace falta decir
que la longitud se aplica estrictamente. Pasar un valor demasiado largo
resultará en un error de análisis. Intente cambiar el siguiente ejemplo haciendo que la cadena
`"Esto funciona."` sea demasiado larga.

```Clarity
(define-data-var message (string-ascii 15) "Esto funciona").
```

Al igual que otros tipos de declaraciones de definición, `define-data-var` solo se puede usar en
el nivel superior de una definición de contrato inteligente; es decir, no puede colocar una declaración de
definición en el medio del cuerpo de una función.

Recuerde que se pueden usar espacios en blanco para hacer que su código sea más legible. Si está
definiendo un tipo de tupla complicado, simplemente espacielo:

```Clarity
(define-data-var high-score
  ;; Definición del tipo de tupla:
  {
    score: uint,
    who: (principal opcional),
    at-height: uint
  }
  ;; Valor de la tupla:
  {
    score: u0,
    who: none,
    at-height: u0
  }
)
;; Imprimir el valor inicial.

  (print (var-get high-score))

;; Cambiar el valor.

(var-set high-score
  {score: u10, who: (some tx-sender), at-height: block-height}
)

;; Imprimir el nuevo valor.

(print (var-get high-score))
```

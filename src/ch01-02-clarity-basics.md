## Conceptos básicos de Clarity

Clarity tiene una sintaxis _similar a la de LISP_. Eso significa que verá muchos
paréntesis. Dentro de estos paréntesis encontrará símbolos, frases, valores y más paréntesis. Las personas que son nuevas en lenguajes similares a LISP pueden sentirse
intimidadas, ya que parece muy extraño en comparación con los lenguajes naturales y muchos otros lenguajes de programación.

Una forma de conceptualizar el código Clarity es pensar en listas dentro de listas. (El
término técnico para este tipo de expresiones es
"[S-expression](https://es.wikipedia.org/wiki/Expresi%C3%B3n_S)".) Las expresiones
no diferencian entre datos y código, ¡lo que hace las cosas un poco más fáciles! Aquí hay
un ejemplo de una expresión Clarity (pista: haga clic en el botón _reproducir_ o cópielo y péguelo en el REPL):

```Clarity
(+ 4 5)
```

¿Adivinó correctamente a qué se evalúa la expresión? Aunque pueda parecer un poco extraño para aquellos que no están acostumbrados a la
_[notación polaca](https://es.wikipedia.org/wiki/Notaci%C3%B3n_polaca)_ en matemáticas, es
fácil hacer una suposición razonable. El símbolo `+` define una operación (en
este caso, la suma), y el `4` y el `5` son entradas. El resultado final: `9`.

Estas expresiones siempre siguen el mismo patrón básico: una llave de apertura
`(`, seguida de un _nombre de función_, opcionalmente seguida de una cantidad de _parámetros de entrada_, y cerrada por una llave de cierre `)`. Las diferentes partes dentro de las llaves están separadas por espacios en blanco.

El símbolo `+` en el ejemplo anterior realmente no tiene un significado especial. Es solo un nombre de función. Aquí hay otro ejemplo:

```Clarity
(concat "Hello" " World!")
```

¿Está empezando a tener sentido? `concat` es el nombre de la función y se proporciona
con dos [cadenas](ch02-02-sequence-types.md#strings) como entradas. "Concat" es
la abreviatura de _concatenate_. Por lo tanto: une dos cadenas para formar el clásico
`"¡Hola mundo!"`.

En este punto, es posible que pueda intuir cómo se combinan expresiones más complejas. Tome el siguiente cálculo como ejemplo: `4 × (15 + 10)`. En Clarity,
se ve así:

```Clarity
(* 4 (+ 15 10))
```

Lo anterior todavía es bastante legible, pero es obvio que las expresiones con
múltiples niveles de anidamiento se vuelven difíciles de leer. Los espacios en blanco solo se usan para
delinear símbolos. Esto significa que podemos usar tabulaciones, espacios y nuevas líneas para hacer que el código de Clarity sea más legible.

```Clarity
(*
(+ 2 3)
(- 6 1)
)
```

A medida que comience a escribir más código Clarity, descubrirá qué formato es
más cómodo para usted. Observar cómo otros desarrolladores formatean su código también ayuda.

Finalmente, es posible agregar comentarios de forma libre a su código. ¡Nunca sea parco en comentarios, especialmente si está trabajando en un proyecto con varias
personas! Los comentarios tienen como prefijo un doble punto y coma (`;;`) y pueden aparecer en su propia línea o después de una expresión. También son útiles para desactivar temporalmente algún código durante el proceso de desarrollo.

```Clarity
;; Un comentario en su propia línea.

(+ 1 2) ;; El resultado será 3.

;; La siguiente línea no se evalúa:
;; (+ 4 5)
```

Recuerde que los contratos Clarity se [consolidan](https://es.wikipedia.org/wiki/Commit) a la cadena tal como están. Por lo tanto, los comentarios
aumenta el tamaño del contrato y cualquier elemento _creativo_ que incluyas
será visible para cualquier persona que inspeccione el código.

```Clarity{"expected_output": "15", "hint": "Convierte el siguiente cálculo en código Clarity:\n(5 * 4) - 5"}

```

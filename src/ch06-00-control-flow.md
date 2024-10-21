# Flujo de control y manejo de errores

Errores, ¿no sería fantástico si los contratos inteligentes no tuvieran errores? El manejo de errores
en Clarity sigue un paradigma muy sencillo. Ya hemos visto que
devolver un `err` desde una [función pública](ch05-01-public-functions.md)
activa una reversión. Eso es bastante significativo, pero comprender el _flujo de
control_ de su contrato inteligente es aún más importante.

¿Qué es el flujo de control? En pocas palabras, es el orden en el que se
evalúan las expresiones. Las funciones presentadas hasta este punto permiten seguir una regla simple de
izquierda a derecha. `begin` lo ilustra perfectamente:

```Clarity
(begin
(print "First")
(print "Second")
(print "Third")
)
```

La primera expresión de impresión se evalúa primero, la segunda después, y así sucesivamente.
Pero hay algunas funciones que realmente influyen en el flujo de control. Estas
se denominan acertadamente _funciones de flujo de control_. Si comprender
[respuestas](ch05-01-public-functions.md) es clave para convertirse en un desarrollador
exitoso de contratos inteligentes, entonces comprender las funciones de control es clave para convertirse en un gran desarrollador de contratos inteligentes. Los nombres de las funciones de flujo de control son:
`asserts!`, `try!`, `unwrap!`, `unwrap-err!`, `unwrap-panic` y
`unwrap-err-panic`.

Hasta ahora, usábamos expresiones `if` para devolver una respuesta `ok` o `err`. Recuerde la parte de retorno de la función `count-even` en el capítulo
sobre [función pública](ch05-01-public-functions.md):

```Clarity,{"nonplayable":true}
(if (is-eq (mod number u2) u0)
(ok "the number is even")
(err "the number is uneven")
)
```

Se puede argumentar que la estructura todavía es bastante legible, pero imagine que necesita
varios condicionales que devuelvan un código de error diferente en caso de falla. ¡Rápidamente terminará con construcciones que ningún desarrollador sensato puede
entender fácilmente! Las funciones de flujo de control son absolutamente necesarias para producir código legible una vez que sus contratos se vuelven más complejos. Le permiten crear
cortocircuitos que devuelven inmediatamente un valor de una función, terminando la ejecución
antes de tiempo y salteando así cualquier expresión que pudiera haber venido después.

Otra cosa útil para entender sobre las funciones de flujo de control es la
diferencia entre las funciones que terminan en un signo de exclamación (como `unwrap!`),
y las que no lo hacen (como `unwrap-panic`). Las que terminan en un signo de exclamación
permiten retornos anticipados arbitrarios de una función. Las que no lo hacen
finalizan la ejecución por completo y arrojan un error de tiempo de ejecución.

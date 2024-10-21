## Funciones públicas

Las funciones públicas pueden ser invocadas desde el exterior tanto por los principales estándar como por los principales
de contrato. Los contratos que presentan alguna interactividad necesitarán al menos
una función pública. El hecho de que puedan ser invocadas no implica que la
_funcionalidad subyacente_ sea pública. El desarrollador puede incluir afirmaciones para
asegurarse de que solo [los llamadores de contrato](ch03-00-keywords.md) o las entradas
sean válidas.

Las funciones públicas _**deben**_ devolver un valor
[tipo de respuesta](ch02-03-composite-types.md#responses). Si la función
devuelve un tipo `ok`, entonces la llamada de función se considera válida y cualquier cambio
realizado en el estado de la cadena de bloques se materializará. Esto significa que los cambios de estado, como actualizar variables o transferir tokens _**solo**_ se confirmarán en la cadena si la llamada de contrato que activó estos cambios
devuelve un `ok`.

Los efectos de devolver un `ok` o un `err` se ilustran con el ejemplo
a continuación. Es una función básica que toma un entero sin signo como entrada y
devolverá un `ok` si es par o un `err` si es impar. También
incrementará una variable llamada `even-values` al comienzo de la función. Para verificar
si el número es par, calculamos el resto de una división por dos y verificamos
que sea igual a cero usando la función `is-eq` (_n mod 2 debe ser igual a 0_).

```Clarity
(define-data-var even-values ​​uint u0)

(define-public (count-even (number uint))
  (begin
      ;; incrementa la variable "event-values" en uno.
      (var-set even-values ​​(+ (var-get even-values) u1))
  
      ;; verifica si el número de entrada es par (número mod 2 es igual a 0).
      (if (is-eq (mod number u2) u0)
        (ok "el número es par")
        (err "el número es impar")
      )
  )
)

  ;; Llama a count-even dos veces.
  (print (count-even u4))
  (print (count-even u7))

  ;; ¿Esto devolverá u1 o u2?
  (print (var-get even-values))
```

¿Notaste cómo la expresión de impresión final devolvió `u1` y no `u2`, a pesar de que la función `count-even` se llama dos veces? Si estás acostumbrado a programar
en un lenguaje diferente, por ejemplo JavaScript, entonces puede que te parezca
extraño: la variable `even-values` se actualiza al inicio de la función, por lo que parece intuitivo que el número debería incrementarse en cada llamada de función.

Edita el ejemplo anterior e intenta realizar las siguientes iteraciones:

- Reemplaza el número impar `u7` con un número par como `u8`. ¿La
impresión de `even-values` contiene `u1` o `u2`?

- Cambia el tipo `err` en la expresión `if` a `ok`. ¿Cuál es el valor de `even-values` entonces?

Lo que sucede es que toda la llamada de función pública se revierte o
_revierte_ tan pronto como devuelve un valor `err`. ¡Es como si la llamada a la función
nunca hubiera ocurrido! Por lo tanto, no importa si actualiza la variable al
inicio o al final de la función.

Comprender las respuestas y cómo pueden afectar el estado de la cadena es clave para ser un desarrollador de contratos inteligentes exitoso. El capítulo sobre manejo de errores brindará
más detalles sobre cómo proteger sus funciones y el flujo de control exacto.

```Claridad,{"validation_code": "(asserts! (is-eq (sum-three u3 u5 u7) (ok u15)) \"Eso no parece correcto, inténtalo de nuevo...\")\n(asserts! (is-eq (sum-three u20 u30 u40) (ok u90)) \"¡Ya casi está, inténtalo de nuevo!\")", "hint": "Escribe una función llamada 'sum-three' que sume 3 enteros sin signo".}
(define-public (sum-three))
  (print (sum-three u3 u5 u9))
```

## asserts! (¡afirma!)

La función `asserts!` toma dos parámetros, el primero es una expresión booleana
y el segundo un valor llamado _throw_. Si la expresión booleana
se evalúa como `true`, entonces `asserts!` devuelve `true` y la ejecución continúa como se espera, pero si la expresión se evalúa como `false`, entonces `asserts!` devolverá
el valor de lanzamiento y _saldrá del flujo de control actual_.

Eso suena complicado, así que veamos algunos ejemplos. Tenga en cuenta
que la forma básica de `asserts!` como se describe se ve así:

```Clarity,{"nonplayable":true}
	(asserts! boolean-expression throw-value)
```

Se dice que la siguiente afirmación _pasa_, ya que la expresión booleana se evalúa
como `true`.

```Clarity
	(asserts! true (err "fallo"))
```

Se dice que el siguiente _fail_, ya que la expresión booleana evalúa como `false`.

```Clarity
	(asserts! false (err "fallo"))
```

¿Observa cómo en algún lugar de ese mensaje de error encontramos `(err "failed")`? Hagámoslo
más claro con una función de prueba. La función de prueba toma un valor de entrada booleano y afirma su veracidad. Para un valor de lanzamiento utilizaremos un `err` y la expresión
final devolverá un `ok`.

```Clarity
(define-public (asserts-example (input bool))
	(begin
		(asserts! input (err "the assertion failed"))
		(ok "end of the function")
	)
)

(print (asserts-example true))
(print (asserts-example false))
```

La primera impresión nos da el `ok` como se ve al final de la expresión `begin`.
Nada demasiado extraño allí. ¡Pero la segunda llamada nos da el valor de lanzamiento `err`!

Aunque la función `begin` nos da el resultado de la expresión final
_en circunstancias normales_, la función de control `asserts!` tiene la capacidad de
_anular ese comportamiento y salir del flujo actual_. Cuando `asserts!` falla,
hace cortocircuito y devuelve el valor de lanzamiento de la función inmediatamente. Esto
hace que `asserts!` sea realmente útil para crear guardias al _afirmar_ (de ahí el
nombre) que ciertos valores son lo que espera que sean.

¿Recuerda la función `is-valid-caller` en el
[capítulo sobre funciones privadas](ch05-02-private-functions.md)? El ejemplo
usó una función `if` para permitir la acción solo si el `contract-caller` era igual al
principal que implementó el contrato. Ahora reescribamos ese contrato para usar
`asserts!` en su lugar:

```Clarity
(define-constant contract-owner tx-sender)

;; Intente eliminar la constante contract-owner anterior y usar una
;; diferente para ver el error de llamadas de ejemplo:
;; (define-constant contract-owner 'ST20ATRN26N9P05V2F1RHFRV24X8C8M3W54E427B2)

(define-constant err-invalid-caller (err u1))

(define-map receivers principal uint)

(define-private (is-valid-caller)
	(is-eq contract-owner contract-caller)
)

(define-public (add-recipient (recipient principal) (amount uint))
(begin
	;; Confirma que el llamador del contrato es válido.
	(asserts! (is-valid-caller) err-invalid-caller)
	(ok (map-set receivers receiver amount))
)
)

(define-public (delete-recipient (recipient principal))
(begin
	;; Confirma que el llamador del contrato es válido. válido.
	(asserts! (is-valid-caller) err-invalid-caller)
	(ok (map-delete receivers receiver))
)
)

;; Dos ejemplos de llamadas a las funciones públicas:
(print (add-recipient 'ST1J4G6RR643BCG8G8SR6M2D9Z9KXT2NJDRK3FBTK u500))
(print (delete-recipient 'ST1J4G6RR643BCG8G8SR6M2D9Z9KXT2NJDRK3FBTK))
```

Eso parece mucho más legible. Si no está de acuerdo, espere hasta llegar al
capítulo sobre las mejores prácticas.

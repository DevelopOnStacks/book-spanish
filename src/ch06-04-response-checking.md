## Comprobación de respuesta

Ya aprendimos que la
[respuesta devuelta por una función pública](ch05-01-public-functions.md) determina
si un cambio de estado se materializa o no en la cadena. Esto no solo es válido
para la llamada inicial del contrato, sino también para las llamadas posteriores. Si un principal
estándar llama a _Contract A_, que a su vez llama a _Contract B_, entonces
la respuesta de _Contract B_ solo influirá en el estado interno de _Contract
B_. Es decir, si _Contract B_ devuelve un `err`, entonces cualquier modificación a
sus propios miembros de datos se revierte, pero _Contract A_ aún puede modificar sus propios
miembros de datos y hacer que se materialicen si devuelve un `ok`. Sin embargo,
el primer contrato en la llamada sigue teniendo el control final. Si devuelve un
`err`, entonces nada más adelante se materializará.

**Por lo tanto, la confirmación o reversión de los cambios se determina de manera secuencial.**

Esto significa que, en una cadena de llamadas de varios contratos, el contrato que realiza la llamada sabe con
absoluta certeza que una llamada secundaria no se materializará en la cadena si devuelve
una respuesta `err`. No obstante, un contrato puede depender del éxito de la llamada secundaria. Por ejemplo, un contrato de billetera está llamando a un contrato de token
para transferir un token. Sucede con demasiada frecuencia que los desarrolladores se olvidan de verificar
el valor de retorno. Para protegerse contra tales errores, Clarity prohíbe que las respuestas intermedias
queden sin verificar. Una respuesta intermedia es una respuesta que,
si bien es parte de una expresión, no es la que se devuelve. Podemos ilustrarlo
con la función `begin`:

```Clarity
(begin
	true ;; esto es un booleano, por lo que está bien.
	(err false) ;; esto es una *respuesta intermedia*.
	(ok true) ;; esta es la respuesta devuelta por el begin.
)
```

Al ejecutar el fragmento, se devuelve el siguiente error:

> Error de análisis: las respuestas intermedias en declaraciones consecutivas deben
> comprobarse

Dado que las respuestas están pensadas para indicar el éxito o el fracaso de una acción, no pueden dejarse pendientes. _Comprobar_ la respuesta simplemente significa lidiar con ella;
es decir, desenvolverla o propagarla. Por lo tanto, podemos poner un `try!` u
otra función de flujo de control a su alrededor para corregir el código.

Cualquier llamada de función que devuelva una respuesta debe ser devuelta por la función que llama o comprobada. Por lo general, esto toma la forma de una llamada de función
intercontractual, pero algunas funciones integradas también devuelven un tipo de respuesta. La función
`stx-transfer?` es una de ellas.

Veremos cómo se ve este comportamiento con el siguiente contrato de depósito.
Contiene una función que permite a un usuario depositar STX y hará un seguimiento de los
depósitos de usuarios individuales. La función no es válida debido a una respuesta intermedia no verificada. Su desafío es _intentar_ localizarla y solucionarla.

```Clarity,{"setup":["::mint_stx ST000000000000000000002AMW42H 10000000"]}
(define-map deposits principal uint)

(define-read-only (get-total-deposit (who principal))
(default-to u0 (map-get? deposits who))
)

(define-public (deposit (amount uint))
	(begin
		(stx-transfer? amount tx-sender (as-contract tx-sender))
		(map-set deposits tx-sender (+ (get-total-deposit tx-sender) amount))
		(ok true)
	)
)

;; Prueba un depósito de prueba
(print (deposit u500))
```

¿Lo resolviste? El análisis nos da una pista, indicando que la expresión `begin`
contiene una respuesta intermedia. En este caso, es el valor de retorno de `stx-transfer?`. La forma más fácil de comprobar la respuesta es simplemente
envolviendo la transferencia en un `try!`. De esa manera, el resultado de la transferencia
se desenvuelve si es exitosa y se propaga si es un `err`. Por lo tanto, reescribimos
la línea de la siguiente manera:

```Clarity,{"nonplayable":true}
(try! (stx-transfer? amount tx-sender (as-contract tx-sender)))
```

También podríamos haber resuelto el problema desenvolviendo la respuesta y usando
declaraciones condicionales. Sin embargo, una estructura de este tipo habría hecho que la función fuera mucho más complicada y, honestamente, complicaría demasiado las cosas. El arte de escribir contratos
inteligentes es crear un flujo de aplicación sencillo, logrando la funcionalidad deseada con la menor cantidad de código.

La nueva función se ve bien, pero en realidad es posible simplificarla
aún más:

```Clarity,{"nonplayable":true}
(define-public (deposit (amount uint))
	(begin
		(map-set deposits tx-sender (+ (get-total-deposit tx-sender) amount))
		(stx-transfer? amount tx-sender (as-contract tx-sender))
	)
)
```

Esta implementación es funcionalmente equivalente. `stx-transfer?` devuelve una
respuesta de tipo `(response bool uint)`, por lo tanto no necesitamos reiterar
nuestro propio `(ok true)` al final de `begin`. Al mover la expresión de transferencia
a la última línea, su respuesta ya no es intermediaria y será devuelta desde la función `deposit`, ya sea `ok` o `err`.

El capítulo sobre [mejores prácticas](ch14-00-best-practices.md) le enseñará algunas
técnicas sobre cómo detectar código que se puede simplificar de la misma manera.

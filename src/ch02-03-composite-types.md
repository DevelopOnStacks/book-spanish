## Tipos compuestos

Son tipos más complejos que contienen varios otros tipos. Los compuestos
facilita mucho la creación de contratos inteligentes más grandes.

### Opcionales

El sistema de tipos en Clarity no permite valores vacíos. Esto significa que un
booleano siempre es `true` o `false`, y un entero siempre contiene un
número. Pero a veces desea poder expresar una variable que podría tener
_algún_ valor o _nada_. Para esto, se utiliza el tipo `opcional`. Este tipo
envuelve un tipo diferente y puede ser `none` o un valor de ese tipo. El
tipo opcional es muy poderoso y las herramientas realizarán verificaciones para asegurarse de que se gestionen correctamente en el código. Veamos algunos ejemplos.

Envolviendo un `uint`:

```Clarity
(some u5)
```

Una cadena ASCII:

```Clarity
(some "Un opcional que contiene una cadena").
```

O incluso un principal:

```Clarity
(some 'ST1HTBVD3JG9C05J7HBJTHGR0GGW7KXW28M5JS8QE)
```

_Nothing_ se representa con la palabra clave `none`:

```Clarity
none
```

Las funciones que pueden o no devolver un valor tienden a devolver un tipo opcional. Como vimos en la sección anterior, tanto `element-at` como `index-of`
devolvieron un `(some ...)`. Esto se debe a que para algunas entradas, no se puede encontrar un valor coincidente. Podemos tomar la misma lista, pero esta vez intentar recuperar un elemento en un índice _más grande_ que el tamaño total de la lista. Vemos que da como resultado un valor
`none`.

```Clarity
(element-at (list 4 8 15 16 23 42) u5000)
```

Al escribir contratos inteligentes, el desarrollador debe manejar los casos en los que `(some ...)`
se devuelve de manera diferente a cuando se devuelve `none`.

Para acceder al valor contenido dentro de un opcional, debe _desenvolverlo_.

```Clarity
(unwrap-panic (some u10))
```

Intentar desenvolver un `none` dará como resultado un error porque no hay nada que
desenvolver. El _"panic"_ en `unwrap-panic` debería delatar eso.

```Clarity
(unwrap-panic none)
```

En capítulos posteriores sobre el manejo de errores y la definición de funciones personalizadas se profundizará en
cómo lidiar con dichos errores y qué efectos tienen en el estado de la cadena.

### Tuplas

Las tuplas son registros que contienen múltiples valores en campos nombrados. Cada campo tiene su
propio tipo, lo que hace que sea muy útil pasar datos estructurados de una sola vez. Las tuplas
tienen su propio formato especial y usan llaves.

```Clarity
{
id: u5, ;; un uint
username: "ClarityIsAwesome", ;; una cadena ASCI
address: 'ST1HTBVD3JG9C05J7HBJTHGR0GGW7KXW28M5JS8QE ;; y un principal
}
```

Los miembros dentro de las tuplas no están ordenados. Los recupera por nombre y no puede
iterarlo. Un miembro específico puede leerse utilizando la función `get`.

```Clarity
(get username { id: 5, username: "ClarityIsAwesome" })
```

Las tuplas, al igual que otros valores, son inmutables una vez definidas. Esto significa que no pueden
cambiarse. Sin embargo, puede fusionar dos tuplas para formar una nueva tupla.
La fusión se realiza de izquierda a derecha y sobrescribirá los valores con la misma clave.

```Clarity
(merge
{id: 5, score: 10, username: "ClarityIsAwesome"}
{score: 20, winner: true}
)
```

La expresión anterior dará como resultado una tupla con ambas claves fusionadas y el puntaje
establecido en `20`.

Dado que la función de fusión devuelve una tupla _completamente nueva_, los tipos de miembros pueden, de hecho, sobrescribirse con una tupla posterior en la secuencia.

```Clarity
(merge
{id: u6, puntuación: 10}
{score: u50}
)
```

### Respuestas

Una respuesta es un tipo compuesto que envuelve a otro tipo como si fuera un opcional.
Sin embargo, lo que es diferente es que un tipo de respuesta incluye una indicación de
si una acción específica fue exitosa o fallida. Las respuestas tienen efectos
especiales cuando son devueltas por funciones públicas. Trataremos esos efectos en el
[capítulo sobre funciones](ch04-01-public-functions.md).

Una respuesta toma la forma concreta de `(ok ...)` o `(err ...)`. Envolver
un valor en una respuesta concreta es sencillo:

```Clarity
(ok true)
```

Los desarrolladores generalmente crean sus propias reglas para indicar el estado de error. Por ejemplo, puede usar números enteros sin signo para representar un código de error específico.

```Clarity
(err u5) ;; Algo salió mal.
```

No existen reglas explícitas sobre qué tipos debe encapsular para sus respuestas.
Actualmente se están proponiendo estándares y abordaremos algunos en el
capítulo sobre _Propuestas de mejora de pilas_ (SIP).

Las respuestas se pueden desencapsular de la misma manera que los tipos opcionales:

```Clarity
(unwrap-panic (ok true))
```

Aunque no es necesario, las funciones privadas y las funciones de solo lectura también pueden
devolver un tipo de respuesta.

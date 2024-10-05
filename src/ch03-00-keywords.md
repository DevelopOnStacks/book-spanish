# Palabras clave

Las palabras clave son términos especiales que tienen un significado asignado. Ya nos encontramos
con algunas palabras clave en los capítulos anteriores: `true`, `false` y `none`. Hay
algunas otras que exigen atención adicional.

## block-height

`discountinuada en clarity 3, vea tenure-height`

Refleja la altura de bloque actual de la cadena de bloques Stacks como un entero
sin signo. Si imaginamos que la punta de la cadena está en la altura 5, podemos leer ese número
en cualquier punto de nuestro código.

Implementado en: `Clarity 1`

```Clarity,{"setup":["::advance_chain_tip 5"]}
(> block-height 1000) ;; devuelve verdadero si la altura de bloque actual ha superado los 1000 bloques.
```

## burn-block-height

Refleja la altura de bloque actual de la cadena de bloques de quema subyacente (en este
caso, Bitcoin) como un entero sin signo, retorna un `uint` número entero.

Implementado en: `Clarity 1`

```Clarity
(> burn-block-height 1000) ;; devuelve verdadero si la altura actual de la cadena de bloques de quema subyacente ha superado los 1000 bloques.
```

## tx-sender

Contiene el principal que envió la transacción. Se puede utilizar para validar el principal que está llamando a una función pública.

Implementado en: `Clarity 1`

```Clarity
tx-sender
```

Tenga en cuenta que es posible que `tx-sender` sea un principal de contrato si se utilizó la
función especial `as-contract` para cambiar el contexto de envío.

```Clarity
(print tx-sender) ;; Imprimirá una dirección de Stacks del remitente de la transacción
```

Tenga en cuenta que el uso de `tx-sender` como verificación de permiso para llamar a un contrato puede exponerlo a una vulnerabilidad en la que un contrato malicioso podría engañar a un usuario para que lo llame en lugar del contrato previsto, pero la verificación de `tx-sender` pasaría, ya que devuelve el llamador del contrato original.

Por ejemplo, creo que estoy llamando al contrato A, pero estoy siendo manipulado socialmente para llamar al contrato b en su lugar. El contrato b llama al contrato a pero pasa parámetros diferentes. Cualquier verificación de permiso en el contrato A pasará ya que soy el `tx-sender` original.

Por este motivo, se recomienda utilizar en su lugar `contract-caller`, que se describe a continuación.

## contract-caller

Contiene el principal que llamó a la función. Puede ser un principal estándar
o un principal de contrato. Si el contrato se llama a través de una transacción firmada
directamente, entonces `tx-sender` y `contract-caller` serán iguales. Si el contrato
llama a otro contrato a su vez, entonces `contract-caller` será igual al contrato
anterior en la cadena.

Implementado en: `Clarity 1`

```Clarity
(print contract-caller) ;; Imprimirá una dirección de Stacks del remitente de la transacción
```

En el ejemplo anterior, el contrato A no sería vulnerable a este exploit, ya que una verificación de permisos utilizando `contract-caller` daría como resultado que el contrato malicioso no pasara la verificación de permisos.

## stack-block-height

Refleja la altura de bloque actual de la cadena de bloques Stacks como un entero
sin signo. Si imaginamos que la punta de la cadena está en la altura 5, podemos leer ese número
en cualquier punto de nuestro código, retorna un `uint` número entero.

Implementado en: `Clarity 3`

```Clarity,{"setup":["::advance_chain_tip 5"]}
(print stacks-block-height) ;; Imprimirá la altura actual del bloque de pilas
```
## tenure-height

Devuelve la cantidad de periodos que han transcurrido, es igual a la longitud de la cadena, retorna un `uint` número entero.

Implementado en: `Clarity 3`

```Clarity,{"setup":["::advance_chain_tip 5"]}
(print tenure-height)) ;; Imprime la altura de perido actual
```

`block-height` seguirá siendo compatible con los contratos Clarity 1 y Clarity 2 existentes, ya que devolverá el mismo valor que `tenure-height`. El uso de `block-height` en un contrato Clarity 3+ activará un error de análisis.

No se preocupe si esto no está completamente claro ahora. Se aclarará a medida que revisemos los ejemplos en el libro.

 # Introducción

_Claridad mental_ es a la vez un libro introductorio y de referencia para el
Lenguaje de contratos inteligentes Clarity. Clarity se desarrolló como un esfuerzo conjunto de
[Hiro PBC](https://hiro.so), [Algorand](http://algorand.com) y varios otros
partes interesadas, que originalmente apunta a la cadena de bloques Stacks. Un significativo
innovación en el campo del desarrollo de contratos inteligentes, Clarity le permite
Redacte contratos inteligentes más seguros y protegidos. El lenguaje está optimizado para
legibilidad y previsibilidad y está diseñado específicamente para desarrolladores que trabajan
aplicaciones con transacciones de alto riesgo.

## Público objetivo

Este libro es accesible tanto para principiantes como para desarrolladores experimentados.
Los conceptos se introducen gradualmente a un ritmo lógico y constante. No obstante,
Los capítulos se prestan bastante bien a ser leídos en un orden diferente. Más
Los desarrolladores experimentados pueden obtener el mayor beneficio saltando directamente a los capítulos
que más les interesen. **Si te gusta aprender con el ejemplo, entonces deberías ir
directamente al [capítulo sobre el uso del clarinet](ch07-00-using-clarinet.md).**

Se supone que tienes conocimientos básicos de programación y
conceptos lógicos subyacentes. El primer capítulo cubre la sintaxis general de
Clarity, pero no profundiza en lo que es la programación en sí. Si
Esto es lo que estás buscando, entonces podrías tener un momento más difícil.
trabajando con este libro a menos que tengas una afinidad natural (no descubierta) por
estos temas. Pero no dejes que eso te disuada, busca una introducción
¡Libro de programación y sigue adelante! El diseño sencillo de Clarity lo convierte en un
Excelente primer idioma para aprender.

## ¿Qué son los contratos inteligentes y las cadenas de bloques?

Clarity es un lenguaje para escribir _contratos inteligentes_ que se ejecutan en una _blockchain_.
Antes de poder analizar qué son los contratos inteligentes, debemos entender qué es un contrato inteligente.
blockchain es. Hay una gran cantidad de información disponible sobre el tema, así como
como los diferentes tipos de cadenas de bloques que existen. Por lo tanto, solo hablaremos brevemente
Tocar el concepto en un sentido más general. Se puede pensar en una cadena de bloques
como un tipo especial de _base de datos distribuida inmutable_:

- Se _distribuye_ en el sentido de que todos los participantes pueden obtener una copia completa
  de la base de datos. Una vez que instale un nodo de Bitcoin, por ejemplo, comenzará
  descargando toda la cadena de bloques de la red. No hay una sola parte
  que aloja o administra toda la cadena de bloques en nombre de los usuarios. Todos
  que juega según las reglas puede participar en la red.

- La _inmutabilidad_ proviene del hecho de que una vez que se agrega información, no puede
  (posiblemente[^1]) se puede cambiar. Las reglas de la red impiden que cualquier actor haga
  cambios en los datos que ya se han insertado. Las cadenas de bloques son únicas en el sentido de que
  Aprovechan la criptografía para garantizar que todos los participantes lleguen a un consenso.
  sobre el estado real de los datos. Permite que la red funcione sin la
  necesidad de confiar en una autoridad central y es por eso que se le conoce como
  "sistema sin confianza". De ahí viene el término "cripto" en "criptomoneda".

La cadena de bloques es, por tanto, una especie de registro público seguro y resistente.
Los cambios se realizan enviando un documento debidamente formateado y firmado digitalmente.
_transacción_ en la red. Los diferentes nodos de la red recopilan estas
transacciones, evaluará su validez y luego las incluirá en la siguiente
bloque basado en algunas condiciones. Para la mayoría de las cadenas de bloques, los nodos que crean
Los bloques son reembolsados ​​por los remitentes de las transacciones. Las transacciones vienen adjuntas
con una pequeña tarifa que el nodo puede reclamar cuando incluye la transacción en un
bloquear.

Los contratos inteligentes son programas que se ejecutan sobre cadenas de bloques y pueden utilizar
sus propiedades únicas. En efecto, significa que puedes crear una aplicación
que no se ejecuta en un solo sistema, sino que se ejecuta y verifica en todos ellos.
una red distribuida. Los mismos nodos que procesan transacciones ejecutarán las
Código de contrato inteligente e incluir el resultado en el siguiente bloque. Los usuarios pueden _implementar_
Contratos inteligentes: el acto de agregar uno nuevo a la cadena y llamar a los existentes.
Unos enviando una transacción. Porque ejecutar algún código consume más recursos.
intensivo, la tarifa de transacción aumentará dependiendo de la complejidad de la
código.

## ¿Qué hace que Clarity sea diferente?

El número de lenguajes de contratos inteligentes crece año tras año. Elegir un primer
El lenguaje puede ser un desafío, especialmente para un principiante. La elección es en gran medida
dictado por el ecosistema que le interesa, aunque algunos idiomas son
Aplicable a más de una plataforma. Cada lenguaje tiene sus ventajas y desventajas.
desventajas y está fuera del alcance de este libro analizarlas todas.
En cambio, nos centraremos en lo que distingue a Clarity y por qué es una excelente opción.
Si requiere la máxima seguridad y transparencia.

Uno de los preceptos fundamentales de Clarity es que es seguro por diseño. El diseño
El proceso se guió mediante el examen de errores, trampas y vulnerabilidades comunes.
en el campo de la ingeniería de contratos inteligentes en su conjunto. Hay innumerables contratos reales
Ejemplos mundiales de casos en los que el fracaso de un desarrollador provocó la pérdida o el robo de grandes cantidades de dinero.
cantidades de tokens. Por nombrar dos grandes: un problema que se ha dado a conocer como el
_Un error de paridad_ provocó la pérdida irreparable de millones de dólares en
Ethereum. En segundo lugar, el hackeo de The DAO (una "criptomoneda autónoma descentralizada")
La "Organización" causó un daño financiero tan grande que la Fundación Ethereum
decidió emitir un hard fork polémico que deshizo el robo. Estos y muchos otros
Otros errores podrían haberse evitado en el diseño del propio lenguaje.

### La Clarity se _interpreta_, no se _compila_

El código de Clarity se interpreta y se envía a la cadena exactamente como está escrito.
Solidity y otros lenguajes se compilan en código de bytes antes de enviarlos a
La cadena. El peligro de los lenguajes de contratos inteligentes compilados es doble: primero, un
El compilador añade una capa de complejidad. Un error en el compilador puede dar lugar a diferentes
código de bytes del que se pretendía y, por lo tanto, conlleva el riesgo de introducir un
vulnerabilidad. En segundo lugar, el código de bytes no es legible para humanos, lo que lo hace muy difícil.
para verificar lo que realmente hace el contrato inteligente. Pregúntese: ¿lo haría?
¿Firmas un contrato que no puedes leer? Si tu respuesta es no, entonces ¿por qué debería serlo?
¿Hay alguna diferencia con los contratos inteligentes?[^2] Con Clarity, lo que ves es lo que quieres.
conseguir.

### La Clarity es _decidible_

Un lenguaje decidible tiene la propiedad de que a partir del código mismo se puede saber
con certeza lo que hará el programa. Esto evita problemas como el
[problema de parada](https://es.wikipedia.org/wiki/Problema_de_la_parada). Con Clarity
sabes con certeza que dada cualquier entrada, el programa se detendrá en un número finito
de pasos. En términos simples: se garantiza que la ejecución del programa finalizará.
La decidibilidad también permite un análisis estático completo del gráfico de llamadas para que
Obtenga una idea precisa del costo exacto antes de la ejecución. No hay forma de que
una llamada de Clarity para "quedarse sin gasolina" en medio de la llamada. Si estás
Si no está seguro de lo que esto significa, no se preocupe por ahora. La gran ventaja de
La decidibilidad se hará más evidente con el tiempo.

### La Clarity no permite la _reentrada_

La reentrada es una situación en la que un contrato inteligente llama a otro, lo que
luego vuelve a llamar al primer contrato—la llamada "vuelve a entrar" en la misma lógica.
Puede permitir que un atacante active múltiples retiros de tokens antes de que
El contrato ha tenido la oportunidad de actualizar su balance interno. El diseño de Clarity
considera la reentrada como una característica anti-funcional y la prohíbe a nivel del lenguaje.

### La Clarity protege contra los _desbordamientos_ y los _desbordamientos insuficientes_

Los desbordamientos y subdesbordamientos ocurren cuando un cálculo da como resultado un número que es
ya sea demasiado grande o demasiado pequeño para ser almacenado, respectivamente. Estos eventos arrojan
Los contratos inteligentes pueden desordenarse y activarse intencionalmente en situaciones de mala calidad.
contratos escritos por los atacantes. Por lo general, esto conduce a una situación en la que el
El contrato está congelado o se le han agotado los tokens. Los desbordamientos y subdesbordamientos de cualquier
El tipo provoca automáticamente que se cancele una transacción en Clarity.

### El soporte para tokens personalizados está integrado

La emisión de tokens fungibles y no fungibles personalizados es un caso de uso popular para
Contratos inteligentes. Las funciones de token personalizadas están integradas en el lenguaje Clarity.
Los desarrolladores no necesitan preocuparse por crear un balance interno,
gestión de suministro y emisión de eventos de tokens. La creación de tokens personalizados se explica en
Profundizaremos en capítulos posteriores.

### En Stacks, las transacciones están protegidas por _condiciones posteriores_

Para proteger aún más los tokens de los usuarios, se pueden adjuntar condiciones posteriores a
transacciones para afirmar que el estado de la cadena ha cambiado de cierta manera una vez que
La transacción se ha completado. Por ejemplo, un usuario que llama a un contrato inteligente puede
Adjunte una condición posterior que indique que después de que se complete la llamada, exactamente 500
El STX debería haberse transferido de una dirección a otra. Si el correo
Si la verificación de condición falla, se revierte toda la transacción. Dado que la costumbre
El soporte de token está integrado en Clarity, las condiciones posteriores también se pueden usar para
proteja cualquier otra ficha de la misma manera.

### Las respuestas devueltas no se pueden dejar sin marcar

Las llamadas a contratos públicos deben devolver una llamada _respuesta_ que indica el éxito
o fracaso. Cualquier contrato que llame a otro contrato debe cumplirse debidamente.
Manejar la respuesta. Los contratos de Clarity que no lo hagan no serán válidos y no podrán
se pueden implementar en la red. Otros lenguajes como Solidity permiten el uso de bajo
llamadas de nivel sin necesidad de comprobar el valor de retorno. Por ejemplo, una
La transferencia de token puede fallar silenciosamente si el desarrollador olvida verificar el resultado.
En Clarity no es posible ignorar los errores, aunque eso obviamente no sucede.
evitar la gestión de errores por parte del desarrollador. Respuestas y errores
El manejo de los mismos se cubre extensamente en los capítulos sobre
[funciones](ch05-00-functions.md) y [flujo de control](ch06-00-control-flow.md).

### Composición sobre herencia

Clarity adopta una composición en lugar de una herencia. Esto significa que Clarity es inteligente.
Los contratos no se heredan unos de otros como se ve en lenguajes como
Solidez. Los desarrolladores, en cambio, definen características que luego son implementadas por
diferentes contratos inteligentes. Permite que los contratos se ajusten a diferentes
interfaces con mayor flexibilidad. No hay necesidad de preocuparse por problemas complejos.
árboles de clases y contratos con comportamiento heredado implícito.

### Acceso a la cadena base: Bitcoin

Los contratos inteligentes de Clarity pueden leer el estado de la cadena base de Bitcoin. Esto significa
¡Puedes usar las transacciones de Bitcoin como un disparador en tus contratos inteligentes! Clarity
También cuenta con una serie de funciones integradas para verificar las firmas secp256k1 y
recuperar claves.

[^1]: Se agregó la nota sobre viabilidad para que sea más correcta. Las cadenas de bloques son
Diseñado para ser altamente resistente al cambio, pero ha habido casos de reescritura.
Las cadenas más débiles pueden ser susceptibles a los llamados "ataques del 51%" que permiten una
poderoso minero para reescribir la historia de esa cadena. Por otro lado, el influyente
Las facciones pueden exigir una actualización de nodo que cambie el historial de la cadena por medio de un
hard fork. Por ejemplo, la Fundación Ethereum "resolvió" el problema de la DAO
hackear con un hard fork.

[^2]: Aunque esta característica hace que sea mucho más fácil de leer Clarity smart
contratos, no necesariamente lo hace fácil. Un buen dominio de Clarity es
Todavía es necesario, pero se puede argumentar que es mucho mejor que un
contrato convencional en papel escrito en lenguaje legal, que honestamente puede ser
considerado un lenguaje por derecho propio. La diferencia real es que Clarity
Sólo permite una interpretación, algo que definitivamente no se puede decir de
jerga legal.

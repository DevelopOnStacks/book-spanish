## Secuencias

Las secuencias contienen una secuencia de datos, como su nombre lo indica. Clarity ofrece tres
tipos diferentes de secuencias: _buffers_, _strings_ y _lists_.

### Buffers

Los buffers son datos no estructurados de una longitud máxima fija. Siempre comienzan con
el prefijo `0x` seguido de una cadena
[hexadecimal](https://es.wikipedia.org/wiki/Sistema_hexadecimal). Cada byte se representa
por dos de los llamados
[hexits](https://en.wiktionary.org/wiki/hexit).

```Clarity
0xa1686f6c6121
```

El buffer anterior deletrea _"¡hola!"_. (Cópielo y péguelo en
[esta página](https://coding.tools/hex-to-ascii) para verificarlo).

### Cadenas

Una cadena es una secuencia de caracteres. Estos pueden definirse como
cadenas [ASCII](https://es.wikipedia.org/wiki/ASCII) o
cadenas [UTF-8](https://es.wikipedia.org/wiki/UTF-8). Las cadenas ASCII solo pueden
contener caracteres latinos básicos, mientras que las cadenas UTF-8 pueden contener elementos divertidos como
emojis. Ambas cadenas están entre comillas dobles (`"`) pero las cadenas UTF-8
también tienen como prefijo una `u`. Al igual que los buffers, las cadenas siempre tienen una longitud máxima fija en Clarity.

ASCII:

```Clarity
"Esta es una cadena ASCII"
```

UTF-8:

```Clarity
u"Y esta es una cadena UTF-8 \u{1f601}"
```

Puede usar cadenas para pasar nombres y mensajes.

### Listas

Las listas son secuencias de longitud fija que contienen otro tipo. Como los tipos
no se pueden mezclar, una lista solo puede contener elementos del mismo tipo. Tome esta lista de
_enteros con signo_ como ejemplo:

```Clarity
(list 4 8 15 16 23 42)
```

Como puede ver, una lista se construye utilizando la función `list`. Aquí hay una lista
de cadenas ASCII:

```Clarity
(list "Hola" "Mundo" "!")
```

Y solo para completar, aquí hay una lista que no es válida debido a tipos mixtos:

```Clarity
(list u5 10 "hola") ;; Esta lista no es válida.
```

Las listas son muy útiles y hacen que sea mucho más fácil realizar acciones en masa. (Por
ejemplo, enviar algunos tokens a una lista de personas). Puede _iterar_ sobre una lista
usando las funciones `map` o `fold`.

`map` aplica una función de entrada a cada elemento y devuelve una nueva lista con los
valores actualizados. La función `not` invierte un booleano (`true` se convierte en `false` y
`false` se convierte en `true`). Podemos invertir una lista de booleanos de esta manera:

```Clarity
(map not (list true true false false))
```

`fold` aplica una función de entrada a cada elemento de la lista _y_ al valor de salida
de la aplicación anterior. También toma un valor inicial para usar como segunda entrada para el primer elemento. El resultado devuelto es el último valor
devuelto por la aplicación final. Esta función también se denomina comúnmente
_reduce_, porque reduce una lista a un solo valor. Podemos usar `fold` para sumar
números en una lista aplicando la función `+` (suma) con un valor inicial
de `u0`:

```Clarity
(fold + (list u1 u2 u3) u0)
```

El fragmento anterior se puede ampliar a lo siguiente:

```Clarity
(+ u3 (+ u2 (+ u1 u0)))
```

### Trabajar con secuencias

#### Longitud

Las secuencias siempre tienen una longitud específica, que podemos recuperar usando la función `len`.

Un buffer (recuerde que cada byte se representa como dos hexágonos):

```Clarity
(len 0xa1686f6c6121)
```

Una cadena:

```Clarity
(len "¿Qué longitud tiene esta cadena?")
```

Y una lista:

```Clarity
(len (list 4 8 15 16 23 42))
```

#### Recuperación de elementos

También le permiten extraer elementos en un índice particular. El siguiente
toma el _cuarto_ elemento de la lista. (El conteo comienza en 0).

```Clarity
(element-at (list 4 8 15 16 23 42) u3)
```

También puede hacer lo inverso y buscar el índice de un elemento particular en una
secuencia. Podemos buscar en la lista para ver si contiene el valor `23`.

```Clarity
(index-of (list 4 8 15 16 23 42) 23)
```

Y obtenemos `(some u4)`, lo que indica que hay un valor de `23` en el índice cuatro. El
atento puede que ahora se pregunte, ¿qué es este _"some"_? Siga leyendo y todo se
revelará en la siguiente sección.

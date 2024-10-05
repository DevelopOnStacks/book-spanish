# Tipos

(Si buscas _signaturas de tipo_, consulta el capítulo posterior
[sobre variables](ch04-02-variables.md#signaturas-de-tipo).)

Un concepto importante para muchos lenguajes de programación son los denominados _tipos_. Un
tipo define qué tipo de información se puede almacenar dentro de una _variable_. Si imaginas que una variable es un contenedor que puede contener algo, el tipo define
qué tipo de cosa contiene.

Los tipos se aplican estrictamente y no se pueden mezclar en Clarity.

La [seguridad de tipos](https://es.wikipedia.org/wiki/Seguridad_de_tipos) es clave porque los
errores de tipo (mezclar dos tipos diferentes) pueden provocar errores inesperados con graves
consecuencias. Por lo tanto, Clarity rechaza cualquier tipo de mezcla de tipos. Aquí hay un
ejemplo:

```Clarity
(+ 2 u3)
```

La expresión anterior genera un error:

> Error de análisis: se esperaba una expresión de tipo **'int'**, se encontró **'uint'** (+ 2
> u3)

Antes de poder responder correctamente a la pregunta de _qué_ tipo se esperaba
_dónde_, echemos un vistazo a los diferentes tipos en Clarity. Los tipos se dividen en
tres categorías: _primitivos_, _secuencias_ y _compuestos_.

- Los [primitivos](ch02-01-primitive-types.md) son los bloques de construcción básicos del
lenguaje. Incluyen números y valores booleanos (verdadero y falso).
- Las [secuencias](ch02-02-sequence-types.md) contienen múltiples valores en orden.
- Los [compuestos](ch02-03-composite-types.md) son tipos complejos que están formados por
otros tipos.

---
layout: article
title: Funciones por partes
---

### Concepto Matemático

Las funciones partidas ó definidas por partes ó por trozos son funciones que para diferentes valores del dominio, tienen una definición diferente. Por ejemplo, si tenemos que definir la función Módulo, para ciertos valores del dominio la función será `f(x)` `=` `x` y para otros valores será `f(x)` `=` `-x`

#### Notación Matemática

{% link_image modulo.gif %}

#### Código en Haskell

En Haskell, las funciones partidas se escriben con Guardas, y se escribe la imagen de cada parte *a la derecha* del igual:

`f x | x >= 0 = x`
`    | x <  0 = -x`

Las funciones por partes no son la única forma de tener diferentes definiciones para una función, existen casos en los cuales alcanza con el uso de [pattern matching](pattern-matching-en-haskell.html).

### Errores Comunes

No debemos confundir el uso de guardas (cuyo concepto se explicó arriba) con las funciones que devuelven booleanos.

#### Ejemplo 1

Tomemos éste ejemplo: "Cierta edad es adulta cuando es 18 ó más"

`esAdulta edad | edad >= 18 = True`
`              | otherwise = False`

¡Pero esto es un uso incorrecto de las guardas! En otras palabras: Si una expresión es verdadera ó falsa *por sí sola*, no se lo debo preguntar, tengo que devolver directamente eso.

Ésta es la **manera correcta** de hacerlo:

`esAdulta edad = edad >= 18`

#### Ejemplo 2

Representemos una función que me diga si alguien siempre dice la verdad, sabiendo que "los niños y los borrachos siempre dicen la verdad" Ésta es una manera de hacerlo, errónea:

`siempreDiceLaVerdad alguien | esNiño alguien = True`
`                            | esBorracho alguien = True`
`                            | otherwise = False`

Si bien ésto funciona, es un mal uso de las funciones por partes, ya que no es cierto que esa función tenga diferentes "partes". La definición es lógica, y es *una sola*. Es un "ó" lógico:

¿Cuando alguien dice la verdad?

-   cuando es niño
-   ó
-   cuando es borracho

*Esa es la definición de la función*. Recordemos que el **ó** logico se escribe así:

`Main> (1>34) || (even 4)`
`True`

y el **y** lógico se escribe así:

`Main> (1>34) && (even 4)`
`False`

Entonces, mi función se define **sin guardas**, de ésta manera:

`siempreDiceLaVerdad alguien = esNiño alguien || esBorracho alguien`

#### Ejemplo 3: No usar pattern matching

A veces, cuando estamos aprendiendo guardas, nos olvidamos que poseemos [Pattern Matching en Haskell](pattern-matching-en-haskell.html).

En otras palabras, podríamos tener una función así:

`ivaPara actividad | actividad == "cultural" = 0.0`
`                  | actividad == "alimentaria" = 10.5`
`                  | otherwise = 21.0`

Cuando en realidad, con Pattern Matching, podría quedar más declarativo:

`ivaPara "cultural" = 0.0`
`ivaPara "alimentaria" = 10.5`
`ivaPara _ = 21.0`

Otro ejemplo de lo mismo:

`longitud lista | null lista = 0`
`              | otherwise = 1 + longitud (tail lista)`

Cuando quedaría más declarativo:

`longitud [] = 0`
`longitud (_:resto) = 1 + longitud resto`

#### Ejemplo 4: Repetición de código

Cuando se repite código a ambos lados de la guarda, ésto es un problema:

`f a | a < 3     = 2 `**`+` `5` `*` `g` `a`**
`    | otherwise = 1 `**`+` `5` `*` `g` `a`**

Que puede arreglarse así:

**`f` `a` `=` `fAux` `a` `+` `5` `*` `g` `a`**
`fAux a | a < 3 = 2`
`       | otherwise = 1`

#### Ejemplo 5: Repetición de código más rebuscada

También puede pasar que se repita código entre las guardas de esta manera:

*Se quiere saber el precio del boleto a partir de la cantidad de kms que voy a recorrer. Se sabe que a partir de 4 km, el cálculo del boleto es el cálculo máximo ($2 + $0.1 x cantidad de kms), mientras que hasta los 4km, el cálculo es el 110% del cálculo máximo*

Se propuso esta solución:

`precioBoleto kms | kms >= 4  = `**`2` `+` `0.1` `*` `kms`**
`                 | otherwise = 110 / 100 * (`**`2` `+` `0.1` `*` `kms`**`)`

Pero arriba y abajo se repite `2` `+` `0.1` `*` `kms`). En este caso, se arregla con la forma más común de corregir la repetición de lógica: **delegando** en ambos casos en la misma función.

En otras palabras, si yo tengo:

`maximo kms = 2 + 0.1 * kms`

Entonces esa lógica está ahora en **un sólo lugar**, que puedo llamar desde donde necesite:

`-- Solución final correcta:`
`precioBoleto kms | kms >= 4  = `**`maximo` `kms`**
`                 | otherwise = 110 / 100 * (`**`maximo` `kms`**`)`

Hay otras formas mejores y peores de evitar éste tipo de repeticiones, pero ésta forma (delegando) es bastante buena y sirve en muchos casos.

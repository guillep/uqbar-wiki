---
layout: article
title: Estrategias de evaluacion
---

Estrategias de evaluación
-------------------------

Introducción
============

La operación que realizamos en funcional es aplicar funciones, la idea del tema que vamos a tratar a continuación es saber qué se tiene que tener en cuenta para determinar el orden en en que aplicarán las funciones de una expresión.

Primer ejemplo
==============

```Haskell
  masUno x = x + 1
```

La expresión masUno (2\*3) puede ser evaluada de la siguiente forma

```Haskell
  masUno (2*3)
```

-   aplicamos \*

```Haskell
  masUno 6
```

-   aplicamos masUno

```Haskell
  6 + 1
```

-   aplicamos +

```Haskell
  7
```

Alternativamente podemos evaluar la misma expresión pero aplicando las funciones en el orden inverso

```Haskell
  masUno (2*3)
```

-   aplicamos masUno

```Haskell
  (2*3) + 1
```

-   aplicamos \*

```Haskell
  6 + 1
```

-   aplicamos +

```Haskell
  7
```

No importa el orden en que apliquemos las funciones vamos a llegar al mismo resultado final. Esto no solo vale para ejemplos sencillos sino que se cumple siempre en Haskell.

Esta propiedad no se cumple en la mayoría de los "lenguajes imperativos" (Pascal, C, Smalltalk, Java, C\#, etc.), veamos un ejemplo en Smalltalk:

Si tenemos la expresión n + (n := 1) y n empieza apuntando a 0.

Si empezamos a evaluar de izquierda a derecha

```Haskell
 n + (n := 1)
```

-   aplicamos n

```Haskell
 0 + (n := 1)
```

-   aplicamos :=

```Haskell
 0 + 1
```

-   aplicamos +

```Haskell
 1
```

Si empezamos a evaluar de derecha a izquierda

```Haskell
 n + (n:= 1)
```

-   aplicamos :=

```Haskell
 n + 1
```

-   aplicamos n

```Haskell
 1 + 1
```

-   aplicamos +

```Haskell
 2
```

Como se puede observar, si evaluamos las expresiones con distintas estrategias obtenemos resultados distintos; esto sucede porque las operaciones involucradas no tienen [ transparencia referencial](transparencia-referencial--efecto-de-lado-y-asignacion-destructiva.html) en este caso particular debido a la introducción de una [ asignación destructiva](transparencia-referencial--efecto-de-lado-y-asignacion-destructiva.html).

Estrategias básicas
===================

A una expresión que consta de una función aplicada a uno o más parámetros y que puede ser "reducida" aplicando dicha función la vamos a llamar Redex (Reducible Expression). Se le dice reducción al hecho de aplicar la función no necesariamente vamos a obtener una expresión "más corta" como veremos más adelante. Consideremos la función mult que tiene como dominio una tupla de 2 números

```Haskell
 mult (x,y) = x * y
```

Si queremos reducir la expresión mult (1+2,2+3) está expresión contiene 3 redexs

-   1. 1+2 (la función + aplicada a 2 parámetros)
-   2. 2+3 (la función + aplicada a 2 parámetros)
-   3. mult (1+2,2+3) (la función mult aplicada a 1 parámetro que es una tupla)

Si queremos evaluar la expresión ¿qué estrategia usamos?

De adentro hacia afuera
-----------------------

También conocida como call-by-value

Una de las estrategias más comunes es comenzar desde adentro hacia afuera (innermost evaluation), esta estrategia elige el redex que está "más adentro" entendiendo por esto al redex que no contiene otro redex. Si existe más de un redex que cumple dicha condición se elige el que está más a la izquierda. Vamos al ejemplo

```Haskell
 mult (1+2,2+3)
```

-   aplicamos el primer +

```Haskell
 mult (3,2+3)
```

-   aplicamos el +

```Haskell
 mult (3,5)
```

-   aplicamos mult

```Haskell
 3 * 5
```

-   aplicamos \*

```Haskell
 15
```

Esta estrategia me asegura que los parámetros de una función están completamente evaluados antes de que la función sea aplicada. Por eso se dice que los parámetros se pasan por valor.

De afuera hacia adentro
-----------------------

También conocida como call-by-name

Otra de las estrategias más comunes es comenzar desde afuera hacia adentro (outtermost evaluation), esta estrategia elige el redex que está "más afuera" entendiendo por esto al redex que no esta contenido en otro redex. Si existe más de un redex que cumple dicha condición se elige el que está más a la izquierda. Vamos al ejemplo

mult (1+2,2+3)

-   aplicamos mult

```Haskell
 (1+2) * (2+3)
```

-   aplicamos el primer + (Si seguimos lo que dijimos arriba deberíamos aplicar primero el \* pero vamos a explicar porque no lo hacemos más abajo)

```Haskell
  3 * (2+3)
```

-   aplicamos +

```Haskell
  3 * 5
```

-   aplicamos \*

```Haskell
  15
```

Usando esta estrategia las funciones se aplican antes que los parámetros sean evaluados. Por esto se dice que los parámetros se pasan por nombre. Nota: Hay que tener en cuenta que muchas funciones que ya vienen con Haskell requieren que sus parámetros estén evaluados antes de que la función sea aplicada, incluso cuando usamos la estrategia "de afuera hacia adentro". Por ejemplo, el operador \* y el + no pueden ser aplicados hasta que sus dos parámetros hayan sido evaluados a números. A las funciones que cumplen con esta propiedad las vamos a llamar funciones estrictas. Funciones estrictas que nos van a interesar a nosotros:

-   Operaciones aritméticas (+,\*,/,etc.)
-   Pattern-Matching (sobre listas, tuplas, etc.)

Evaluaciones que no terminan
============================

Tengan en cuenta la siguiente definición

```Haskell
 inf = 1 + inf
```

Intentar reducir la expresión inf siempre nos va a dar como resultado una expresión más y más grande (independientemente de la estrategia de evaluación que usemos)

```Haskell
 inf
```

-   aplicamos inf

```Haskell
 1 + inf
```

-   aplicamos inf (porque + es estricta)

```Haskell
 1 + (1 + inf)
```

-   aplicamos inf (porque + es estricta)

....

```Haskell
 1 + (1 + (1 + (1 + (1 + (1 + .... + inf )))))
```

Por ende, está evaluación nunca terminaría.

Sabiendo que

```Haskell
 fst (x,_) = x
```

Consideremos la expresión fst (0,inf)

Usando la estrategia call-by-value

```Haskell
 fst (0,inf)
```

-   aplicamos inf

```Haskell
 fst (0, 1 + inf )
```

-   aplicamos inf

```Haskell
 fst (0, 1 + (1 + inf) )
```

-   aplicamos inf

```Haskell
 fst (0, 1 + (1 + (1 + inf) ) )
```

-   aplicamos inf

...

Usando call-by-value la evaluación de la expresión no termina.

Usemos call-by-name:

```Haskell
 fst (0,inf)
```

-   aplicamos fst

```Haskell
 0
```

Usando call-by-name la expresión se evalúa por completo con solo una reducción. En este ejemplo se puede ver que ciertas expresiones que pueden no terminar cuando se evalúan con la estrategia call-by-value pueden terminar cuando se usa la estrategia call-by-name.

De forma más general: Si existe alguna secuencia de evaluación que haga terminar la evaluación de la expresión entonces con la estrategia call-by-name también se termina la evaluación y se produce el mismo resultado final.

Corolario: si te es mucho muy importante que una expresión termine la estrategia que querés usar es call-by-name

Lazy Evaluation
===============

Visión técnica
--------------

Si tenemos la siguiente definición

```Haskell
 alCuadrado x = x * x
```

Vamos a evaluar la expresión alCuadrado (2\*3) usando call-by-value

```Haskell
 alCuadrado (1+2)
```

-   aplicamos +

```Haskell
 alCuadrado 3
```

-   aplicamos alCuadrado

```Haskell
 3 * 3
```

-   aplicamos \*

```Haskell
 9
```

Ahora vamos a evaluar la misma expresión usando call-by-name

```Haskell
 alCuadrado (1+2)
```

-   aplicamos alCuadrado el primer +

```Haskell
 (1+2) * (1+2)
```

-   aplicamos el +

```Haskell
 3 * (1+2)
```

-   aplicamos el \*

```Haskell
 3 * 3
```

-   aplicamos el \*

```Haskell
 9
```

Llegamos la mismo resultado pero en el segundo ejemplo realizamos una reducción más (4 reducciones vs 3 reducciones).

Con call-by-name la expresión (1+2) se evaluó dos veces.

Corolario: cuando usamos call-by-value los parámetros son evaluados una y solo una vez; cuando usamos call-by-name el mismo parámetro puede llegar a ser evaluado más de una vez.

Para evitar este quilombo en vez de tener la expresión (1+2) vamos a tener un "puntero a la expresión" llamémoslo p.

```Haskell
alCuadrado (1+2)
```

-   aplicamos alCuadrado

```Haskell
let p = (1+2) in p * p
```

-   aplicamos +

```Haskell
let p = 3 in p * p
```

-   aplicamos \*

```Haskell
 9
```

Cualquier reducción que se haga en una expresión se va a conocer automáticamente por los punteros a dicha expresión. Al uso de punteros para compartir expresiones que representan la mismo parámetro lo vamos a llamar Sharing. Al uso de la estrategia call-by-name más el Sharing lo vamos a llamar Lazy Evaluation (esta es la estrategia que usa Haskell). El Sharing nos asegura que usar Lazy Evaluation nunca requiera más pasos que la estrategia call-by-value.

Visión operativa
----------------

A efectos de resumir lo que vimos hasta ahora vamos a entender lo siguiente ...

**Lazy Evaluation**: con esta estrategia los parámetros solo se resuelven cuando son necesarios (y son evaluados solo lo necesario). También conocida como **evaluación perezosa** o **diferida**.

A la estrategia call-by-value (y sus variantes) también se las conoce como Eager Evaluation. **Eager Evaluation**: con esta estrategia los parámetros tienen que resolverse antes de aplicar la función. También conocida como **evaluación ansiosa**.

Estructuras infinitas
---------------------

Pensemos en la siguiente definición

```Haskell
  unos = 1 : unos
```

(A partir de ahora vamos a pensar que evaluamos todo en Haskell así que la estrategia que usamos es Lazy Evaluation)

```Haskell
 unos
```

-   aplicamos unos

```Haskell
 1 : unos
```

-   aplicamos unos

```Haskell
 1 : ( 1 : unos )
```

-   aplicamos unos

...

En Haskell

```Haskell
  > unos
   [1,1,1,1,1,1,1........
```

Como se puede ver la evaluación de unos no termina. A pesar de esto podemos usar la expresión unos dentro de nuestro programa y aplicarla a otras funciones. Por ejemplo

Siendo head (x:\_) = x y la expresión head unos

```Haskell
 head unos
```

-   deberíamos aplicar head pero como head me fuerza a tener la lista separada en cabeza:cola tenemos que evaluar unos por el pattern-matching

```Haskell
 head (1:unos)
```

-   aplicamos head

```Haskell
 1
```

Con este ejemplo podemos ver que unos no es una lista infinita sino potencialmente infinita, si aplicamos sobre ella funciones que no la fuerzan a evaluarse por completo la computación termina (eso sonó apocalíptico). La potencia de Lazy Evaluation está en que la expresión unos se evalúa solo lo necesario para que pueda usarla la función que la recibe como parámetro.

Listas infinitas
----------------

Ya vimos la lista de unos que es "infinita", ahora veamos como hacer una lista que tenga todos los números naturales

```Haskell
 naturalesDesde x = x : naturalesDesde (x+1)
```

```Haskell
 > naturalesDesde 1
   [1,2,3,4,5,6,7,8,9,...........
```

Haskell trae un atajo para esto

```Haskell
 naturalesDesde x = [x..]
```

También sirve para hacer listas con alguna condición entre dos de sus elementos consecutivos

```Haskell
 > [1,3..]
   [1,3,5,7,9,11,.........
```

Entonces si queremos obtener los primeros 24 múltiplos de 13 podemos hacerlo de esta forma:

```Haskell
 > [13,26..24*13]
   [13,26,39,52,65,78,91,104,117,130,143,156,169,182,195,208,221,234,247]
```

Pero también podemos resolverlo con una lista infinita usando take como veremos a continuación gracias a la evaluación perezosa.

```Haskell
 > take 24 [13,26..]
   [13,26,39,52,65,78,91,104,117,130,143,156,169,182,195,208,221,234,247]
```

Ejemplos
========

Dada la siguiente definición de take

```Haskell
 take 0 _ = []
 take _ [] = []
 take n (x:xs) = x : (take (n-1) xs)
```

```Haskell
 take 3 [1..]
```

-   aplicamos ..

```Haskell
 take 3 (1:[2..])
```

-   aplicamos take - 3ra línea

```Haskell
 1 : (take 2 [2..])
```

-   aplicamos ..

```Haskell
 1 : (take 2 (2:[3..]))
```

-   aplicamos take - 3ra línea

```Haskell
 1 : ( 2 : (take 1 [3..]))
```

-   aplicamos ..

```Haskell
 1 : ( 2 : (take 1 (3:[4..])))
```

-   aplicamos take - 3ra línea

```Haskell
 1 : ( 2 : ( 3 : (take 0 [ 4.. ]))))
```

-   aplicamos take - 1ra línea

```Haskell
 1 : ( 2 : ( 3 : [] ))) = [1,2,3]
```

Vamos a otro ejemplo

```Haskell
 take 3 [4+5,2/0,3*2]
```

-   aplicamos el + (porque dice x: en take 3ra línea)

```Haskell
 take 3 [9,2/0,3*2]
```

-   aplicamos take - 3ra línea

```Haskell
 9 : take 2 [2/0,3*2]
```

-   aplicamos /

```Haskell
 9 : ERROR DIVISON BY ZERO !!!
```

Dada la definición de (!!)

```Haskell
(!!) 0 (x:_) = x
(!!) n (_:xs) = (!!) (n-1) xs
```

```Haskell
(!!) 2 [4+5,2/0,3*2]
```

-   aplicamos el !! (no es necesario aplicar el + porque en (!!) dice (\_:xs) )

```Haskell
(!!) 1 [2/0,3*2]
```

-   aplicamos el !! (no es necesario aplicar la / por lo anterior)

```Haskell
(!!) 0 [3*2]
```

-   aplicamos \* (porque la primer línea de !! lo pide)

```Haskell
(!!) 0 [6]
```

-   aplicamos !!

```Haskell
6
```

Supongamos que hacemos esta consulta:

```Haskell
 > head (filter (3<) [1..])
 ```

Si bien la expresión `filter (3<) [1..]` no termina (seguiría buscando cuáles son mayores a 3 infinitamente), como lo que primero se evalúa es el head y se difiere la ejecución del filtrado, la ejecución va a terminar en cuanto el filter encuentre su primer elemento que pertenezca a la solución que es el 4.

Es importante notar que en este otro caso:

```Haskell
 > head (filter (<0) [1..])
 ```

La evaluación nunca termina por más que se use head que era lo que antes acotaba la ejecución, ya que nunca se va a encontrar el primer elemento que cumpla la condición a diferencia del caso anterior.

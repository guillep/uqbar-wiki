Motivación
----------

Si nuestra base de conocimiento es

`padre(homero,bart).`
`padre(homero,maggie).`
`padre(homero,lisa).`

Vimos varias consultas posibles

`%Es cierto que homero es padre de bart?`
`?- padre(homero,bart).`
`Yes`
`%Es cierto que homero es papá?`
`?- padre(homero,_).`
`Yes`
`%Quienes son los padres de bart?`
`?- padre(P,bart).`
`P = homero`
`%Quienes son los hijos de homero?`
`?- padre(homero,H).`
`H = bart;`
`...`

`%Quienes son padre e hijo?`
`?- padre(X,Y).`
`X = homero`
`Y = bart;`
`etc.`

Pero a pesar de esta gran gama de consultas hay ciertas preguntas que se vuelven complicadas o imposibles. Vamos a tomar como ejemplo la siguiente pregunta: ¿Cuántos hijos tiene homero?.

En el estado actual de las cosas tenemos que hacer la consulta

`?- padre(homero,H).`
`H = bart;`
`H = lisa;`
`H = maggie;`
`No`

Contamos la cantidad de respuestas, en este caso los posibles valores de H y obtenemos la respuesta ... 3. Ahora bien, esto es impracticable cuando el número de respuestas es alto y además no responde a nuestra pregunta de forma directa.

Pensemos en predicados e individuos y definamos un predicado que relacione lo que nos interesa ... una persona y su cantidad de hijos. Dicho predicado puede llamarse cantidadDeHijos/2.

`?- cantidadDeHijos(homero,C).`
`C = 3`

Perfecto, ya tenemos definido nuestro objetivo ahora definamos el predicado a través de una regla (lo quiero hacer por comprensión porque quiero que me sirva para cualquier persona, no solo homero)

`cantidadDeHijos(P,Cantidad) :- ... .`

Para resolver esto volvamos un poco para atrás. En el mundo del paradigma lógico ¿qué es lo que da 3? La cantidad de respuestas de la consulta .

Entonces lo que nos gustaría hacer es tener en un solo lugar todas las respuestas a la consulta para después contar todos los posibles valores de H pero todavía no sabemos cómo hacer eso así que imaginemos que por el momento es magia.

El conjunto como un individuo
-----------------------------

Sin importar como obtenemos esas respuestas las queremos tener identificadas en un solo lugar, en un solo individuo ... un individuo compuesto que representa un conjunto!

En el caso de homero ese conjunto va a ser .

A este tipo de individuo que representa conjuntos lo vamos a llamar **lista** y se encierra entre corchetes () donde cada elemento del conjunto se separa por coma ().

En nuestra definición del predicado vamos a definir una variable Hijos que va representar a dicho conjunto, pero a nosotros no nos interesa el conjunto de hijos sino la cantidad de elementos que tiene ese conjunto ...

Una vez hecha la magia, lo que necesitamos es un predicado que relacione a un conjunto con su cantidad de elementos y eso ya viene con prolog. Ese predicado se llama .

`cantidadDeHijos(P,Cantidad) :- `
`    acá va la magia que le da valor a la variable Hijos,`
`    length(Hijos,Cantidad).`

Nos falta hacer la magia!

¿Cómo obtener todas las respuestas 'juntas'?
--------------------------------------------

La magia consiste en que Hijos sean todos los H que son respuesta de la consulta padre(P,H) cuando P es por ejemplo homero. Hay un predicado en Prolog que hace exactamente eso:

`findall(Elemento,Consulta,Conjunto)`

Dónde:

-   es una variable de la

-   es una lista con todos los Elementos que hacen verdadera a la Consulta

Entonces:

  
findall es un predicado que relaciona a una consulta con el conjunto (lista) de sus respuestas.

El otro parámetro me permite indicar cuál de las variables que usé en la consulta es la que me interesa para completar la lista. Volviendo a nuestro ejemplo

`cantidadDeHijos(P, Cantidad) :- `
`    findall(H, padre(P,H), Hijos),`
`    length(Hijos, Cantidad).`

Con esta definición nuestro objetivo se cumple

`?- cantidadDeHijos(homero,C).`
`C = 3`

Observamos que la variable de la cláusula que define cantidadDeHijos llega ligada al findall, por lo tanto la consulta que está adentro (el 2do parámetro) queda que efectivamente tiene 3 respuestas.

Inversibilidad del predicado findall
------------------------------------

Ahora bien, agregemos algunos hechos a la base de conocimiento

`padre(homero,bart).`
`padre(homero,maggie).`
`padre(homero,lisa).`
`padre(abraham,homero).`
`padre(abraham,herbert).`

Y pensemos en otra consulta

`?- cantidadDeHijos(P,C).`
`C = 5.`

No funciona como esperábamos, ¿a qué se debe?

Debemos mirar la consulta que se está realizando en el findall (el 2do parámetro): . Si pensamos cuántas respuestas tiene esa pregunta veremos que ¡efectivamente son 5!

`%Quienes son padre e hijo?`
`?- padre(P,H).`
`P = homero,`
`H = bart ;`
`P = homero,`
`H = maggie ;`
`P = homero,`
`H = lisa ;`
`P = abraham,`
`H = homero ;`
`P = abraham,`
`H = herbert.`

En el findall/3 como primer parámetro dijimos que nos interesa solamente la variable H de cada respuesta, e Hijos será un conjunto de esos H. Por lo tanto Hijos es la lista \[bart,maggie,lisa,homero,herbert\] que tiene 5 elementos (la respuesta a nuestra consulta).

### Un findall *totalmente inversible*: Generación

El problema está en la consulta de adentro del findall, no queremos preguntar "¿Quienes son padre e hijo?" sino "¿Quienes son hijos de P?" (risas) y para preguntar eso P debe llegar ligada al findall, para poder preguntar por los hijos de una persona en particular.

Ahora tenemos que averiguar cómo ligar a P, y para eso hay que pensar cuáles serían los posibles P que nos interesan. Una respuesta sencilla es pensar que queremos que P sea una persona, entonces podríamos agregar al antecedente la restricción: .

  
(También se podría usar el predicado padre/2 en lugar de persona/1, analizamos la diferencia entre ambos en el próximo apartado.)

cantidadDeHijos(P,Cantidad) :-

`    persona(P), % Generacion, asi la variable P llega ligada al findall`
`    findall(H,padre(P,H),Hijos),`
`    length(Hijos,Cantidad).`

Lo que logramos al hacer que P llegue ligada al findall es que el predicado cantidadDeHijos/2 sea totalmente inversible. A esta técnica la denominamos [generación](generacion.html).

### Dos formas distintas de generación

Dijimos que en realidad hay dos formas de determinar cuáles son todos los P que me interesan:

1.  Una forma es decir que P es una persona, entonces podríamos poner:
2.  Por el otro podemos pensar que P tiene que ser padre, entonces surge la opción: . (Me interesa que sea padre y no me importa en principio cuáles son sus hijos.

¿Qué pasaría si usamos en lugar de como generador?

`cantidadDeHijos(P,Cantidad) :-`
`    padre(P,_), %Generacion, asi la variable P llega ligada al findall`
`    findall(H,padre(P,H),Hijos),`
`    length(Hijos,Cantidad).`

La diferencia la vamos a encontrar si hacemos la consulta:

`?- cantidadDeHijos(bart,C).`

Con la solución propuesta en el apartado anterior nos dice que bart tiene cero hijos:

`?- cantidadDeHijos(bart,C).`
`C = 0`

Con la segunda posibilidad, bart no es una posible respuesta para (porque no es padre de nadie). Entonces lo que voy a obtener es:

`?- cantidadDeHijos(bart,C).`
`No`

En este caso consideramos que es más saludable un 0 que un No. Independientemente de eso, lo que debe quedar de todo esto es que las distintas formas de generar nos pueden dar diferentes resultados como respuesta y hay que elegir qué queremos.

Una pregunta adicional que podría surgir es: ¿qué pasa si no tenemos el predicado persona? Bueno, habrá que agregarlo a la base de conocimientos, y para ello tenemos dos posibilidades:

Por extensión:uno por uno enumerando cada persona (un hecho para cada persona:  

`persona(bart).`
`persona(lisa).`
`... etc.`

Por comprensión:con una regla descubrir quiénes podemos considerar persona a partir de la información que ya tenemos. Una forma de hacer eso sería:  

`persona(Papa) :- padre(Papa,_).`
`persona(Hijo) :- padre(_,Hijo).`

Es decir, el que es padre de alguien es una persona, y el que es hijo también.
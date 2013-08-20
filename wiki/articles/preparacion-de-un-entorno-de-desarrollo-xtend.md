### Download e instalación base

Descargá en [este link](http://www.eclipse.org/xtend/download.html) el plugin para el Eclipse.

Tenés dos opciones: **Full Eclipse** (bajarte el Eclipse + el plugin directamente) o [configurar tu entorno base](http://uqbar-wiki.org/index.php?title=Preparacion_de_un_entorno_de_desarrollo_Java) para luego descargar el plugin.

El plugin de xtend es un plugin relativamente grande. Si tu instalación de eclipse tiene además otros plugins para programar en otros lenguajes, puede que sea recomendable armar una instalación aparte. Si uno usa muchos lenguajes con eclipse como IDE, puede ser recomendable tener un eclipse para cada uno (salvo obviamente en el caso en que varios lenguajes se utilicen en el mismo proyecto). Eso hace que el IDE sea más liviano y minimiza posibles problemas de incompatibilidad entre los diferentes features.

De todos los items que aparecerán en el update site debemos elegir "xtend". Es importante también instalar la integración con m2eclipse. Acá un screenshot a modo de ejemplo (ojo que seguramente cambien los nombres de las cosas en el futuro).

![](xtend-plugin.png "xtend-plugin.png")

### ¿Cómo empezar?

-   Crear un proyecto Java.
-   Luego crear una clase XTend.
-   El eclipse mostrará un error similar a

Para más detalles pueden mirar <http://www.eclipse.org/xtend/download.html>

### Tips

-   Instalate la versión 2.4 ó superior, ya que versiones anteriores tienen problemas entre clases xtend e interfaces java cuando hay referencias circulares entre sí.

### Documentación

-   [Documentación oficial](http://www.eclipse.org/xtend/documentation.html)

### Links útiles

-   Si venís del mundo Java chequeá [este link](http://jnario.org/org/jnario/jnario/documentation/20FactsAboutXtendSpec.html)

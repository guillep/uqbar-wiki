Download e instalación base
---------------------------

Descargá en [este link](http://www.eclipse.org/downloads/) el Eclipse IDE for Java EE Developers. Luego de [configurar tu entorno base](http://uqbar-wiki.org/index.php?title=Preparacion_de_un_entorno_de_desarrollo_Java), instalás el plugin para desarrollo de Android desde el Eclipse Marketplace.

1.  Help &gt; Eclipse Marketplace...
2.  buscar "Android"
3.  Seleccionar Android Development Tool for Eclipse.
4.  Confirm
5.  Finish

Reiniciar el IDE.

### Cómo empezar

-   <http://developer.android.com/index.html>
-   <http://www.vogella.com/android.html>
-   Un [tutorial](http://www.youtube.com/watch?v=zS1frzHbKWY) de youtube que va bien tranquilo

Configuración para lenguaje Xtend
---------------------------------

Una vez instalado [el entorno base](preparacion-de-un-entorno-de-desarrollo-xtend.html)

1.  Window &gt; Preferences, Xtend compiler &gt; Output folder for generated Java files, cambiar el directorio a "gen"
2.  En cada proyecto: agregar la librería Xtend en el proyecto: botón derecho sobre el proyecto &gt; Configure build path &gt; Libraries: Add Xtend Library, y luego chequear Xtend Library en la solapa "Order and Export" para que se incluya en el ejecutable de Android.

### Archetype de Maven para Android + Xtend

Si te interesa tenés un archetype propio de Xtend para Android:

`mvn archetype:generate -DarchetypeGroupId=org.eclipse.xtend \`
`  -DarchetypeArtifactId=xtend-android-archetype \`
`  -DarchetypeCatalog=`[`http://repo.maven.apache.org/maven2`](http://repo.maven.apache.org/maven2)

Material de Android en Xtend
----------------------------

=

-   <http://blog.efftinge.de/2011/12/writing-android-uis-with-xtend.html>
-   <http://nataliaossipova.wordpress.com/2013/03/10/xtending-android-development/>

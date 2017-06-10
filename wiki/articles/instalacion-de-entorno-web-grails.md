---
layout: article
title: Instalacion de entorno web grails
featured: true
---

<img src="/img/languages/grails-logo.jpg" width="30%" height="30%"/>

# Pre-requisitos

Asumimos que tenés instaladas las herramientas básicas, en particular necesitaremos que tengas una JDK, no puede ser una JRE. Para más ayuda, seguí este [link](preparacion-de-un-entorno-de-desarrollo-java.html).

# GGTS: Entorno integrado de Desarrollo y Framework Grails

Te recomendamos utilizar la versión 3.6.0 del GGTS, que es más liviana que las siguientes. Los links para descargar son:

-   linux 64 bits: <http://download.springsource.com/release/STS/3.6.0/dist/e4.4/groovy-grails-tool-suite-3.6.0.RELEASE-e4.4-linux-gtk-x86_64.tar.gz>
-   linux 32 bits: <http://download.springsource.com/release/STS/3.6.0/dist/e4.4/groovy-grails-tool-suite-3.6.0.RELEASE-e4.4-linux-gtk.tar.gz>
-   win64: <http://download.springsource.com/release/STS/3.6.0/dist/e4.4/groovy-grails-tool-suite-3.6.0.RELEASE-e4.4-win32-x86_64.zip>
-   win32: <http://download.springsource.com/release/STS/3.6.0/dist/e4.4/groovy-grails-tool-suite-3.6.0.RELEASE-e4.4-win32.zip>
-   mac cocoa 64: <http://download.springsource.com/release/STS/3.6.0/dist/e4.4/groovy-grails-tool-suite-3.6.0.RELEASE-e4.4-macosx-cocoa-x86_64.tar.gz>

Si querés descargar la última versión estable del GGTS tenés el siguiente link

-   <https://spring.io/tools/ggts/all>

Allí seleccionás el entorno adecuado para tu sistema operativo.

## Configuraciones

### ¿Dónde lo instalo?

-   Es preferible que el directorio de instalación no tenga espacios (C:\\GGTS, C:\\IDEGrails, /home/fer/IDEGrails, etc.)

### ¿Qué JDK uso?

-   Si bien en teoría la JDK 1.8 es compatible con Grails 2.4, nuestra experiencia nefasta nos dice que **no uses una JDK 1.8, tiene que ser 1.6 ó 1.7** (cualquiera, la instalás en otra carpeta)
-   Recordamos que no es válido asociarle una JRE sino que tiene que incluir el compilador y demás herramientas de desarrollo que vienen con la JDK.
-   La JDK y el GGTS deben coincidir en la versión de 32 ó 64 bits (si el GGTS es 32 bits tenés que apuntar a una JDK de 32 bits, o no va a funcionar)

### ¿Cómo asocio la JDK al GGTS?

Para asociar la JDK al GGTS: si descargás y ejecutás el instalador lo definís ahí mismo, si descomprimís el archivo o se lo querés cambiar creás un acceso directo al GGTS:

```bash
GGTS -vm "/path donde está la JDK"
```

-   las comillas son importantes si en el path hay espacios en blanco, y
-   tené en cuenta incluir el directorio bin del JDK en el path.

Más información en <https://spring.io/tools/ggts>

<!-- -->

## Web Server

El IDE trae consigo un Web Server Tomcat (llamado *vFabric tcServer*) integrado con el entorno.

# Configuración del Workspace

Si estás en Windows te recomendamos que uses un directorio raíz: C:\\WorkspaceGrails o similar (no C:\\Documents and Settings\\User\\etc.)

# Configuración del entorno Grails

-   Primero, seguir las configuraciones estándares para cualquier eclipse como se muestra en [este link](preparacion-de-un-entorno-de-desarrollo-java.html#tocAnchor-1-7-1)

<!-- -->

-   Además, les recomendamos modificar Window &gt; Web Browser &gt; Default web browser para ejecutar las aplicaciones en un browser externo (si no el IDE se pone todavía más pesado)

{% wiki_note_warning %}
  IMPORTANTE: Cómo configurar la versión default de Grails
{% endwiki_note_warning %}
 
Ir a Window &gt; Preferences, y filtrar por la palabra "Grails". Seleccionar al nodo "Grails" (esto es: Groovy &gt; Grails), y en la grilla "Grails Installations", pueden pasar dos cosas:

-   el GGTS que instalaste ya tiene una configuración de Grails default. En ese caso vas a ver chequeada una opción de Grails, y no deberías hacer más nada
-   el GGTS lo descomprimiste en una carpeta, entonces tenés que configurar manualmente cuál es la versión de Grails con la que vas a trabajar.

Para hacer eso (o para incorporar otra versión de Grails a tu entorno de desarrollo), hagan click en Add..., luego Browse y buscar la carpeta donde saben que está el framework Grails. Si no estás seguro de dónde queda eso, es en el mismo directorio donde descomprimiste el GGTS. *Ej:* si descomprimiste el GGTS en la carpeta "C:\\GGTS\\GGTS360", la versión de Grails con la que viene es 2.4.2, y se ubicará en el directorio "C:\\GGTS\\GGTS360\\grails-2.4.2". Eligen ese directorio y presionan ok dos veces.

## Configurar el proxy para poder bajar dependencias

Si estás trabajando con un proxy tenés que ir a Window &gt; Preferences, Groovy &gt; Grails &gt; Grails Launch y configurar estas variables

-   **http.nonProxy.hosts**: direcciones locales como 10.\*
-   **http.proxyHost**: tu servidor proxy
-   **http.proxyPort**: el puerto que escucha el proxy

# ¿Y si no quiero usar Eclipse?

Después buscar un IDE de su agrado e incorporarle el plugin para Grails, acá tienen un link para integrarlo con IDEA: <http://grails.org/IDEA+Integration>

# Cómo crear un proyecto desde cero

Muy simple, New &gt; Grails project.

# Cómo empezar

-   Tutoriales para conocer la tecnología: <http://grails.org/start>
-   Manual de referencia: <http://grails.org/doc/latest/guide/>
-   Plugins que se pueden instalar: <http://grails.org/plugins/>

# Cómo levanto las aplicaciones

-   Ctrl + Shift + Alt + G &gt; run-app va a levantar el servidor en el proyecto sobre el cual están parados (eso se puede cambiar en la consola)
-   Una vez que aparezca un mensaje con el link a la aplicación

pueden copiar la URL e ir a un Browser y pegar esa dirección para probar la aplicación.

También pueden dar un click sobre la URL y eso los llevará por defecto a un browser interno, que consume bastantes recursos. Por eso recomendamos modificar la configuración default para que les abra un browser por afuera del entorno: Window &gt; Web Browser &gt; Default system web browser.

Hay una solapa Server en el cual pueden agregar o eliminar las aplicaciones web. No obstante este server requiere una configuración adicional para asignarle un Tomcat en forma manual (no es el Tomcat interno que trae el entorno GGTS de Grails), por lo que por el momento recomendamos no utilizarlo.

<!-- -->

# Migrar a una version diferente de Grails

En el caso en que quieras migrar de versión un proyecto,

-   tenés que buscar el archivo application.properties, modificar la línea donde está la versión de Grails a la que corresponda

```bash
app.grails.version=2.3.8
```

-   modificar si es necesario en el BuildConfig.groovy las versiones de las dependencias (las versiones para un proyecto default las podés conocer creando un proyecto Grails vacío y revisando cómo se genera el BuildConfig)
-   luego desde el menú Grails Tools &gt; Refresh Dependencies

Con eso debería ser suficiente. De haber algún problema: hacer un grails clean (Ctrl + Alt + Shift + G &gt; clean) y un clean + refresh desde el GGTS

<!-- -->

# Troubleshooting

## Error al querer refrescar dependencias o compilar

Si te aparece un error en la consola como éste

```bash
java.lang.ClassNotFoundException: unable to locate the java compiler com.sun.tools.javac.Main, please change your classloader settings
```

no estás apuntando en tu variable de entorno JAVA\_HOME a un JDK configurado en tu GGTS. Reapuntá alguno de los dos.

## Compatibilidad de las versiones de Grails y JDK

Asegurate de respetar la misma versión de 32/64 bits de tu JDK/GGTS, de lo contrario cuando crees un nuevo proyecto Grails te puede aparecer un mensaje indicando que la JDK asignada corresponde a una JRE (no encontrará 'tools.jar')

## Problemas con Groovy Object al compilar

Al bajar ejemplos puede haber un problema con la versión del compilador ("Groovy Object not found" o si avisa que hay errores con el Build path), en ese caso hay que hacer botón derecho sobre el proyecto &gt; Properties &gt; Groovy compiler y seleccionar "I don't care" en la opción "Groovy compiler level".

## Integración con JDK

Grails 2.2.2/2.2.3 y la versión 1.7.0_25 tienen problemas temporales de integración, si te aparece este mensaje al compilar/correr la app

```bash
Could not determine Hibernate dialect for database name [H2]!
```

reemplazá la versión de tu JDK por una distinta.

## Problemas al querer abrir la ventana de comandos

Si al querer abrir la ventana de comandos (Ctrl + Shift + Alt + G) te aparece una ventana de error con el siguiente mensaje:

```bash
An internal error occurred during: "Retrieving available scripts".
java.lang.NullPointerException
```

es que estás trabajando en un proyecto que tiene una versión de Grails que no está instalada en tu máquina, las opciones son

1.  Instalar esa versión de Grails en un directorio diferente (en <http://grails.org/download>)
2.  O bien migrar el proyecto a la versión de Grails que tenés instalada en tu máquina como se explicó [anteriormente](instalacion-de-entorno-web-grails-migrar-a-una-version-diferente-de-grails.html)

<!-- -->

## Problemas para levantar el IDE con proyectos pesados

No debería ocurrir, ya que los ejemplos son didácticos y están pensados para levantar en entornos sin mayores problemas, pero si el IDE está ocupando mucha memoria al iniciar conviene chequear [este link]( http://stackoverflow.com/questions/13515704/how-to-fix-groovy-grails-tool-suite-3-extreme-memory-usage)

## Errores extraños en las annotations

Revisá que el compilador tenga seteada una compatibilidad a un JDK igual para todos el grupo de trabajo. Esto se ve en Window &gt; Preferences, Java &gt; Compiler, JDK Compliance level : Compiler Compliance Level (deberían utilizar el mismo todos los integrantes del grupo).

## Errores al trabajar con librerías externas

Esto puede ocurrir si estás trabajando con xtend, que utiliza guava como dependencia. Si al hacer run-app ves un mensaje de error similar al siguiente

```bash
ClassNotFoundException: com.google.common.base.Objects
```

buscá el archivo BuildConfig.groovy de tu proyecto y comentá estas dos líneas de configuración:

```
grails.project.war.file
grails.project.fork
```

# Integración con otros lenguajes de programación

-   [Integración Grails con Xtend](integracion-grails-con-xtend.html)
-   [Integración Grails con Scala](integracion-grails-con-scala.html)
-   Grails se integra naturalmente con Java, sólo hay que definir las clases Java en el source folder src/java y se incluyen con la aplicación web

# Links relacionados

-  [Temario Algoritmos III](algo3-temario.html)

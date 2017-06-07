---
layout: article
title: Configuraciones generales para cualquier eclipse
---

Hay varias cosas que es útil configurar en Eclipse, independientemente del lenguaje que elijan.

Compiler
--------

Algunas versiones de Eclipse utilizan por defecto compatibilidad con el compilador Java 1.4, algo que no es conveniente si vamos a trabajar con herramientas como Generics o Annotations que vienen a partir del JDK 1.5.

Para esto deben ir a Window Preferences &gt; Java &gt; Compiler &gt; y donde dice JDK Compliance subir la propiedad "Compiler compliance level" de 1.4 a una superior (1.8 ó superior)

En caso contrario al bajar proyectos compilados en JDKs superiores aparecerán mensajes de error como estos:

```bash

Syntax error, annotations are only available if source level is 1.5 or greater
Syntax error, parameterized types are only available if source level is 1.5 or greater

```

Encoding
--------

Para no tener problemas con los tildes y demás caracteres especiales al bajarse los ejemplos conviene tener sincronizado el mismo encoding. Para eso, desde la barra de menú: Window &gt; Preferences, filtrar por "encoding" y cambiar todos a "UTF-8" o "ISO 10646/Unicode(UTF-8)". Por ejemplo: En General &gt; Workspace &gt; Text file encoding, seleccionar Other &gt; UTF-8. Aplicar cambios.

Spell
-----

Si van a programar en español, es recomendable desactivar el diccionario (viene por defecto en inglés). Para ello filtrar en el menú por la palabra "spell" y desactivar la corrección ortográfica (Spelling &gt; desactivar el check Enable spell checking). Aplicar cambios.

Otra opción es que se bajen un diccionario español de internet y lo configuren.

Warnings
--------

En varios lenguajes de la JVM, tendremos warning sobre serialización, una tecnología que no utilizaremos en la materia. Conviene desactivar el warning default de clases serializables que no definan un identificador de versión: Window &gt; Preferences, filtrar por "Serializable", solapa Java / Compiler / "Errors/Warnings", "Potential programming problems", y se setea el valor de "Serializable class without serialVersionUID" a Ignore. Aplicar cambios.

Shortcuts
---------

En algunas distribuciones de Linux existe un shortcut por defecto que es Ctrl + Space, que colisiona con el shortcut del content assist de Eclipse . Para solucionar el problema, hay que deshabilitar el binding: en Ubuntu: System Settings -&gt; Keyboard-&gt; Shortcuts en Lubuntu: click en el logo que esta abajo a la izquierda -&gt; Preferencias -&gt; Metodos de entrada por Teclado y se cambia a "Disabled"


Indices de Maven
----------------

Si instalaron el plugin de Maven para Eclipse, o si ya viene instalado con el que hayan descargado, y no piensan utilizarlo, para ahorrar tiempos de carga es recomendable desactivar la indización de artefactos. Esto es particularmente útil cuando tenemos conexión de red limitada o máquinas con pocos recursos. Para eso, ir a Window &gt; Preferences &gt; Maven. Marcar Offline y desmarcar Download repository indexes

Si vas a trabajar con Maven, te recomendamos que tildes estas dos opciones:

-   Download Artifact Sources
-   Download Artifact JavaDoc

para poder tener acceso al fuente y a los javadocs de todas las clases hechas por terceros (cuando utilices frameworks, por ejemplo). También recordá destildar la marca Offline.

Filtros de paquetes
-------------------

Groovy, Java y Xtend comparten la forma de importar paquetes. En un JDK estándar hay muchos paquetes, y sólo usaremos unos pocos. Es recomendable indicarle a Eclipse que no nos sugiera paquetes que casi con seguridad no usaremos.

Para eso, en Java &gt; Appearance &gt; Type Filters, agregar las siguientes expresiones:

```bash

bash
sun.*
*.internal.*
edu.emory.mathcs.backport.*
java.awt.*
java.swing.*
org.omg.*

```

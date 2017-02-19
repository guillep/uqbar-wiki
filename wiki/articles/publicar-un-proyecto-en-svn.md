---
layout: article
title: Publicar un proyecto en svn
---

Antes de publicar un proyecto es (obviamente) necesario tener un usuario y un repositorio svn, una forma de hacer eso es [Crear un proyecto en xp-dev](crear-un-proyecto-en-xp-dev.html)

Desde el Eclipse:
-----------------

Botón derecho sobre el proyecto -&gt; Team -&gt; Share Project y ahí poner la url del repositorio donde esta el proyeto seguido de . Por ejemplo:

```
http://svn2.xp-dev.com/svn/nombre_del_repositorio/nombre_del_proyecto/trunk
```
Esto va a crear la carpeta del proyecto y/o trunk si no existen.

Este paso nos lleva a la perspectiva Synchronize del eclipse donde tenemos que comitear para guardar en el trunk nuestros sources del proyecto. En este momento, antes de comitear tenemos que indicarle al svn los archivos y carpetas que no queremos versionar, para esto, le damos botón derecho -&gt; Add to <svn:ignore>.

Lo que tenemos que agregar al <svn:ignore> en un proyecto simple es:

-   la carpeta que guarda información del eclipse
-   la carpeta que es donde estan los compilados
-   el y el

**Recordemos que siempre ponemos en el <svn:ignore> todo aquello que se genera automáticamente, en este caso, todos estos archivos los genera el maven.**

Entonces ahora que ya le dijimos al svn que es lo que no queremos versionar, estamos en condiciones de comitear. Botón derecho sobre el proyecto -&gt; Commit.

Después hay que crear las carpetas branches y tags en el repositorio.

Desde la consola:
-----------------

---
layout: article
title: JFace controles y binding
---

**Nota:** el objetivo de esta página es comentar cómo se produce el binding entre controles de SWT/JFace y atributos de dominio. Si desea documentación para programar interfaces de usuario puede consultar [la página oficial de SWT](http://www.eclipse.org/swt/) y [el wiki de JFace](http://wiki.eclipse.org/index.php/JFace)


# Cómo se implementa el binding en JFace

A través de la clase DataBindingContext que conoce a ambos observers: los interesados en los cambios del modelo (BeansObservables) pero también al modelo le interesa los cambios que hace el usuario a través de la vista (SWTObservables).

*Ejemplo:* si queremos tener binding bidireccional entre el atributo nombre de un Socio y el control textbox de la pantalla de actualización de un socio

-   Al generarse la pantalla debemos bindear la propiedad "nombre" del objeto socio con el control textbox de la pantalla de actualización
-   Si se modifica el valor del atributo nombre de un socio (por afuera de la UI), debemos disparar la notificación al textbox. Esto se hace en el setter de nombre, enviando el mensaje firePropertyChange.
-   Si el usuario escribe algo en el textbox, eso dispara automáticamente la actualización del modelo. Ejemplo: en el textbox del nombre escribimos "TARCISO", entonces se disparará el mensaje setNombre("TARCISO") al objeto Socio. El setNombre a su vez disparará un firePropertyChange, pero la lógica de notificación es inteligente y sabe cortar aquí el flujo de avisos para no entrar en loop.

# Control Textbox

En SWT el textbox está representado por la clase `org.eclipse.swt.widgets.Text`

Podemos bindear la propiedad text de un control textbox contra un atributo String de un modelo de la siguiente manera

<!-- -->

```java
new DataBindingContext().bindValue(
    SWTObservables.observeText(controlTextBoxEnCuestion, SWT.FocusOut), 
    BeansObservables.observeValue(objetoModelo, propiedadDelModeloContraElQueSeBindea), 
    null, 
    null);
```

donde controlTextBoxEnCuestion es de tipo Text, objetoModelo es cualquier Object y propiedadDelModeloContraElQueSeBindea es un String.

Otras propiedades que admiten binding bidireccional:

-   **editable**: un valor booleano que permite habilitar/deshabilitar el input desde el teclado
-   **visible**: el control puede hacerse visible o invisible en base al binding con un atributo de tipo boolean del modelo
-   **font/foreground/background**: se puede modificar la letra con la que se visualizan los datos cargados en el control, el color de la letra o bien el de fondo, en base a un atributo de un modelo.

*Ejemplo:* queremos permitir cambiar el nombre a un socio solamente si está activo. El socio define como interfaz setNombre/getNombre y un isActivo(), no importa cómo se implementa. Entonces para bindear la propiedad Editable del textbox que ingresa el nombre (llamado textNombre) debemos escribir lo siguiente:

<!-- -->

```java
new DataBindingContext().bindValue(
    SWTObservables.observeEditable(textNombre), 
    BeansObservables.observeValue(socio, "activo"), 
    null, 
    null);
```

Nótese que no necesariamente la "propiedad" del modelo tiene que ser un atributo (variable de instancia) del objeto modelo. En nuestro caso el estar activo podría no ser un getter booleano sino un atributo calculado dinámicamente. Lo que hace JFace es recuperar el valor del método getActivo() o isActivo() para el objeto socio (a través de Reflection) y asociarlo a la propiedad editable del control (lo que devuelva el método getActivo() debe ser un booleano).

Mirando la clase SWTObservables podemos ver todas las propiedades que el framework en cuestión nos permite vincular.

Las siguientes propiedades no tienen binding bidireccional con un atributo del modelo. No obstante, podemos modificar sus valores al cargar el formulario o ante cualquier otro evento que el usuario o la pantalla dispare:

-   **width:** el tamaño del control
-   **size:** la cantidad de caracteres que permite cargar

# Control Checkbox

En SWT el checkbox está representado por la clase `org.eclipse.swt.widgets.Button` pasándole el parámetro SWT.CHECK al constructor.

Se puede bindear la propiedad SELECTION contra un atributo boolean de un modelo:

<!-- -->

```java
new DataBindingContext().bindValue(
    SWTObservables.observeSelection(controlCheckBoxEnCuestion), 
    BeansObservables.observeValue(objetoModelo, atributoBooleanDelModeloContraElQueSeBindea), 
    null, 
    null);
```

También se puede bindear las propiedades visible, font, foreground y background, entre otras. No tiene sentido bindear la propiedad text, hacerlo resulta en error.

# Control Combo

En SWT el combo está representado por la clase `org.eclipse.swt.widgets.Combo`

Se agregan elementos al combo:

-   enviando el mensaje `add(String string)` o `add(String string, int index)` o `setItem(int index, String string)`
-   enviando el mensaje `setItems(String[] items)`

Como se desprende de la interfaz de cada uno de los métodos

-   no podemos agregar cualquier objeto, tienen que ser strings, esto nos fuerza a 1) tener en paralelo una lista de elementos "posta" relacionados o 2) recuperar con ese string el elemento (el string debe identificar de manera unívoca a ese elemento). *Ejemplo:* si el combo muestra socios de un videoclub, debemos mostrar por un lado el nombre del socio pero por otra parte tenemos que encontrar el objeto socio "posta" en base al nombre. De la misma manera ocurriría con los géneros de una película, o con los libros de una biblioteca, etc.
-   los elementos se guardan en un orden, por eso existen métodos que agregan a partir de una posición

Para conocer cuál es el elemento seleccionado del combo enviamos el mensaje getSelectionIndex(), que devuelve un int. Vemos el javadoc:

<!-- -->

```java
/**
 * 
 * Returns the zero-relative index of the item which is currently selected in the
 * receiver's list, or -1 if no item is selected.
 *
 */
public int getSelectionIndex() 
```

Tener el selectionIndex como un entero refuerza la idea de que el orden en el combo es importante, si tengo los elementos en un Set no me serviría el selectionIndex.

El binding relaciona un String con la propiedad selection del combo cuando el usuario modifica el elemento elegido.

*Ejemplo:* queremos definir un combo cuyos elementos son la lista de socios de un videoclub. El código es el siguiente

<!-- -->

```java
Combo comboSocios = new Combo(formulario, SWT.LEFT | SWT.BORDER | SWT.SINGLE);
comboSocios.setLayoutData(new GridData(150, SWT.DEFAULT));
for (Socio socio : this.getSocios()) {
    comboSocios.add(socio.getNombreCompleto());
}
new DataBindingContext()
    .bindValue(SWTObservables.observeSelection(comboSocios), 
               BeansObservables.observeValueEditarSocioModel, socioSeleccionado), 
               null,
               null);
```

El binding no lo podemos hacer contra un socio en particular, necesitamos trabajar con un model que tenga comportamiento adicional. El EditarSocioModel define el método setSocioSeleccionado de la siguiente manera:

<!-- -->

```java
public void setSocioSeleccionado(String nombreCompletoSocio) {
`    ...
}
```

Aquí entonces hay que convertir el nombre del socio (un String) al objeto Socio en cuestión si queremos enviarle algún mensaje.

# Control Grilla

En SWT la grilla se representa con la clase `org.eclipse.swt.widgets.Table`


# Control Botón

En SWT la grilla se representa con la clase `org.eclipse.swt.widgets.Button` pasándole el parámetro SWT.PUSH al constructor.

El botón tiene listeners, cuando el usuario presiona el botón se dispara la notificación del botón apretado a cada listener. Nosotros escribimos el código no en el botón, sino en el listener.

*Ejemplo:*

<!-- -->

```java
Button botonAceptar = new Button(formulario, SWT.PUSH);
botonAceptar.setText("Aceptar");
botonAceptar.addSelectionListener(
    new SelectionListener() {
        @Override
        public void widgetSelected(SelectionEvent e) {
            ... <-- ACA ESCRIBIMOS EL CODIGO DEL BOTON -->
        }
        @Override
        public void widgetDefaultSelected(SelectionEvent e) {
            this.widgetSelected(e);
        }
         
    );
``` 

Las propiedades más interesantes de bindear son enabled (si el botón está habilitado) y visible (si está visible).


# Campos calculados

Podemos subclasificar la clase **ComputedValue** para vincular cualquier expresión que deseemos: sólo tenemos que redefinir el método 

```java
protected Object calculate()
```

Pueden verse los siguientes ejemplos [&gt; ver ComputedValue](http://wiki.eclipse.org/JFace_Data_Binding/Snippets)


# Links relacionados

-   [Formas de vincular una vista con el modelo de dominio](formas-de-vincular-una-vista-con-el-modelo-de-dominio.html)
-   [Ejemplos de Binding entre vista y modelo](ejemplos-de-binding-entre-vista-y-modelo.html)
-   [Temario Algoritmos III](algo3-temario.html)


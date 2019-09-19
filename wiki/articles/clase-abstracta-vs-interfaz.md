---
layout: article
title: Clase abstracta vs interfaz
---

# Definiciones


## Clase abstracta

Una clase abstracta es una clase que no tiene instancias. Su utilidad consiste en proveer estructura y comportamiento común a todas las subclases que heredan de ella.

```java
public abstract class Figura {
   final public boolean menorQue(Figura otraFigura) {
      return this.getArea() < otraFigura.getArea();
   }
   public abstract double getArea();
   public abstract double getPerimetro();
}
```

Si tenemos la clase Rectángulo, Círculo, etc. que heredan de Figura todas se benefician del comportamiento default para el método menorQue que permite comparar una figura con otra. Por otra parte los métodos que resuelven el área y el perímetro son **abstractos**, no se provee una implementación sino que deben ser codificados por las subclases.

## Interfaz

Una interfaz permite especificar un contrato (formado por un conjunto de operaciones). Las clases que adhieren a ese contrato deben implementar esos métodos.

```java
/** Implementing this interface allows an object to be the target of
 *  the "foreach" statement.
 * @since 1.5
 */
public interface Iterable<T> {
    /**
     * Returns an iterator over a set of elements of type T.
     * 
     * @return an Iterator.
     */
    Iterator<T> iterator();
}
```

En este ejemplo del lenguaje Java las clases que implementen la interfaz Iterable deben definir un método iterator(). Todos los métodos son abstractos, ya que la interfaz sólo declara la firma de cada uno de los métodos: nombre, parámetros que recibe, tipo que devuelve y excepciones que puede arrojar.

# Comparación

Tanto la clase abstracta como la interfaz definen un tipo, pero mientras que el objetivo al definir una clase abstracta es reutilizar comportamiento, en la interfaz la idea principal es posibilitar el polimorfismo entre clases que no tienen cosas en común. Dependiendo del lenguaje, es posible que tengamos restricciones para crear métodos o atributos sobre una interfaz.

Recordemos que en la mayoría de las tecnologías se trabaja únicamente con herencia simple (solamente es posible heredar de una única superclase), mientras que una clase puede tener múltiples tipos (implementar diferentes interfaces). Entonces en esos casos donde la herencia es una herramienta rígida, que permite una sola chance para ordenar la jerarquía de objetos, la interfaz puede resultar una alternativa viable.

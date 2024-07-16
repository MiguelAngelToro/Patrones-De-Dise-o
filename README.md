Patrón Creacional: Factory Method

Propósito del patrón:
El patrón Factory Method define una interfaz para crear un objeto, pero permite a las clases derivadas decidir qué clase instanciar. Esto permite la creación de objetos de una manera más flexible y controlada.

Situaciones útiles:
Es útil cuando se tiene una superclase con múltiples subclases y se quiere delegar la responsabilidad de instanciación a las subclases, según el contexto.

Ejemplo de código en Java:

java

// Product interface
interface Tarea {
    void realizar();
}

// Concrete Product
class TareaSimple implements Tarea {
    @Override
    public void realizar() {
        System.out.println("Realizando tarea simple...");
    }
}

// Concrete Product
class TareaCompleja implements Tarea {
    @Override
    public void realizar() {
        System.out.println("Realizando tarea compleja...");
    }
}

// Creator
abstract class CreadorDeTareas {
    public abstract Tarea crearTarea();
}

// Concrete Creator
class CreadorDeTareasSimples extends CreadorDeTareas {
    @Override
    public Tarea crearTarea() {
        return new TareaSimple();
    }
}

// Concrete Creator
class CreadorDeTareasComplejas extends CreadorDeTareas {
    @Override
    public Tarea crearTarea() {
        return new TareaCompleja();
    }
}

// Uso del patrón Factory Method
public class Main {
    public static void main(String[] args) {
        CreadorDeTareas creador = new CreadorDeTareasSimples();
        Tarea tarea = creador.crearTarea();
        tarea.realizar();
    }
}

Patrón Estructural: Decorator

Propósito del patrón:
El patrón Decorator permite añadir funcionalidades a objetos existentes dinámicamente, sin alterar su estructura básica. Esto se logra utilizando composición en lugar de herencia para extender la funcionalidad de los objetos.

Situaciones útiles:
Es útil cuando se desea agregar funcionalidades a objetos de manera flexible y modular, sin modificar su código.

Ejemplo de código en Java:

java

// Component interface
interface Tarea {
    void realizar();
}

// Concrete Component
class TareaSimple implements Tarea {
    @Override
    public void realizar() {
        System.out.println("Realizando tarea simple...");
    }
}

// Decorator
abstract class DecoradorTarea implements Tarea {
    protected Tarea tareaDecorada;

    public DecoradorTarea(Tarea tareaDecorada) {
        this.tareaDecorada = tareaDecorada;
    }

    public void realizar() {
        tareaDecorada.realizar();
    }
}

// Concrete Decorator
class TareaConPrioridad extends DecoradorTarea {
    public TareaConPrioridad(Tarea tareaDecorada) {
        super(tareaDecorada);
    }

    @Override
    public void realizar() {
        super.realizar();
        System.out.println(" - con prioridad alta");
    }
}

// Uso del patrón Decorator
public class Main {
    public static void main(String[] args) {
        Tarea tarea = new TareaSimple();
        tarea = new TareaConPrioridad(tarea);
        tarea.realizar();
    }
}

Patrón de Comportamiento: Observer

Propósito del patrón:
El patrón Observer define una dependencia uno a muchos entre objetos, de modo que cuando un objeto cambia su estado, todos los objetos dependientes son notificados y actualizados automáticamente.

Situaciones útiles:
Es útil cuando se necesita mantener la consistencia entre objetos relacionados sin acoplamiento fuerte, permitiendo que múltiples objetos reaccionen a cambios en un objeto observado.

Ejemplo de código en Java:

java

import java.util.ArrayList;
import java.util.List;

// Subject (Observable)
class Tarea {
    private List<Observador> observadores = new ArrayList<>();
    private String estado;

    public void agregarObservador(Observador observador) {
        observadores.add(observador);
    }

    public void eliminarObservador(Observador observador) {
        observadores.remove(observador);
    }

    public void setState(String estado) {
        this.estado = estado;
        notificarObservadores();
    }

    private void notificarObservadores() {
        for (Observador observador : observadores) {
            observador.actualizar(estado);
        }
    }
}

// Observer
interface Observador {
    void actualizar(String estado);
}

// Concrete Observer
class Usuario implements Observador {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    @Override
    public void actualizar(String estado) {
        System.out.println(nombre + " ha sido notificado con el estado: " + estado);
    }
}

// Uso del patrón Observer
public class Main {
    public static void main(String[] args) {
        Tarea tarea = new Tarea();
        tarea.agregarObservador(new Usuario("Juan"));
        tarea.agregarObservador(new Usuario("María"));

        tarea.setState("En progreso");
        tarea.setState("Completada");
    }
}

Conclusiones:

    Patrones Creacionales: El uso de Factory Method podría ayudar a gestionar la creación de diferentes tipos de tareas dentro de la aplicación.

    Patrones Estructurales: Decorator podría ser útil para añadir funcionalidades adicionales a las tareas, como etiquetas de prioridad o notas adicionales.

    Patrones de Comportamiento: Observer podría utilizarse para notificar a los usuarios interesados (como asignados a una tarea) cuando el estado de una tarea cambie, manteniendo la consistencia y actualización automática de la información.

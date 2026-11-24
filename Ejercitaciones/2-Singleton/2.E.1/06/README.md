# Ejercicio 6: Gestor de Temas y Apariencia Visual

## Explicación del Proyecto y Lógica
**Descripción:** Simulación de una aplicación de productividad con múltiples vistas (`VistaPrincipal` y `VistaConfiguracion`) que comparten una misma preferencia visual (`CLARO` u `OSCURO`) y una paleta de colores personalizable, reflejando los cambios de apariencia de forma inmediata en toda la interfaz.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** en la clase `GestorTemas` para centralizar el estado visual global de la aplicación. Al restringir la creación de instancias con un constructor privado, exponer el acceso único mediante `obtenerInstancia()` y encapsular las paletas de colores detrás de métodos controlados con validación (`cambiarTema`, `alternarTema`, `personalizarColor`), se evita que el estado visual se fragmente entre diferentes vistas o ventanas.

## Evidencia de Diseño (UML)
<img width="641" height="450" alt="imagen" src="https://github.com/user-attachments/assets/e6996983-667e-4c0e-a8ff-a0f976e3e6ee" />

## Fragmento de Código Clave
```java
public class GestorTemas {
    private static GestorTemas instancia;
    private String[][] paletas;
    private Tema temaActual;

    private GestorTemas() {
        paletas = new String[Tema.values().length][];
        paletas[Tema.CLARO.ordinal()] = new String[]{"#FFFFFF", "#000000", "#4A90E2"};
        paletas[Tema.OSCURO.ordinal()] = new String[]{"#121212", "#EEEEEE", "#BB86FC"};
        temaActual = Tema.CLARO;
        historial = new String[5];
        cantidadHistorial = 0;
    }

    public static synchronized GestorTemas obtenerInstancia() {
        if (instancia == null) {
            instancia = new GestorTemas();
        }
        return instancia;
    }
}
```

## Evidencia de Ejecución
<img width="711" height="610" alt="{F6B71B90-53CE-40BD-8297-D89158BA49CA}" src="https://github.com/user-attachments/assets/07dc7c02-9cc4-46df-9a6b-77c55f34386d" />

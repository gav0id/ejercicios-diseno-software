# Ejercicio 5: Gestor Concurrente de Reservas de una Sala de Lectura

## Explicación del Proyecto y Lógica
**Descripción:** Sistema concurrente para administrar los 10 lugares disponibles de una sala de lectura compartida por múltiples puestos de atención simulados mediante hilos. Permite registrar reservas por identificador de socio, rechazar duplicados o solicitudes sin cupo, cancelar reservas recuperando el lugar y consultar tanto las reservas activas como la disponibilidad actual.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** seguro ante concurrencia en `GestorReservasSala` para centralizar el control de los 10 cupos en una única instancia compartida entre todos los hilos, evitando sobreventa de lugares. El método `obtenerInstancia()` sincronizado garantiza la unicidad del objeto al inicializar, mientras que los métodos `synchronized` (`reservar`, `cancelar`, `obtenerReservas` y `lugaresDisponibles`) aseguran la atomicidad de las operaciones sobre el estado compartido y protegen el arreglo interno devolviendo copias defensivas.

## Evidencia de Diseño (UML)
<img width="482" height="204" alt="05 drawio" src="https://github.com/user-attachments/assets/8ce0dab8-64e5-4375-9d36-6c4feb6eaef2" />

## Fragmento de Código Clave
```java
public class GestorReservasSala {
    private static GestorReservasSala instancia;
    private final String[] reservas = new String[10];
    private int cantidad = 0;

    private GestorReservasSala() {}

    public static synchronized GestorReservasSala obtenerInstancia() {
        if (instancia == null) {
            instancia = new GestorReservasSala();
        }
        return instancia;
    }

    public synchronized boolean reservar(String idSocio) {
        if (idSocio == null || idSocio.isBlank() || buscar(idSocio) != -1 || cantidad >= 10) {
            return false;
        }
        reservas[cantidad++] = idSocio;
        return true;
    }
}
```

## Evidencia de Ejecución
<img width="489" height="274" alt="imagen" src="https://github.com/user-attachments/assets/278ead7c-05ad-496a-b65e-ea8ed2862615" />

# Ejercicio 9: Motor Avanzado de Eventos para Arquitectura de Microservicios Simulada

## Explicación del Proyecto y Lógica
**Descripción:** Bus de eventos centralizado (`BusEventos`) para una arquitectura de microservicios simulada donde distintos servicios (`ServicioBienvenida`, `ServicioFacturacion`, `ServicioAnalitica`) se suscriben a tipos de eventos (`Evento`) y reaccionan de forma desacoplada y tolerante a fallos.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante un bus central (`BusEventos`), la interfaz `SuscriptorEvento` y el `record` `Evento`. Las rutas se almacenan en un `ConcurrentHashMap<String, CopyOnWriteArrayList<SuscriptorEvento>>` indexado por tipo de evento para enrutar con rapidez y seguridad en entornos concurrentes, mientras que el `try-catch` por suscriptor impide que la excepción de un oyente interrumpa la notificación al resto.

## Evidencia de Diseño (UML)
<img width="882" height="394" alt="09 drawio" src="https://github.com/user-attachments/assets/b8466ec0-7380-4e62-82b7-18a7e16337d1" />

## Fragmento de Código Clave
```java
public record Evento(String tipo, String payload) {}

public interface SuscriptorEvento {
    void procesar(Evento evento);
}

public void publicar(Evento evento) {
    if (evento == null || evento.tipo() == null) return;
    CopyOnWriteArrayList<SuscriptorEvento> lista = rutas.get(evento.tipo().trim().toUpperCase());
    if (lista == null) return;

    for (SuscriptorEvento s : lista) {
        try {
            s.procesar(evento);
        } catch (Exception e) {
            System.out.println("BusEventos - error en suscriptor para " + evento.tipo() + " - " + e.getMessage());
        }
    }
}
```

## Evidencia de Ejecución
<img width="872" height="259" alt="{DD131650-6E43-46AF-892A-8126C3DAB466}" src="https://github.com/user-attachments/assets/befc4da3-7a4b-44dd-b9e4-e57ff2a4e34f" />

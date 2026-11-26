# Ejercicio 9: Fábrica Avanzada de Conectores IoT para Telemetría

## Explicación del Proyecto y Lógica
**Descripción:** Plataforma industrial de telemetría que inicializa y gestiona canales de comunicación IoT para múltiples protocolos (`MQTT`, `HTTP`, `CoAP` y protocolos registrados en caliente como `MODBUS`), validando credenciales y registrando métricas operativas concurrentes de éxitos y fallos por protocolo.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz `ConectorIoT` y el creador abstracto `CreadorConectorIoT`, cuyo método fábrica `crearConector(String credencial)` es sobrescrito por cada factoría concreta (`CreadorMQTT`, `CreadorHTTP`, `CreadorCoAP`) para validar seguridad, inicializar el conector y lanzar excepciones ante fallos. Sobre esta jerarquía, `GestorTelemetria` actúa como registro centralizado usando `ConcurrentHashMap` y `AtomicInteger`, permitiendo incorporar nuevos protocolos en tiempo de ejecución sin modificar el código base (principio Abierto/Cerrado).

## Evidencia de Diseño (UML)
![Diagrama de Clases UML](diagrama_uml.png)

## Fragmento de Código Clave
```java
public interface ConectorIoT {
    void conectar(String credencial);
    String leerTelemetria();
}

public abstract class CreadorConectorIoT {
    public abstract ConectorIoT crearConector(String credencial) throws Exception;
}

public class GestorTelemetria {
    private final ConcurrentHashMap<String, CreadorConectorIoT> fabricas = new ConcurrentHashMap<>();
    private final ConcurrentHashMap<String, AtomicInteger> metricasExito = new ConcurrentHashMap<>();
    private final ConcurrentHashMap<String, AtomicInteger> metricasFallo = new ConcurrentHashMap<>();

    public void registrarFabrica(String protocolo, CreadorConectorIoT creador) {
        fabricas.put(protocolo.toUpperCase(), creador);
        metricasExito.putIfAbsent(protocolo.toUpperCase(), new AtomicInteger(0));
        metricasFallo.putIfAbsent(protocolo.toUpperCase(), new AtomicInteger(0));
    }
}
```

## Evidencia de Ejecución

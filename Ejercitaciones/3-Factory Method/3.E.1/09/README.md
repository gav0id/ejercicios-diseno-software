# Ejercicio 9: Fábrica Avanzada de Conectores IoT para Telemetría

## Explicación del Proyecto y Lógica
**Descripción:** Plataforma industrial de telemetría que inicializa y gestiona canales de comunicación IoT para múltiples protocolos (`MQTT`, `HTTP`, `CoAP` y protocolos registrados en caliente como `MODBUS`), validando credenciales y registrando métricas operativas concurrentes de éxitos y fallos por protocolo.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz `ConectorIoT` y el creador abstracto `CreadorConectorIoT`, cuyo método fábrica `crearConector(String credencial)` es sobrescrito por cada factoría concreta (`CreadorMQTT`, `CreadorHTTP`, `CreadorCoAP`) para validar seguridad, inicializar el conector y lanzar excepciones ante fallos. Sobre esta jerarquía, `GestorTelemetria` actúa como registro centralizado usando `ConcurrentHashMap` y `AtomicInteger`, permitiendo incorporar nuevos protocolos en tiempo de ejecución sin modificar el código base (principio Abierto/Cerrado).

## Evidencia de Diseño (UML)
<img width="1122" height="506" alt="09 drawio" src="https://github.com/user-attachments/assets/48eae0dc-f1bb-42a3-9965-221b0b8fb3c0" />

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
<img width="699" height="406" alt="{ABB970FE-55C1-494C-960C-50177771D1EA}" src="https://github.com/user-attachments/assets/e13e4da5-0fd0-4b5b-9f9f-41023327e784" />

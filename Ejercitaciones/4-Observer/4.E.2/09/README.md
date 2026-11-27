# Ejercicio 9: Motor Asíncrono de Eventos para Monitoreo de Servidores

## Explicación del Proyecto y Lógica
**Descripción:** Bus de eventos centralizado (`BusAlertas`) para monitoreo de servidores donde distintos microservicios (`ServicioAutoEscalado`, `ServicioRespaldoDisco`, `ServicioNotificacionGuardia`) se suscriben a tipos de alertas (`CPU_ALTA`, `MEMORIA_INSUFICIENTE`, `FALLO_DISCO`) y procesan eventos (`AlertaSistema`) de forma asíncrona y tolerante a fallos.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** sobre un bus de eventos centralizado mediante la interfaz `SuscriptorAlerta`, el `record` inmutable `AlertaSistema` y el sujeto observable `BusAlertas`. Las suscripciones se indexan por tipo de alerta en un `ConcurrentHashMap<String, CopyOnWriteArrayList<SuscriptorAlerta>>` y se despachan a través de un `ExecutorService` con `try-catch` por oyente, logrando enrutamiento concurrente y aislamiento ante excepciones individuales.

## Evidencia de Diseño (UML)
<img width="882" height="414" alt="09 drawio" src="https://github.com/user-attachments/assets/b91a4c15-039f-4772-a0b4-50da567e2c62" />

## Fragmento de Código Clave
```java
public record AlertaSistema(String tipo, String severidad, String detalle) {}

public interface SuscriptorAlerta {
    void manejar(AlertaSistema alerta);
}

public void publicar(AlertaSistema alerta) {
    if (alerta == null || alerta.tipo() == null) return;
    CopyOnWriteArrayList<SuscriptorAlerta> lista = rutas.get(alerta.tipo().trim().toUpperCase());
    if (lista == null) return;

    for (SuscriptorAlerta s : lista) {
        executor.submit(() -> {
            try {
                s.manejar(alerta);
            } catch (Exception e) {
                System.out.println("BusAlertas - error en suscriptor para " + alerta.tipo()
                        + " - " + e.getMessage());
            }
        });
    }
}
```

## Evidencia de Ejecución
<img width="971" height="273" alt="{BCE8A24B-8DEC-4D6D-BE68-344E5E937C96}" src="https://github.com/user-attachments/assets/001ff6ce-7083-4587-80a0-b87e002e1848" />

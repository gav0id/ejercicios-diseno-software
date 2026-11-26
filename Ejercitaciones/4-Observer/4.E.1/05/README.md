# Ejercicio 5: Monitor de Sensores de Humos en Edificio Inteligente

## Explicación del Proyecto y Lógica
**Descripción:** Sistema de automatización contra incendios donde múltiples sensores de humo ubicados en distintas plantas del edificio (`SensorHumo`) detectan emergencias y alertan simultáneamente a la central de alarmas (`CentralAlarmas`), los rociadores automáticos (`RociadoresAutomaticos`) y el servicio de conserjería (`ServicioConserjeria`), enviando la ubicación exacta y el nivel de humo.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `SistemaRespuesta` y el sujeto observable `SensorHumo`. Cada sensor mantiene su propia lista de suscriptores (`List<SistemaRespuesta>`) y notifica a los sistemas vinculados mediante `detectarHumo(String nivelHumo)`. Esta arquitectura permite que múltiples sistemas escuchen una o varias fuentes de peligro en simultáneo y garantiza que la incorporación de nuevos dispositivos de respuesta no requiera modificar el código de los sensores.

## Evidencia de Diseño (UML)
<img width="882" height="299" alt="05 drawio" src="https://github.com/user-attachments/assets/13401463-a9d8-46a4-a5b3-202f64b26906" />

## Fragmento de Código Clave
```java
public interface SistemaRespuesta {
    void responder(String ubicacion, String nivelHumo);
}

public void detectarHumo(String nivelHumo) {
    System.out.println("Sensor [" + ubicacion + "] - humo detectado (" + nivelHumo + ")");
    for (SistemaRespuesta s : new ArrayList<>(sistemas)) {
        s.responder(ubicacion, nivelHumo);
    }
}
```

## Evidencia de Ejecución
<img width="740" height="311" alt="{9CCB5AD7-93DE-47AA-AA6F-0FFFA8F58797}" src="https://github.com/user-attachments/assets/1723eb02-196f-4f98-babd-ddcfce1be1e7" />

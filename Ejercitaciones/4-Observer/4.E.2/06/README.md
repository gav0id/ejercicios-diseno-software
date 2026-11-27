# Ejercicio 6: Panel Financiero de Variación de Divisas

## Explicación del Proyecto y Lógica
**Descripción:** Plataforma de cambio de moneda (`MonitorDivisas`) que procesa cotizaciones en tiempo real (`USD`, `EUR`), calcula la variación porcentual respecto al ciclo anterior y notifica de forma inmediata a distintas herramientas de visualización (`GraficoTendencia`, `TablaConversion` y `AlarmaLimite`) mediante el objeto `CotizacionDivisa`.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `PanelOperador`, el `record` inmutable `CotizacionDivisa` y el sujeto observable `MonitorDivisas`. `MonitorDivisas` centraliza la actualización de valores en un `ConcurrentHashMap` y gestiona los paneles con `CopyOnWriteArrayList`, propagando el evento a todos los suscriptores sin acoplarse a la lógica de cada panel ni sufrir fallos por modificaciones concurrentes.

## Evidencia de Diseño (UML)
<img width="882" height="414" alt="06 drawio" src="https://github.com/user-attachments/assets/b0724e56-d49b-43cc-8382-87bc58e6d6ff" />

## Fragmento de Código Clave
```java
public record CotizacionDivisa(String divisa, double anterior, double actual, double variacionPct) {}

public interface PanelOperador {
    void actualizar(CotizacionDivisa cotizacion);
}

public synchronized void actualizarCotizacion(String divisa, double nuevoValor) {
    if (divisa == null || divisa.isBlank() || nuevoValor <= 0) return;
    String clave = divisa.trim().toUpperCase();
    double anterior = cotizaciones.getOrDefault(clave, nuevoValor);
    double variacionPct = ((nuevoValor - anterior) / anterior) * 100.0;
    cotizaciones.put(clave, nuevoValor);

    CotizacionDivisa evento = new CotizacionDivisa(clave, anterior, nuevoValor, variacionPct);
    for (PanelOperador p : paneles) {
        p.actualizar(evento);
    }
}
```

## Evidencia de Ejecución
<img width="772" height="313" alt="imagen" src="https://github.com/user-attachments/assets/d17ba902-a467-4b2c-9817-24fc21765d55" />

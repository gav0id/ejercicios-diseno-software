# Ejercicio 10: Sistema Empresarial de Notificación de Riesgos y Cumplimiento Legal

## Explicación del Proyecto y Lógica
**Descripción:** Sistema bancario de gestión de riesgos (`GestorRiesgoCliente`) que registra cambios en el perfil de riesgo de clientes, notifica de forma controlada a subsistemas regulatorios (`CumplimientoLegal`, `PrevencionLavado` y `ReportesRegulatorios`) y genera un historial inmutable (`RegistroAuditoriaRiesgo`) con las notificaciones exitosas y las incidencias ocurridas.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** con aislamiento de fallos y seguridad multihilo mediante la interfaz `ObservadorRegulatorio`, el `record` inmutable `RegistroAuditoriaRiesgo` y el sujeto observable `GestorRiesgoCliente`. Cada llamada a `evaluarCambio(...)` se ejecuta dentro de un `try-catch` individual para evitar fallos en cascada, mientras que `CopyOnWriteArrayList`, `ConcurrentHashMap` y `List.copyOf` garantizan integridad y trazabilidad en escenarios concurrentes.

## Evidencia de Diseño (UML)
<img width="922" height="540" alt="10 drawio" src="https://github.com/user-attachments/assets/8c193934-b666-4cb6-a0d0-9e5a6308b718" />

## Fragmento de Código Clave
```java
public record RegistroAuditoriaRiesgo(
    String cliente,
    String riesgoAnterior,
    String riesgoNuevo,
    List<String> exitosos,
    List<String> incidencias
) {
    public RegistroAuditoriaRiesgo {
        exitosos = List.copyOf(exitosos);
        incidencias = List.copyOf(incidencias);
    }
}

for (ObservadorRegulatorio o : observadores) {
    try {
        o.evaluarCambio(cliente, anterior, actual);
        exitosos.add(o.getNombre());
    } catch (Exception e) {
        incidencias.add(o.getNombre() + ": " + e.getMessage());
    }
}
```

## Evidencia de Ejecución
<img width="1263" height="331" alt="{F5A9B19F-C8BE-4A0F-8A9A-ACF9F217CD65}" src="https://github.com/user-attachments/assets/18022f75-cb01-4f3b-80f2-fa9e86cfd680" />

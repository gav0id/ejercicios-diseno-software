# Ejercicio 10: Sistema Empresarial de Cumplimiento Normativo y Auditoría de Riesgos

## Explicación del Proyecto y Lógica
**Descripción:** Gestor bancario de riesgo corporativo (`GestorRiesgoCliente`) que audita cambios en el nivel de riesgo de cuentas empresariales (`ALTO`, `CRITICO`) a través de `PrevencionLavado`, `AnalisisLegal` y `OrganismosInternacionales`, generando un registro inmutable (`AuditoriaCumplimiento`) con entidades exitosas e incidencias.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `EntidadRegulatoria`, el `record` inmutable `AuditoriaCumplimiento` y el sujeto observable `GestorRiesgoCliente`. Cada cambio de riesgo aísla las excepciones por observador mediante `try-catch` para evitar fallos en cascada, usa estructuras concurrentes (`CopyOnWriteArrayList`, `ConcurrentHashMap`) y devuelve colecciones inmutables (`List.copyOf`) para garantizar trazabilidad segura en entornos multihilo.

## Evidencia de Diseño (UML)
<img width="882" height="589" alt="10 drawio" src="https://github.com/user-attachments/assets/ce722944-cee3-447b-96f1-a1672bb1bea7" />

## Fragmento de Código Clave
```java
public record AuditoriaCumplimiento(
    String cuentaCorporativa,
    String riesgoAnterior,
    String riesgoNuevo,
    List<String> exitosas,
    List<String> incidencias
) {
    public AuditoriaCumplimiento {
        exitosas = List.copyOf(exitosas);
        incidencias = List.copyOf(incidencias);
    }
}

public interface EntidadRegulatoria {
    String getNombre();
    void auditar(String cuentaCorporativa, String riesgoAnterior, String riesgoNuevo);
}

for (EntidadRegulatoria e : observadores) {
    try {
        e.auditar(cuentaCorporativa, anterior, actual);
        exitosas.add(e.getNombre());
    } catch (Exception ex) {
        incidencias.add(e.getNombre() + ": " + ex.getMessage());
    }
}
```

## Evidencia de Ejecución
<img width="1261" height="329" alt="imagen" src="https://github.com/user-attachments/assets/f274e3c7-7fb1-4b3a-939e-1e337906f493" />

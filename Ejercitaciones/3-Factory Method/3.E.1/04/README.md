# Ejercicio 4: Fábrica de Exportadores de Reportes Financieros

## Explicación del Proyecto y Lógica
**Descripción:** Sistema contable que genera informes financieros estructurados con encabezados y datos tabulares en formato PDF y CSV. Antes de exportar, aplica una validación general y delega en cada exportador las reglas específicas de su formato (por ejemplo, impedir celdas con comas en CSV).

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz `ExportadorReporte` y la clase creadora abstracta `CreadorExportador`. El método plantilla `generarReporte()` centraliza el flujo fijo (validación de datos no vacíos, obtención del exportador mediante el método fábrica abstracto `crearExportador()`, validación específica del formato y exportación), mientras que las subclases `CreadorPDF` y `CreadorCSV` encapsulan la instanciación concreta de cada formato sin acoplar al cliente.

## Evidencia de Diseño (UML)
<img width="562" height="404" alt="04 drawio" src="https://github.com/user-attachments/assets/ae4dbba2-4ed2-4fcc-810f-29928000b07a" />

## Fragmento de Código Clave
```java
public interface ExportadorReporte {
    boolean validar(String[] encabezados, String[][] filas);
    String exportar(String[] encabezados, String[][] filas);
}

public abstract class CreadorExportador {
    public abstract ExportadorReporte crearExportador();

    public String generarReporte(String[] encabezados, String[][] filas) {
        if (encabezados == null || encabezados.length == 0 || filas == null || filas.length == 0) {
            return "RECHAZADO - reporte sin datos";
        }
        ExportadorReporte exportador = crearExportador();
        if (!exportador.validar(encabezados, filas)) {
            return "RECHAZADO - formato de datos invalido";
        }
        return exportador.exportar(encabezados, filas);
    }
}

public class CreadorCSV extends CreadorExportador {
    @Override
    public ExportadorReporte crearExportador() {
        return new ExportadorCSV();
    }
}
```

## Evidencia de Ejecución
<img width="573" height="355" alt="{E741E43A-5767-4484-9FE7-0B30ADBF037D}" src="https://github.com/user-attachments/assets/6f3721ab-7522-47a3-8083-a0ae52b43d6a" />

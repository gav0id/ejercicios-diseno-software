# Ejercicio 8: Refactorización de Código Condicional hacia Factory Method

## Explicación del Proyecto y Lógica
**Descripción:** Refactorización de un módulo de importación de datos (`CSV`, `XML` y `JSON`) que elimina estructuras condicionales rígidas de instanciación directa y desacopla la lógica de procesamiento principal (`ProcesadorDatos`) de las implementaciones concretas de cada formato.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz `ImportadorDatos` y la clase creadora abstracta `CreadorImportador`, que declara el método fábrica `crearImportador()`. Las subclases `CreadorCSV`, `CreadorXML` y `CreadorJSON` encapsulan la instanciación de su importador correspondiente. Al inyectar `CreadorImportador` en `ProcesadorDatos` por constructor, se logra inversión de control y cumplimiento estricto del principio de responsabilidad única (SRP), separando la selección/creación del formato de la lógica de lectura y procesamiento.

## Evidencia de Diseño (UML)
<img width="1202" height="426" alt="08 drawio" src="https://github.com/user-attachments/assets/731cc422-bfe5-4a45-b91d-27f81ee53172" />

## Fragmento de Código Clave
```java
public interface ImportadorDatos {
    String importar(String archivo);
}

public abstract class CreadorImportador {
    public abstract ImportadorDatos crearImportador();
}

public class ProcesadorDatos {
    private final CreadorImportador creador;

    public ProcesadorDatos(CreadorImportador creador) {
        this.creador = creador;
    }

    public void procesar(String archivo) {
        ImportadorDatos importador = creador.crearImportador();
        System.out.println("Datos procesados - " + importador.importar(archivo));
    }
}
```

## Evidencia de Ejecución
<img width="663" height="250" alt="{1BC78653-A03B-48EA-B3BA-DDDA1725C03D}" src="https://github.com/user-attachments/assets/76f8c617-9f2f-4b30-827b-b36692275d76" />

# Ejercicio 9: Sistema de Monitoreo de Métricas de Rendimiento

## Explicación del Proyecto y Lógica
**Descripción:** Sistema transversal de monitoreo que recopila contadores de operaciones y tiempos de latencia (cantidad, promedio y máximo) enviados desde distintos componentes (`InterceptorRed` y `ModuloBaseDatos`), consolidando todo en un reporte exportable y archivando métricas anteriores cuando se alcanza el umbral de capacidad.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** con *Double-Checked Locking* (`volatile` + bloque `synchronized` solo en la inicialización) en `ColectorMetricas` para centralizar la agregación de métricas sin generar cuellos de botella por bloqueos globales. Además, el uso de un objeto `candado` privado sincroniza únicamente las secciones críticas de escritura/lectura, mientras que la estrategia interna de archivado (`archivarYLimpiar`) controla la capacidad en memoria para evitar desbordamientos bajo alta carga operativa.

## Evidencia de Diseño (UML)
<img width="478" height="349" alt="{1DD85F35-D2B8-4CE4-8B77-70D5BAE835E1}" src="https://github.com/user-attachments/assets/f7f486cf-1702-4480-9ceb-5eda89d5edba" />

## Fragmento de Código Clave
```java
public class ColectorMetricas {
    private static volatile ColectorMetricas instancia;
    private final Object candado = new Object();

    private ColectorMetricas() {
        nombresContadores = new String[MAX_METRICAS];
        valoresContadores = new long[MAX_METRICAS];
        nombresLatencias = new String[MAX_METRICAS];
        cantidadMediciones = new long[MAX_METRICAS];
        sumaLatencias = new long[MAX_METRICAS];
        maximoLatencias = new long[MAX_METRICAS];
        archivo = new String[MAX_ARCHIVO];
    }

    public static ColectorMetricas obtenerInstancia() {
        if (instancia == null) {
            synchronized (ColectorMetricas.class) {
                if (instancia == null) {
                    instancia = new ColectorMetricas();
                }
            }
        }
        return instancia;
    }
}
```

## Evidencia de Ejecución
<img width="529" height="320" alt="{0527EEF5-9F41-4F23-98DD-82DBD4CDE1D0}" src="https://github.com/user-attachments/assets/c2844744-83a5-4062-aae7-291c0bb67d3d" />

# Ejercicio 7: Cola de Impresión Centralizada

## Explicación del Proyecto y Lógica
**Descripción:** Simulación de una central de impresión compartida en red donde múltiples terminales (`TerminalVentas` y `TerminalAdministracion`) envían documentos con metadatos (`nombreArchivo` y `autor`) para ser procesados en orden estricto de llegada (FIFO), gestionando una cola principal y una lista de espera auxiliar.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** en la clase `ColaImpresion` para centralizar la gestión de trabajos de impresión en una única instancia compartida. Al encapsular la estructura de cola dentro del Singleton, restringir el constructor y sincronizar (`synchronized`) tanto `obtenerInstancia()` como las operaciones de encolado y desencolado, se previenen condiciones de carrera, pérdida de documentos o duplicación de tareas ante peticiones concurrentes.

## Evidencia de Diseño (UML)
<img width="647" height="420" alt="{E464EA1A-3C58-4114-8F9E-6C0B97F25031}" src="https://github.com/user-attachments/assets/ca6b7c9e-4069-4776-bf74-a69b9c91af19" />

## Fragmento de Código Clave
```java
public class ColaImpresion {
    private static ColaImpresion instancia;
    private Documento[] cola;
    private int inicio;
    private int fin;
    private int cantidad;

    private ColaImpresion() {
        cola = new Documento[3];
        espera = new Documento[2];
        inicio = 0;
        fin = 0;
        cantidad = 0;
        cantidadEspera = 0;
    }

    public static synchronized ColaImpresion obtenerInstancia() {
        if (instancia == null) {
            instancia = new ColaImpresion();
        }
        return instancia;
    }
}
```

## Evidencia de Ejecución
![Sistema funcionando](ejecucion.png)
<img width="615" height="405" alt="{A90B279B-34DE-4407-A058-13E50F5ADFB9}" src="https://github.com/user-attachments/assets/7c7709e5-d0c1-4cfb-863b-3010427ffaf4" />

# Ejercicio 9: Cola Compartida de Impresión Administrativa

## Explicación del Proyecto y Lógica
**Descripción:** Sistema concurrente para gestionar la impresora administrativa compartida por los sectores de recepción, facturación y archivo de un hospital. Recibe documentos con identificador único, nombre, sector y cantidad de páginas desde múltiples hilos en paralelo, los procesa en estricto orden de llegada (FIFO) simulando la impresión por consola y emite un informe final con todos los trabajos procesados.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** en `GestorImpresion` para garantizar que todos los hilos operen sobre una única cola compartida, evitando pérdidas o duplicaciones de trabajos. El diseño diferencia claramente la sincronización de creación —resuelta con *Double-Checked Locking* y `volatile` en `obtenerInstancia()`— de la sincronización del estado interno, donde todas las operaciones sobre la cola y el registro de procesados se protegen mediante un objeto `candado` privado.

## Evidencia de Diseño (UML)
<img width="602" height="404" alt="09 drawio" src="https://github.com/user-attachments/assets/93627a11-3eec-4047-b824-ce57d4688e17" />

## Fragmento de Código Clave
```java
public final class GestorImpresion {
    private static volatile GestorImpresion instancia;
    private final Object candado = new Object();

    private GestorImpresion() {}

    public static GestorImpresion obtenerInstancia() {
        if (instancia == null) {
            synchronized (GestorImpresion.class) {
                if (instancia == null) {
                    instancia = new GestorImpresion();
                }
            }
        }
        return instancia;
    }

    public boolean encolar(Documento doc) {
        if (doc == null || doc.getId() == null || doc.getId().isBlank()) return false;
        synchronized (candado) {
            if (existeId(doc.getId())) return false;
            if (fin == cola.length) {
                cola = agrandar(cola, inicio, fin);
                fin -= inicio;
                inicio = 0;
            }
            cola[fin++] = doc;
            return true;
        }
    }
}
```

## Evidencia de Ejecución
<img width="648" height="603" alt="{AE63CB3C-7309-4E94-86BF-38429D7C71E1}" src="https://github.com/user-attachments/assets/28f9d4d1-5c7e-43b0-beb7-f83a2675690a" />

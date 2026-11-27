# Ejercicio 8: Refactorización de Código Acoplado hacia Bus de Eventos

## Explicación del Proyecto y Lógica
**Descripción:** Refactorización de una aplicación de gestión de inventarios donde `ProductoInventario` deja de invocar directamente a los módulos de facturación y estadísticas, pasando a actuar como sujeto observable que notifica cambios de stock (`VENTA`, `REPOSICION`) a observadores independientes (`ModuloFacturacion` y `ModuloEstadisticas`).

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `ObservadorInventario` y el sujeto observable `ProductoInventario`. Al reemplazar las llamadas directas por una lista de observadores tipada con la interfaz e inversión de control, `ProductoInventario` conserva únicamente la responsabilidad de administrar su stock y notificar mediante `notificar()`, aislando el núcleo de cambios en los módulos receptores.

## Evidencia de Diseño (UML)
<img width="882" height="282" alt="08 drawio" src="https://github.com/user-attachments/assets/fed691fe-3d3f-4464-bd02-0e3c3b9ba8a7" />

## Fragmento de Código Clave
```java
public interface ObservadorInventario {
    void actualizar(String producto, int stockAnterior, int stockNuevo, String operacion);
}

public void modificarStock(int delta, String operacion) {
    if (delta == 0 || stock + delta < 0) return;
    int anterior = this.stock;
    this.stock += delta;
    notificar(anterior, this.stock, operacion);
}

private void notificar(int anterior, int actual, String operacion) {
    for (ObservadorInventario o : new ArrayList<>(observadores)) {
        o.actualizar(nombre, anterior, actual, operacion);
    }
}
```

## Evidencia de Ejecución
<img width="788" height="265" alt="{14584E75-2629-4806-859B-ACDE0E65C9DE}" src="https://github.com/user-attachments/assets/56ef1321-d9c0-4e50-98f9-5bf7bfa9dd6c" />

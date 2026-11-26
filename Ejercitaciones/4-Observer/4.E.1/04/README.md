# Ejercicio 4: Sistema de Alertas de Stock Bajo para Comercio Electrónico

## Explicación del Proyecto y Lógica
**Descripción:** Sistema de inventario para comercio electrónico que controla el stock de un producto (`ProductoInventario`) ante operaciones de venta y notifica automáticamente al departamento de compras (`DepartamentoCompras`) y al sistema de marketing (`SistemaMarketing`) únicamente cuando el stock desciende por debajo de un umbral mínimo preestablecido.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `ObservadorStock` y el sujeto observable `ProductoInventario`. El producto encapsula su estado interno (`nombre`, `stock`, `umbralMinimo`) y evalúa la regla de negocio tras cada venta (`stock < umbralMinimo`) para emitir notificaciones selectivas. Los observadores reciben los datos del evento a través de `actualizar(producto, stockActual, umbralMinimo)` y deciden de forma independiente qué acción ejecutar, manteniendo desacoplada la gestión de inventario de la lógica de compras y marketing.

## Evidencia de Diseño (UML)
<img width="882" height="194" alt="04 drawio" src="https://github.com/user-attachments/assets/7312241a-a022-4d95-b547-82efc03836b9" />

## Fragmento de Código Clave
```java
public interface ObservadorStock {
    void actualizar(String producto, int stockActual, int umbralMinimo);
}

public void vender(int cantidad) {
    if (cantidad <= 0 || cantidad > stock) {
        System.out.println("Venta rechazada - cantidad invalida (" + cantidad + ") para " + nombre);
        return;
    }
    stock -= cantidad;
    System.out.println("Venta - " + cantidad + "x " + nombre + " - stock restante: " + stock);
    if (stock < umbralMinimo) {
        notificar();
    }
}

private void notificar() {
    for (ObservadorStock o : new ArrayList<>(observadores)) {
        o.actualizar(nombre, stock, umbralMinimo);
    }
}
```

## Evidencia de Ejecución
<img width="824" height="338" alt="{74F6A5A2-DD29-42FC-9AA8-120636EC6FCD}" src="https://github.com/user-attachments/assets/435c1e50-9ead-43f0-8a71-62c79d49c510" />

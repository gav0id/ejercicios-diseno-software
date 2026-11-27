# Ejercicio 4: Sistema de Alertas de Precio en Lista de Deseos

## Explicación del Proyecto y Lógica
**Descripción:** Catálogo de productos (`ProductoCatalogo`) para tienda en línea que notifica automáticamente a canales suscritos (`NotificadorCorreo` y `PanelNotificacionesWeb`) únicamente cuando el precio de un artículo baja respecto al valor anterior, destacando rebajas iguales o superiores al 20%.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `ObservadorPrecio` y el sujeto observable `ProductoCatalogo`. El producto encapsula su estado comercial (`nombre`, `precio`) y su colección de canales (`List<ObservadorPrecio>`), evaluando la condición `nuevoPrecio < anterior` antes de notificar con copia defensiva (`new ArrayList<>(canales)`), permitiendo que cada observador reaccione según el porcentaje de rebaja sin acoplarse al catálogo.

## Evidencia de Diseño (UML)
<img width="962" height="284" alt="04 drawio" src="https://github.com/user-attachments/assets/57b91490-a53a-49bb-8b44-b5102ba48187" />

## Fragmento de Código Clave
```java
public interface ObservadorPrecio {
    void actualizar(String producto, double precioAnterior, double precioNuevo, double porcentajeRebaja);
}

public void actualizarPrecio(double nuevoPrecio) {
    if (nuevoPrecio <= 0) {
        System.out.println("Catalogo - precio invalido rechazado para " + nombre);
        return;
    }
    double anterior = this.precio;
    this.precio = nuevoPrecio;
    if (nuevoPrecio < anterior) {
        double rebaja = ((anterior - nuevoPrecio) / anterior) * 100.0;
        notificar(anterior, nuevoPrecio, rebaja);
    }
}
```

## Evidencia de Ejecución
<img width="954" height="274" alt="imagen" src="https://github.com/user-attachments/assets/de65043e-6896-4f07-9de0-917e02c4c142" />

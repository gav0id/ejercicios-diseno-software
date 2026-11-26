# Ejercicio 6: Factoría de Métodos de Pago para Comercio Electrónico

## Explicación del Proyecto y Lógica
**Descripción:** Plataforma de comercio electrónico que procesa transacciones a través de distintas pasarelas de pago (Tarjeta de Crédito con 5% de recargo, Transferencia Bancaria sin recargo y Monedero Digital con 2% de comisión), seleccionando dinámicamente la pasarela según los datos de la `OrdenCompra` y devolviendo un comprobante de transacción.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz común `PasarelaPago` y la clase creadora abstracta `CreadorPago`. El método `procesarOrden(OrdenCompra)` valida primero que el monto cumpla con el mínimo definido por cada subclase (`montoMinimo()`) y, recién si es válido, invoca el método fábrica abstracto `crearPasarela()` para ejecutar el cobro. De esta forma, `CreadorTarjeta`, `CreadorTransferencia` y `CreadorMonedero` encapsulan tanto la instanciación de su pasarela como sus reglas de mínimo, manteniendo al cliente desacoplado de las implementaciones concretas.

## Evidencia de Diseño (UML)
![Diagrama de Clases UML](diagrama_uml.png)

## Fragmento de Código Clave
```java
public interface PasarelaPago {
    String cobrar(double monto);
}

public abstract class CreadorPago {
    public abstract PasarelaPago crearPasarela();
    protected abstract double montoMinimo();

    public String procesarOrden(OrdenCompra orden) {
        if (orden == null || orden.monto() < montoMinimo()) {
            return String.format("RECHAZADO - monto minimo para %s: $%.2f", orden.medio(), montoMinimo());
        }
        return crearPasarela().cobrar(orden.monto());
    }

    public static CreadorPago seleccionar(OrdenCompra orden) {
        if (orden.medio().equalsIgnoreCase("tarjeta")) return new CreadorTarjeta();
        if (orden.medio().equalsIgnoreCase("transferencia")) return new CreadorTransferencia();
        if (orden.medio().equalsIgnoreCase("monedero")) return new CreadorMonedero();
        throw new IllegalArgumentException("Medio no soportado: " + orden.medio());
    }
}
```

## Evidencia de Ejecución

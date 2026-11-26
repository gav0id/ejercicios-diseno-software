# Ejercicio 4: Fábrica de Generadores de Facturas Comerciales

## Explicación del Proyecto y Lógica
**Descripción:** Sistema de ventas en línea que emite comprobantes de pago en distintos formatos estructurados (`Factura Estándar PDF` con IVA 21% y `Ticket Simplificado de Caja` con impuesto 10% y tope de $100.000), validando previamente los datos monetarios antes de generar el comprobante.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz `GeneradorFactura` y la clase creadora abstracta `CreadorFactura`. El método `emitirComprobante()` combina la validación estructural inicial con la llamada al método fábrica abstracto `crearGenerador()`, delegando luego la validación monetaria específica (`validar()`) y el formateo (`generar()`) en el producto concreto instanciado por `CreadorFacturaPDF` o `CreadorTicketCaja`.

## Evidencia de Diseño (UML)
<img width="1202" height="267" alt="04 drawio" src="https://github.com/user-attachments/assets/f9700afc-83c5-4d1f-a69d-6095da73c998" />

## Fragmento de Código Clave
```java
public interface GeneradorFactura {
    boolean validar(String[] conceptos, double[] importes);
    String generar(String[] conceptos, double[] importes);
}

public abstract class CreadorFactura {
    public abstract GeneradorFactura crearGenerador();

    public String emitirComprobante(String[] conceptos, double[] importes) {
        if (conceptos == null || importes == null || conceptos.length == 0 || conceptos.length != importes.length) {
            return "RECHAZADO - comprobante sin items validos";
        }
        GeneradorFactura generador = crearGenerador();
        if (!generador.validar(conceptos, importes)) {
            return "RECHAZADO - datos monetarios invalidos";
        }
        return generador.generar(conceptos, importes);
    }
}
```

## Evidencia de Ejecución
<img width="674" height="410" alt="imagen" src="https://github.com/user-attachments/assets/19fba6d1-8444-4201-bb59-9ce7103cfdb1" />

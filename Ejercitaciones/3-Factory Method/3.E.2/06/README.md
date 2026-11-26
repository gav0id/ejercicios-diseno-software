# Ejercicio 6: Factoría de Métodos de Envío Logístico

## Explicación del Proyecto y Lógica
**Descripción:** Plataforma de comercio electrónico que calcula costos y tiempos estimados de envío utilizando distintos transportistas (`Correo Postal Tradicional`, `Envío Expreso Privado` con 15% de recargo y `Retiro en Sucursal` sin costo), seleccionando dinámicamente el servicio según los datos de la `OrdenEnvio`.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz `ServicioEnvio` y la clase creadora abstracta `CreadorEnvio`. El método `procesarEnvio(OrdenEnvio)` valida los datos generales y los requisitos geográficos propios de cada transportista (`validarDestino(OrdenEnvio)`) antes de invocar el método fábrica abstracto `crearServicio()`. De esta forma, `CreadorCorreoPostal`, `CreadorExpresoPrivado` y `CreadorRetiroSucursal` encapsulan tanto sus validaciones de destino como la instanciación del servicio concreto, manteniendo al cliente desacoplado.

## Evidencia de Diseño (UML)
<img width="1122" height="546" alt="06 drawio" src="https://github.com/user-attachments/assets/787bfacf-e057-40bb-b66e-5d49f0b18ca9" />

## Fragmento de Código Clave
```java
public interface ServicioEnvio {
    String calcularEnvio(OrdenEnvio orden);
}

public abstract class CreadorEnvio {
    public abstract ServicioEnvio crearServicio();
    protected abstract boolean validarDestino(OrdenEnvio orden);

    public String procesarEnvio(OrdenEnvio orden) {
        if (orden == null || orden.destino() == null || orden.destino().isBlank() || orden.pesoKg() <= 0) {
            return "RECHAZADO - datos de envio invalidos";
        }
        if (!validarDestino(orden)) {
            return "RECHAZADO - destino no cumple requisitos para " + orden.metodo() + " (" + orden.destino() + ")";
        }
        return crearServicio().calcularEnvio(orden);
    }

    public static CreadorEnvio seleccionar(OrdenEnvio orden) {
        if (orden.metodo().equalsIgnoreCase("postal")) return new CreadorCorreoPostal();
        if (orden.metodo().equalsIgnoreCase("expreso")) return new CreadorExpresoPrivado();
        if (orden.metodo().equalsIgnoreCase("sucursal")) return new CreadorRetiroSucursal();
        throw new IllegalArgumentException("Metodo de envio no soportado: " + orden.metodo());
    }
}
```

## Evidencia de Ejecución
<img width="934" height="266" alt="{BF859A49-3048-4DA4-8CEE-7BA907D6E2E1}" src="https://github.com/user-attachments/assets/b16a221b-4e5a-4851-bf79-82872ea2aaa8" />

# Ejercicio 8: Refactorización hacia Inyección de Dependencias

## Explicación del Proyecto y Lógica
**Descripción:** Refactorización de un sistema de cobros (`ServicioCheckout` y `ServicioSuscripciones`) que anteriormente dependía de llamadas estáticas rígidas a un Singleton de pasarela de pago. Mediante la introducción de la interfaz `PasarelaPago`, los servicios reciben la dependencia por constructor, permitiendo operar en producción con la instancia única real (`PasarelaPagoReal`) y en pruebas con un doble de prueba (`PasarelaPagoFalsa`).

**Lenguaje:** Java

**Justificación Lógica (Singleton + Inyección de Dependencias):**  
Se mantuvo el patrón **Singleton** en `PasarelaPagoReal` para garantizar que exista una única instancia operativa de la pasarela real durante todo el proceso. Sin embargo, para eliminar el acoplamiento fuerte y facilitar las pruebas unitarias, se aplicó el **Principio de Inversión de Dependencias (DIP)** mediante la interfaz `PasarelaPago` e **Inyección de Dependencias por constructor**, logrando que ninguna clase consumidora invoque llamadas globales estáticas en su interior.

## Evidencia de Diseño (UML)
<img width="585" height="319" alt="{6F3B49C2-B735-4F46-9244-0B5AD635B711}" src="https://github.com/user-attachments/assets/544eb4a2-6db4-44b7-a42f-e45177c5dab1" />

## Fragmento de Código Clave
```java
public interface PasarelaPago {
    String procesarPago(String cliente, double monto);
}

public final class PasarelaPagoReal implements PasarelaPago {
    private static PasarelaPagoReal instancia;

    private PasarelaPagoReal() {
        historial = new String[5];
        cantidadHistorial = 0;
    }

    public static synchronized PasarelaPagoReal obtenerInstancia() {
        if (instancia == null) {
            instancia = new PasarelaPagoReal();
        }
        return instancia;
    }
}

public class ServicioCheckout {
    private final PasarelaPago pasarela;

    public ServicioCheckout(PasarelaPago pasarela) {
        this.pasarela = pasarela;
    }
}
```

## Evidencia de Ejecución
<img width="459" height="324" alt="{61A07A6F-C160-4B45-BB93-6AF00C7C0CF3}" src="https://github.com/user-attachments/assets/e4e35aa6-9a0c-4799-a99c-ad296c722ffe" />

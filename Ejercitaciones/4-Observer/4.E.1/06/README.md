# Ejercicio 6: Panel de Cotizaciones Bursátiles en Tiempo Real

## Explicación del Proyecto y Lógica
**Descripción:** Plataforma financiera que procesa las actualizaciones de precio de un activo bursátil (`CotizacionBursatil`), calcula automáticamente su variación porcentual y notifica en tiempo real a distintos paneles de inversores (`GraficoLineas`, `TablaResumen` y `AlertaVolatilidad`).

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `PanelInversor` y el sujeto observable `CotizacionBursatil`. El sujeto mantiene la lista de suscriptores en una colección segura ante concurrencia (`CopyOnWriteArrayList<PanelInversor>`) y, al ejecutarse `actualizarPrecio(double nuevoPrecio)`, propaga los datos primitivos actualizados a cada panel sin crear objetos adicionales en memoria ni acoplarse a la lógica de visualización de cada componente.

## Evidencia de Diseño (UML)
<img width="882" height="309" alt="06 drawio" src="https://github.com/user-attachments/assets/72cec81e-bd20-40c2-acd8-e986b54b8b2c" />

## Fragmento de Código Clave
```java
public interface PanelInversor {
    void actualizar(String simbolo, double precioAnterior, double precioActual, double variacionPct);
}

public synchronized void actualizarPrecio(double nuevoPrecio) {
    if (nuevoPrecio <= 0) return;
    double anterior = this.precioActual;
    double variacionPct = ((nuevoPrecio - anterior) / anterior) * 100.0;
    this.precioActual = nuevoPrecio;

    for (PanelInversor p : paneles) {
        p.actualizar(simbolo, anterior, nuevoPrecio, variacionPct);
    }
}
```

## Evidencia de Ejecución
<img width="659" height="336" alt="imagen" src="https://github.com/user-attachments/assets/0c347eea-d3fd-40cc-9f7e-0358be681235" />

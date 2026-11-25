# Ejercicio 7: Generador Central de Números de Atención

## Explicación del Proyecto y Lógica
**Descripción:** Sistema de emisión de turnos para la recepción de un hospital donde tres terminales (`TerminalAtencion`) solicitan números consecutivos de atención a partir de 1. Centraliza la numeración, registra desde qué terminal se pidió cada número, permite consultar el último emitido sin alterar el contador y muestra el historial completo.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** en `GeneradorTurnos` para concentrar el contador y el historial en una única instancia compartida por las tres terminales, evitando que cada puesto lleve su propia numeración y genere números repetidos. Al mantener el constructor y el contador privados, las terminales solo pueden solicitar números de forma controlada a través de `obtenerInstancia().generarNumero(...)`.

## Evidencia de Diseño (UML)
<img width="562" height="174" alt="07 drawio" src="https://github.com/user-attachments/assets/6cf0deac-863a-4118-8819-eb4a3466101a" />

## Fragmento de Código Clave
```java
public class GeneradorTurnos {
    private static GeneradorTurnos instancia;
    private int contador = 0;
    private String[] historial = new String[10];

    private GeneradorTurnos() {}

    public static GeneradorTurnos obtenerInstancia() {
        if (instancia == null) {
            instancia = new GeneradorTurnos();
        }
        return instancia;
    }

    public int generarNumero(String terminal) {
        if (contador == historial.length) {
            String[] nuevo = new String[historial.length * 2];
            for (int i = 0; i < contador; i++) nuevo[i] = historial[i];
            historial = nuevo;
        }
        historial[contador] = "#" + (contador + 1) + " - " + terminal;
        return ++contador;
    }
}
```

## Evidencia de Ejecución
<img width="364" height="592" alt="{AA490ABE-ABC0-4BED-9070-48293B3A7D48}" src="https://github.com/user-attachments/assets/96112206-8e94-4cc4-ad03-d05193929319" />

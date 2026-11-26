# Ejercicio 1: Notificador de Clic en Botón de Interfaz

## Explicación del Proyecto y Lógica
**Descripción:** Componente gráfico interactivo (`Boton`) que notifica automáticamente a oyentes registrados (`Ventana` y `ContadorDeClics`) cuando se simula un evento de clic enviando la pista asociada, permitiendo suscribir y desuscribir observadores.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `ObservadorClic` y el sujeto observable `Boton`. `Boton` conserva una colección genérica `List<ObservadorClic>` sin acoplarse a implementaciones concretas de ventanas o contadores y propaga cada pulsación a través de `hacerClic(String pista)` invocando `alHacerClic(pista)` en todos los observadores suscritos.

## Evidencia de Diseño (UML)
<img width="832" height="280" alt="01 drawio" src="https://github.com/user-attachments/assets/8a0d4165-965c-48e6-aa57-b63fa2125ed6" />

## Fragmento de Código Clave
```java
public interface ObservadorClic {
    void alHacerClic(String pista);
}

public void hacerClic(String pista) {
    for (ObservadorClic observador : observadores) {
        observador.alHacerClic(pista);
    }
}
```

## Evidencia de Ejecución
<img width="576" height="313" alt="{261D5A53-439B-40CD-912B-15CF3756FA95}" src="https://github.com/user-attachments/assets/9e832203-2ead-47ec-8ffb-8c7139e9a4e2" />

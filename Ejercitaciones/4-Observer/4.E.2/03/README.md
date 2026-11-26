# Ejercicio 3: Marcador de Puntuación para Videojuego Sencillo

## Explicación del Proyecto y Lógica
**Descripción:** Videojuego arcade en consola de texto (`MarcadorJuego`) que propaga de forma simultánea la puntuación acumulada y el motivo del cambio a la pantalla principal de juego (`PantallaJuego`) y al registro histórico de récords (`RegistroRecords`), permitiendo suscribir y desuscribir oyentes dinámicamente.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `ObservadorPuntuacion` y el sujeto observable `MarcadorJuego`. `MarcadorJuego` mantiene la lista de suscriptores (`List<ObservadorPuntuacion>`) y notifica pasivamente mediante `actualizar(puntuacion, motivo)` al invocarse `sumarPuntos()`, desacoplando la acumulación de puntaje de la visualización y del control de récords.

## Evidencia de Diseño (UML)
<img width="842" height="286" alt="03 drawio" src="https://github.com/user-attachments/assets/6a7331e7-08ec-4161-8d9c-570ae1d3e96e" />

## Fragmento de Código Clave
```java
public interface ObservadorPuntuacion {
    void actualizar(int puntuacion, String motivo);
}

public void sumarPuntos(int puntos, String motivo) {
    if (puntos <= 0) return;
    puntuacion += puntos;
    notificar(motivo);
}

private void notificar(String motivo) {
    for (ObservadorPuntuacion o : new ArrayList<>(observadores)) {
        o.actualizar(puntuacion, motivo);
    }
}
```

## Evidencia de Ejecución
<img width="601" height="295" alt="imagen" src="https://github.com/user-attachments/assets/6626cf68-6169-4f21-b002-a09561b9f475" />

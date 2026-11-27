# Ejercicio 5: Automatización de Persianas según Sensor de Luz Solar

## Explicación del Proyecto y Lógica
**Descripción:** Sistema domótico (`SensorLuzSolar`) que mide la luminosidad exterior en distintas zonas de la vivienda (`Living`, `Estudio`) y coordina de forma automática la posición de las persianas motorizadas (`PersianaMotorizada`) y la intensidad de las luces interiores (`LucesInteriores`).

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `ActuadorDomotico` y el sujeto observable `SensorLuzSolar`. Cada sensor administra su propia lista de actuadores (`List<ActuadorDomotico>`) y notifica pasivamente mediante `ajustar(zona, nivelLux)` al invocarse `reportarLuminosidad()`, permitiendo múltiples sensores y actuadores desacoplados sin modificar el sensor al incorporar nuevos dispositivos.

## Evidencia de Diseño (UML)
<img width="882" height="264" alt="05 drawio" src="https://github.com/user-attachments/assets/31382e19-81e0-4eb2-9e8c-028a10287568" />

## Fragmento de Código Clave
```java
public interface ActuadorDomotico {
    void ajustar(String zona, int nivelLux);
}

public void reportarLuminosidad(int nivelLux) {
    if (nivelLux < 0) return;
    System.out.println("Sensor Luz [" + zona + "] - luminosidad exterior: " + nivelLux + " lux");
    for (ActuadorDomotico a : new ArrayList<>(actuadores)) {
        a.ajustar(zona, nivelLux);
    }
}
```

## Evidencia de Ejecución
<img width="633" height="335" alt="imagen" src="https://github.com/user-attachments/assets/836c7d01-a977-4427-9cec-25a4db20c4f8" />

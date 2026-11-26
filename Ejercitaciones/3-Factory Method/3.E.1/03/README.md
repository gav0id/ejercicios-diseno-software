# Ejercicio 3: Sistema de Notificaciones Multiplataforma Básico

## Explicación del Proyecto y Lógica
**Descripción:** Plataforma de servicios que envía alertas a sus usuarios a través de distintos canales de comunicación (Correo Electrónico y SMS) mediante una interfaz unificada de envío, permitiendo incorporar nuevos canales sin modificar la lógica de notificación.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** definiendo el contrato común `Notificacion` y la clase creadora abstracta `CreadorNotificacion`, que centraliza el envío (`notificar(String mensaje)`) y declara el método fábrica abstracto `crearNotificacion()`. De esta manera, la instanciación de cada canal concreto queda delegada en sus factorías especializadas (`CreadorEmail` y `CreadorSMS`), asegurando que el código consumidor dependa únicamente de las abstracciones.

## Evidencia de Diseño (UML)
<img width="482" height="314" alt="03 drawio" src="https://github.com/user-attachments/assets/43deb7e0-a026-4e8a-83d1-00dcbbd4782e" />

## Fragmento de Código Clave
```java
public interface Notificacion {
    void enviar(String mensaje);
}

public abstract class CreadorNotificacion {
    public abstract Notificacion crearNotificacion();

    public void notificar(String mensaje) {
        Notificacion canal = crearNotificacion();
        canal.enviar(mensaje);
    }
}

public class CreadorEmail extends CreadorNotificacion {
    @Override
    public Notificacion crearNotificacion() {
        return new NotificacionEmail();
    }
}
```

## Evidencia de Ejecución
<img width="440" height="208" alt="{B6ADE3C5-1340-4EDA-975A-053412361B81}" src="https://github.com/user-attachments/assets/fe8d0e3b-3853-400c-a65e-7fb4d8d1c9fd" />

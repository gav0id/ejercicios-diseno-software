# Ejercicio 1: Notificador de Cambio de Temperatura

## Explicación del Proyecto y Lógica
**Descripción:** Sistema para una estación meteorológica digital que actualiza simultáneamente múltiples pantallas y paneles informativos (`PantallaLobby` y `PanelLaboratorio`) cada vez que cambia la temperatura ambiente, permitiendo registrar y desvincular observadores dinámicamente.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `ObservadorTemperatura` y el sujeto observable `EstacionMeteorologica`. `EstacionMeteorologica` mantiene una lista genérica `List<ObservadorTemperatura>` sin acoplarse a los detalles visuales de cada pantalla, y notifica automáticamente a todos los suscriptores (`o.actualizar(temperatura)`) cada vez que se actualiza la temperatura mediante `setTemperatura(double nuevaTemperatura)`.

## Evidencia de Diseño (UML)
<img width="752" height="256" alt="01 drawio" src="https://github.com/user-attachments/assets/d4ce2f1c-4adf-4583-81b5-89944c87f1ff" />

## Fragmento de Código Clave
```java
public interface ObservadorTemperatura {
    void actualizar(double temperatura);
}

public class EstacionMeteorologica {
    private final List<ObservadorTemperatura> observadores = new ArrayList<>();
    private double temperatura;

    public void registrar(ObservadorTemperatura o) {
        if (o != null && !observadores.contains(o)) {
            observadores.add(o);
        }
    }

    public void desvincular(ObservadorTemperatura o) {
        observadores.remove(o);
    }

    public void setTemperatura(double nuevaTemperatura) {
        this.temperatura = nuevaTemperatura;
        notificar();
    }

    private void notificar() {
        for (ObservadorTemperatura o : observadores) {
            o.actualizar(temperatura);
        }
    }
}
```

## Evidencia de Ejecución
<img width="525" height="238" alt="{A42DD345-2EEA-4948-B16C-6785D55CC497}" src="https://github.com/user-attachments/assets/27904912-4d6e-4c19-a6d7-65b554c2d5cb" />

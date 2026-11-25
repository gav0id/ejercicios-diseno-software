# Ejercicio 6: Configuración General de un Hospital

## Explicación del Proyecto y Lógica
**Descripción:** Aplicación administrativa para un hospital compuesta por los módulos de recepción (`ModuloRecepcion`) y consultorios (`ModuloConsultorios`), los cuales consultan y modifican de forma sincronizada los parámetros generales de la institución (nombre, horario de apertura/cierre y duración predeterminada de los turnos).

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** con inicialización diferida (*lazy initialization*) en `ConfiguracionHospital` para centralizar la configuración global y evitar diferencias entre pantallas. El constructor privado impide instanciaciones directas desde los módulos, mientras que `obtenerInstancia()` crea el objeto únicamente al solicitarse por primera vez y devuelve siempre la misma referencia, comprobado mediante la comparación de identidad y el contador interno de creaciones.

## Evidencia de Diseño (UML)
<img width="602" height="324" alt="06 drawio" src="https://github.com/user-attachments/assets/cb3539c8-eea9-467c-8e55-985a8c760ccc" />

## Fragmento de Código Clave
```java
public class ConfiguracionHospital {
    private static ConfiguracionHospital instancia;
    private static int creaciones = 0;

    private String nombre = "Hospital Central";
    private LocalTime apertura = LocalTime.of(8, 0);
    private LocalTime cierre = LocalTime.of(20, 0);
    private int duracionTurno = 30;

    private ConfiguracionHospital() {
        creaciones++;
    }

    public static ConfiguracionHospital obtenerInstancia() {
        if (instancia == null) {
            instancia = new ConfiguracionHospital();
        }
        return instancia;
    }
}
```

## Evidencia de Ejecución
<img width="626" height="352" alt="{E6CFC3FE-8195-46A0-AC01-581B1407AF6E}" src="https://github.com/user-attachments/assets/8225e87e-ff38-4a65-a075-cb550d3fee1a" />

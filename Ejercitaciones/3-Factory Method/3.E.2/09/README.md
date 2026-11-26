# Ejercicio 9: Fábrica Avanzada de Sensores Ambientales para IoT

## Explicación del Proyecto y Lógica
**Descripción:** Plataforma industrial de telemetría que inicializa y lee sensores ambientales sobre protocolos de bajo nivel (`ModBus`, `I2C`, `SPI` y registro dinámico de `UART`), validando parámetros de hardware y direcciones de memoria, y registrando métricas operativas de éxitos y fallos por protocolo.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz `SensorAmbiental` y la clase creadora abstracta `CreadorSensor`, que declara el método fábrica `crearSensor(String direccionMemoria)`. Las subclases `CreadorModBus`, `CreadorI2C` y `CreadorSPI` encapsulan la validación de hardware y direcciones de memoria junto con la instanciación e inicialización de cada sensor, mientras `GestorSensores` centraliza el registro dinámico de fábricas (`ConcurrentHashMap`) y las métricas (`AtomicInteger`) sin modificar el código base.

## Evidencia de Diseño (UML)
<img width="1302" height="597" alt="09 drawio" src="https://github.com/user-attachments/assets/7da89528-147e-47bf-9367-9319a32a896b" />

## Fragmento de Código Clave
```java
public interface SensorAmbiental {
    void inicializar(String direccionMemoria);
    String leerDatos();
}

public abstract class CreadorSensor {
    public abstract SensorAmbiental crearSensor(String direccionMemoria) throws Exception;
}

public class CreadorModBus extends CreadorSensor {
    private final String puerto;

    public CreadorModBus(String puerto) {
        this.puerto = puerto;
    }

    @Override
    public SensorAmbiental crearSensor(String direccionMemoria) {
        if (puerto == null || !puerto.startsWith("COM")) {
            throw new IllegalStateException("ModBus - puerto serial invalido: " + puerto);
        }
        if (direccionMemoria == null || !direccionMemoria.matches("0x[0-9A-Fa-f]{4}")) {
            throw new IllegalArgumentException("ModBus - direccion de registro invalida: " + direccionMemoria);
        }
        SensorModBus s = new SensorModBus(puerto);
        s.inicializar(direccionMemoria);
        return s;
    }
}
```

## Evidencia de Ejecución
<img width="691" height="444" alt="{B4FF94AC-4E73-451B-AC6B-1D19FFCB934A}" src="https://github.com/user-attachments/assets/aa44bcd7-0110-437f-b8a1-6f9576ec0882" />

# Ejercicio 5: Administrador de Dispositivos de Hardware Únicos

## Explicación del Proyecto y Lógica
**Descripción:** Simulación de un sistema de automatización industrial donde distintos módulos (`ModuloBrazo` y `ModuloSensores`) interactúan con un único puerto serial físico (`COM3`) conectado a un dispositivo robótico, validando su estado operacional (`LIBRE`, `OCUPADO`, `FALLA`) antes de enviar comandos.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** en la clase `final ControladorHardware` para garantizar un único punto de acceso sincronizado al puerto físico. Al combinar un constructor privado, el método estático sincronizado `obtenerInstancia()` y el bloqueo explícito de `clone()`, se impide cualquier duplicación de la instancia en memoria y se evita que dos módulos envíen instrucciones simultáneas que puedan provocar fallas críticas de hardware.

## Evidencia de Diseño (UML)
<img width="642" height="452" alt="{90C59FAD-9D6C-4643-B927-CF7696703D38}" src="https://github.com/user-attachments/assets/0e9493ae-c044-48b3-9088-78a5ef0121c9" />

## Fragmento de Código Clave
```java
public final class ControladorHardware {
    private static ControladorHardware instancia;
    private final String puerto;
    private EstadoDispositivo estado;

    private ControladorHardware() {
        puerto = "COM3";
        estado = EstadoDispositivo.LIBRE;
        historial = new String[5];
        cantidadHistorial = 0;
    }

    public static synchronized ControladorHardware obtenerInstancia() {
        if (instancia == null) {
            instancia = new ControladorHardware();
        }
        return instancia;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        throw new CloneNotSupportedException("El controlador no se puede duplicar");
    }
}
```

## Evidencia de Ejecución
<img width="799" height="595" alt="{BDA596E6-EC42-4B88-8ED4-626DC40511E7}" src="https://github.com/user-attachments/assets/cc7ce124-aa0e-4c67-9e07-326d77180636" />

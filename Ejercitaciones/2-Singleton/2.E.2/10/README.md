# Ejercicio 10: Administrador Concurrente de Turnos de Consultorio

## Explicación del Proyecto y Lógica
**Descripción:** Sistema concurrente de gestión de turnos hospitalarios compartido entre recepción y un puesto de autoservicio (`PuestoTurnos`). Permite cargar horarios disponibles por profesional y fecha, reservar turnos de forma atómica evitando asignaciones duplicadas ante 20 solicitudes simultáneas, cancelar turnos liberando el horario y consultar la agenda actualizada desde ambos puestos.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** en `AdministradorTurnos` para centralizar la agenda en una única instancia compartida por ambos puestos de atención. La creación única se protege con *Double-Checked Locking* y `volatile` en `obtenerInstancia()`, mientras que la verificación de disponibilidad y la asignación del turno se ejecutan de forma atómica bajo un objeto `candado` privado, complementado con objetos `Turno` inmutables y copias defensivas en las consultas.

## Evidencia de Diseño (UML)
<img width="749" height="564" alt="10 drawio" src="https://github.com/user-attachments/assets/9056b043-0f72-4952-8fc5-1d76985c3ad1" />

## Fragmento de Código Clave
```java
public final class AdministradorTurnos {
    private static volatile AdministradorTurnos instancia;
    private final Object candado = new Object();

    private AdministradorTurnos() {}

    public static AdministradorTurnos obtenerInstancia() {
        if (instancia == null) {
            synchronized (AdministradorTurnos.class) {
                if (instancia == null) {
                    instancia = new AdministradorTurnos();
                }
            }
        }
        return instancia;
    }

    public boolean reservar(String paciente, String profesional, LocalDate fecha, LocalTime horario) {
        if (paciente == null || paciente.isBlank()) return false;
        synchronized (candado) {
            int pos = buscar(profesional, fecha, horario);
            if (pos == -1 || !agenda[pos].estaLibre()) return false;
            agenda[pos] = agenda[pos].conPaciente(paciente.trim());
            return true;
        }
    }
}
```

## Evidencia de Ejecución
<img width="579" height="390" alt="imagen" src="https://github.com/user-attachments/assets/3b143163-0a07-4b42-99aa-0790db1b105c" />

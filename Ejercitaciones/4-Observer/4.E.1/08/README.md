# Ejercicio 8: Refactorización de Código Acoplado hacia el Patrón Observer

## Explicación del Proyecto y Lógica
**Descripción:** Refactorización de una aplicación de gestión de proyectos donde `Tarea` deja de invocar directamente a los servicios de correo y estadísticas, pasando a actuar como sujeto observable que notifica sus cambios de estado (`PENDIENTE`, `EN_PROGRESO`, `COMPLETADA`, `ARCHIVADA`) a módulos independientes (`NotificadorCorreo` y `ModuloEstadisticas`).

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `ObservadorTarea` y el sujeto observable `Tarea`. Al eliminar las dependencias directas e introducir inversión de control sobre `List<ObservadorTarea>`, `Tarea` conserva únicamente la responsabilidad de gestionar su propio estado y propagar los cambios mediante `notificar(anterior, actual)`, evitando que modificaciones en los receptores afecten la lógica de negocio principal.

## Evidencia de Diseño (UML)
<img width="882" height="314" alt="08 drawio" src="https://github.com/user-attachments/assets/c27c80eb-b65f-49d1-a1ce-0f2761a81697" />

## Fragmento de Código Clave
```java
public interface ObservadorTarea {
    void actualizar(String titulo, String estadoAnterior, String estadoNuevo);
}

public void cambiarEstado(String nuevoEstado) {
    if (nuevoEstado == null || nuevoEstado.isBlank() || nuevoEstado.equalsIgnoreCase(estado)) return;
    String anterior = this.estado;
    this.estado = nuevoEstado.toUpperCase();
    notificar(anterior, this.estado);
}

private void notificar(String anterior, String actual) {
    for (ObservadorTarea o : new ArrayList<>(observadores)) {
        o.actualizar(titulo, anterior, actual);
    }
}
```

## Evidencia de Ejecución
<img width="740" height="271" alt="{0D0D928D-D584-45F6-9694-A241F8D1910F}" src="https://github.com/user-attachments/assets/d30a8ee3-e65f-4d48-ab5d-5a0ec3cc750a" />

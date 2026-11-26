# Ejercicio 3: Registro Automático de Auditoría por Consola

## Explicación del Proyecto y Lógica
**Descripción:** Sistema de gestión empresarial que informa automáticamente las operaciones críticas sobre empleados (`alta`, `modificacion` y `baja`) a un módulo de auditoría en consola (`AuditorConsola`) y a una bitácora en archivo (`AuditorArchivo`), permitiendo suscribir y desuscribir oyentes dinámicamente en tiempo de ejecución.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `Auditor` y el sujeto observable `GestorEmpleados`. La lógica de negocio (`alta`, `modificar` y `baja`) no realiza llamadas directas a consola ni a archivos, sino que invoca a `notificar(String evento)` para propagar cada evento a todos los `Auditor` registrados en la lista `auditores`, manteniendo un desacoplamiento total entre el gestor y los módulos de auditoría.

## Evidencia de Diseño (UML)
<img width="882" height="167" alt="03 drawio" src="https://github.com/user-attachments/assets/757ccac4-fb96-40a7-aed0-c422553924cc" />

## Fragmento de Código Clave
```java
public interface Auditor {
    void registrar(String evento);
}

public void modificar(String empleado, String cambio) {
    notificar("MODIFICACION - empleado: " + empleado + " - " + cambio);
}

private void notificar(String evento) {
    for (Auditor a : new ArrayList<>(auditores)) {
        a.registrar(evento);
    }
}
```

## Evidencia de Ejecución
<img width="860" height="267" alt="imagen" src="https://github.com/user-attachments/assets/3b8deab5-45bd-4858-bcbf-a00680ac174e" />

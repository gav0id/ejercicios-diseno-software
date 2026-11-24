# Ejercicio 1: Configuración General de una Biblioteca

## Explicación del Proyecto y Lógica
**Descripción:** Aplicación de gestión para una biblioteca donde los módulos de préstamos (`ModuloPrestamos`) y consultas (`ModuloConsultas`) comparten y modifican una misma configuración institucional (nombre, máximo de libros por socio y plazo de devolución en días), manteniendo un historial unificado de cambios.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** en `ConfiguracionBiblioteca` para centralizar los parámetros globales de la institución y evitar que cada módulo cree su propia copia desactualizada. Al restringir la instanciación con un constructor privado, exponer el acceso único mediante `obtenerInstancia()` y validar cada modificación en los setters, se garantiza que ambos módulos operen siempre sobre exactamente el mismo objeto en memoria.

## Evidencia de Diseño (UML)
<img width="562" height="504" alt="01 drawio" src="https://github.com/user-attachments/assets/0f3bf1cc-8bd5-4f6e-826a-f4e158f1ae52" />

## Fragmento de Código Clave
```java
public class ConfiguracionBiblioteca {
    private static ConfiguracionBiblioteca instancia;
    private String nombre;
    private int maxLibrosPorSocio;
    private int plazoDevolucionDias;

    private ConfiguracionBiblioteca() {
        nombre = "Biblioteca Central";
        maxLibrosPorSocio = 3;
        plazoDevolucionDias = 14;
        historial = new String[5];
        cantidadHistorial = 0;
    }

    public static ConfiguracionBiblioteca obtenerInstancia() {
        if (instancia == null) {
            instancia = new ConfiguracionBiblioteca();
        }
        return instancia;
    }
}
```

## Evidencia de Ejecución
<img width="716" height="358" alt="{260F7362-A96D-4488-9A8C-CA1919A64C44}" src="https://github.com/user-attachments/assets/7cacdbc5-e394-498f-8a56-5b12970fad5f" />

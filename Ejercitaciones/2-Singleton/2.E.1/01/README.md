# Ejercicio 1: Configuración Inicial de la Aplicación

## Explicación del Proyecto y Lógica
**Descripción:** Simulación de una aplicación modular donde distintos componentes (`ModuloAutenticacion` y `ModuloReportes`) necesitan consultar parámetros globales de configuración (como idioma o zona horaria) sin recargar el archivo maestro en cada consulta.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** en la clase `ConfiguracionApp` para garantizar que los parámetros clave-valor se carguen en memoria una única vez durante todo el ciclo de vida del sistema. Al restringir la instanciación mediante un constructor privado y centralizar el acceso a través del método estático `obtenerInstancia()`, se evita el desperdicio de recursos de E/S y se asegura que todos los módulos de la aplicación lean exactamente la misma configuración consistente.

## Evidencia de Diseño (UML)
![Diagrama de Clases UML](diagrama_uml.png)

## Fragmento de Código Clave
```java
public class ConfiguracionApp {
    private static ConfiguracionApp instancia;
    private String[] claves;
    private String[] valores;
    private int cantidad;

    private ConfiguracionApp() {
        claves = new String[10];
        valores = new String[10];
        cantidad = 0;
        cargarParametros();
    }

    public static ConfiguracionApp obtenerInstancia() {
        if (instancia == null) {
            instancia = new ConfiguracionApp();
        }
        return instancia;
    }
}
```

## Evidencia de Ejecución
<img width="1300" height="245" alt="imagen" src="https://github.com/user-attachments/assets/a13803f0-5c8e-4cef-aec0-62c0da24f7e7" />


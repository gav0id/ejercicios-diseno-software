# Ejercicio 2: Gestor de Conexiones a Base de Datos Simple

## Explicación del Proyecto y Lógica
**Descripción:** Simulación de un sistema modular donde distintos componentes (`ModuloClientes` y `ModuloPedidos`) necesitan ejecutar consultas y actualizaciones sobre una base de datos local, compartiendo una única conexión física y un historial unificado de operaciones.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se aplicó el patrón **Singleton** en la clase `GestorConexion` para encapsular un recurso costoso (la conexión a la base de datos) y centralizar su ciclo de vida. Al restringir la instanciación mediante un constructor privado y exponer el acceso único con `obtenerInstancia()`, se evita abrir múltiples conexiones redundantes y se garantiza que el estado de la conexión (`conectada`) esté siempre sincronizado entre todos los módulos del sistema.

## Evidencia de Diseño (UML)
<img width="515" height="384" alt="{4584FC59-8ED0-419A-AA59-B5B3161A2B77}" src="https://github.com/user-attachments/assets/e17bc9ad-242b-448f-9b98-33f736e4e7fe" />


## Fragmento de Código Clave
```java
public class GestorConexion {
    private static GestorConexion instancia;
    private boolean conectada;

    private GestorConexion() {
        conectada = false;
        historial = new String[5];
        cantidadHistorial = 0;
    }

    public static GestorConexion obtenerInstancia() {
        if (instancia == null) {
            instancia = new GestorConexion();
        }
        return instancia;
    }
}
```

## Evidencia de Ejecución
<img width="614" height="546" alt="imagen" src="https://github.com/user-attachments/assets/db7f35b6-c040-4c93-a485-6956e2f2d4d8" />

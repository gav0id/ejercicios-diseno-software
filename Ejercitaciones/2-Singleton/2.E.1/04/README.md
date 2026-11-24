# Ejercicio 4: Servicio de Caché en Memoria

## Explicación del Proyecto y Lógica
**Descripción:** Simulación de una plataforma web donde distintos módulos (`ModuloPerfiles` y `ModuloSesiones`) comparten un repositorio de caché en memoria para almacenar, actualizar, consultar y eliminar perfiles de usuario por clave única, evitando consultas repetitivas al servidor principal.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** con inicialización diferida (*lazy initialization*) en la clase `ServicioCache` para garantizar que exista un único repositorio de caché compartido en toda la aplicación. Al encapsular la estructura de datos interna como privada, restringir el constructor y sincronizar tanto `obtenerInstancia()` como las operaciones de lectura y escritura, se optimiza el uso de memoria y se asegura que todos los módulos trabajen siempre sobre el mismo estado actualizado.

## Evidencia de Diseño (UML)


## Fragmento de Código Clave
```java
public class ServicioCache {
    private static ServicioCache instancia;
    private String[] claves;
    private String[] valores;
    private int cantidad;

    private ServicioCache() {
        claves = new String[3];
        valores = new String[3];
        cantidad = 0;
    }

    public static synchronized ServicioCache obtenerInstancia() {
        if (instancia == null) {
            instancia = new ServicioCache();
        }
        return instancia;
    }
}
```

## Evidencia de Ejecución
<img width="731" height="530" alt="{83560420-29EA-4AC5-B940-113DD93B7838}" src="https://github.com/user-attachments/assets/b4891aab-22a5-440f-9037-7005035e4391" />

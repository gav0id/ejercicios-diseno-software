# Ejercicio 2: Creador de Conexiones a Motores de Base de Datos

## Explicación del Proyecto y Lógica
**Descripción:** Software de administración de bases de datos capaz de conectarse a distintos motores (`SQLite` y `PostgreSQL`) según la configuración del usuario, ofreciendo un punto de creación unificado para ejecutar consultas de prueba.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz común `ConexionBD` y la clase creadora abstracta `CreadorConexion`, que centraliza el flujo de prueba (`probarConexion()`) y declara el método fábrica abstracto `crearConexion()`. Cada creador concreto (`CreadorSQLite` y `CreadorPostgreSQL`) encapsula la instanciación y los parámetros de configuración de su motor, manteniendo separada en el cliente la selección del creador respecto de la lógica de negocio que opera sobre la conexión.

## Evidencia de Diseño (UML)
<img width="502" height="332" alt="02 drawio" src="https://github.com/user-attachments/assets/506a45b4-3b26-44f4-9fa4-6384f072163a" />

## Fragmento de Código Clave
```java
public interface ConexionBD {
    void ejecutarConsultaPrueba(String sql);
}

public abstract class CreadorConexion {
    public abstract ConexionBD crearConexion();

    public void probarConexion(String sql) {
        ConexionBD conexion = crearConexion();
        conexion.ejecutarConsultaPrueba(sql);
    }
}

public class CreadorSQLite extends CreadorConexion {
    @Override
    public ConexionBD crearConexion() {
        return new ConexionSQLite("mangas.db");
    }
}
```

## Evidencia de Ejecución
<img width="700" height="209" alt="{098C5CD6-AD59-466D-9040-59C2A012F6E9}" src="https://github.com/user-attachments/assets/de77a8f2-1dbc-40e8-897e-97b4d74fdf0d" />

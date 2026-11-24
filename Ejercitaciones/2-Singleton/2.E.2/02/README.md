# Ejercicio 2: Registro Centralizado de Actividades de la Biblioteca

## Explicación del Proyecto y Lógica
**Descripción:** Sistema de registro compartido para una biblioteca donde los sectores de préstamos (`SectorPrestamos`), devoluciones (`SectorDevoluciones`) y atención a socios (`SectorAtencionSocios`) incorporan eventos con fecha, hora, sector y descripción en un único historial cronológico, permitiendo consultar el total de eventos, el listado completo y filtrar por sector.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** en `RegistroActividades` para unificar la captura de eventos provenientes de los tres sectores en una única instancia con estado propio, evitando registros fragmentados o soluciones basadas únicamente en miembros estáticos. La colección interna privada se protege mediante copias defensivas en las consultas y objetos inmutables (`Actividad`), asegurando que el historial no pueda alterarse desde afuera.

## Evidencia de Diseño (UML)

## Fragmento de Código Clave
```java
public class RegistroActividades {
    private static RegistroActividades instancia;
    private Actividad[] actividades;
    private int cantidad;

    private RegistroActividades() {
        actividades = new Actividad[3];
        cantidad = 0;
    }

    public static RegistroActividades obtenerInstancia() {
        if (instancia == null) {
            instancia = new RegistroActividades();
        }
        return instancia;
    }

    public Actividad[] obtenerHistorial() {
        Actividad[] copia = new Actividad[cantidad];
        for (int i = 0; i < cantidad; i++) {
            copia[i] = actividades[i];
        }
        return copia;
    }
}
```

## Evidencia de Ejecución
<img width="656" height="378" alt="{A29DC011-9C5D-4B5A-A70F-FEB8ADE442FA}" src="https://github.com/user-attachments/assets/186e81d3-ca59-44e5-9820-e45536450565" />

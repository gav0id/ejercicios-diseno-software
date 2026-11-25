# Ejercicio 4: Administrador Único de Préstamos

## Explicación del Proyecto y Lógica
**Descripción:** Sistema de gestión de préstamos para una biblioteca con dos mostradores (`MostradorPrincipal` y `MostradorSecundario`) que operan sobre un mismo estado centralizado. Permite registrar préstamos por identificador único de ejemplar físico, impedir préstamos duplicados de ejemplares ocupados, procesar devoluciones validando que exista un préstamo activo y consultar los préstamos vigentes de un socio.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** en `AdministradorPrestamos` para centralizar el estado de los préstamos activos en una única instancia compartida entre todos los mostradores, evitando que dos puntos de atención presten el mismo ejemplar físico a socios distintos. Al encapsular la colección de objetos inmutables `Prestamo` y concentrar las validaciones dentro del administrador antes de modificar el estado, se garantiza consistencia en toda la aplicación.

## Evidencia de Diseño (UML)
<img width="802" height="464" alt="04 drawio" src="https://github.com/user-attachments/assets/59c760e0-0ef5-46c1-80b6-e1e1665cfea1" />


## Fragmento de Código Clave
```java
public class AdministradorPrestamos {
    private static AdministradorPrestamos instancia;
    private Prestamo[] activos;
    private int cantidad;

    private AdministradorPrestamos() {
        activos = new Prestamo[2];
        cantidad = 0;
    }

    public static AdministradorPrestamos obtenerInstancia() {
        if (instancia == null) {
            instancia = new AdministradorPrestamos();
        }
        return instancia;
    }

    public String prestar(String idEjemplar, String socio, LocalDate fecha) {
        if (estaVacio(idEjemplar)) return "RECHAZADO: identificador de ejemplar vacio";
        if (estaVacio(socio)) return "RECHAZADO: socio vacio";
        if (fecha == null) return "RECHAZADO: fecha nula";

        String id = idEjemplar.trim();
        int posicion = buscarPosicion(id);
        if (posicion != -1) {
            return "RECHAZADO: el ejemplar " + id + " ya esta prestado a " + activos[posicion].getSocio();
        }
        if (cantidad == activos.length) {
            agrandar();
        }
        activos[cantidad] = new Prestamo(id, socio.trim(), fecha);
        cantidad++;
        return "PRESTADO: " + id + " a " + socio.trim();
    }
}
```

## Evidencia de Ejecución
<img width="655" height="402" alt="{0909A02C-3F20-40FA-A11D-5615A180394C}" src="https://github.com/user-attachments/assets/e70ad795-205c-4396-9ecd-8b180d32bf29" />


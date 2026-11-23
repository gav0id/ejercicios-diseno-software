# Ejercicio 3: Registro de Eventos (Logger) Básico

## Explicación del Proyecto y Lógica
**Descripción:** Sistema de registro centralizado de eventos donde distintos módulos (`ModuloUsuarios` y `ModuloPagos`) reportan operaciones, advertencias y errores con marca de tiempo automática y niveles de severidad (`INFO`, `WARNING`, `ERROR`), almacenando un historial unificado.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** en la clase `Logger` para canalizar toda la escritura de eventos a través de un único punto de control. Al restringir la instanciación mediante un constructor privado, exponer el acceso global con `obtenerInstancia()` (y métodos estáticos de conveniencia) y sincronizar el método `registrar()`, se evitan bloqueos de concurrencia y corrupción de datos al escribir sobre el mismo recurso compartido.

## Evidencia de Diseño (UML)
<img width="642" height="383" alt="{13E1E6EE-4C2C-42CE-9523-104BCBD136A7}" src="https://github.com/user-attachments/assets/03cb7f63-bf8e-47bf-a0be-1b9647524445" />

## Fragmento de Código Clave
```java
public class Logger {
    private static Logger instancia;
    private static final DateTimeFormatter FORMATO =
            DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");

    private Logger() {
        historial = new String[10];
        cantidadHistorial = 0;
    }

    public static Logger obtenerInstancia() {
        if (instancia == null) {
            instancia = new Logger();
        }
        return instancia;
    }

    public synchronized void registrar(NivelLog nivel, String mensaje) {
        String marca = LocalDateTime.now().format(FORMATO);
        String linea = "[" + marca + "] [" + nivel + "] " + mensaje;
        System.out.println(linea);
        guardarEnHistorial(linea);
    }
}
```

## Evidencia de Ejecución
<img width="691" height="610" alt="{99FE0469-26C3-4B27-B251-B6364D34F5EB}" src="https://github.com/user-attachments/assets/027d00ff-c8c3-45fd-936b-838a3aebcad7" />

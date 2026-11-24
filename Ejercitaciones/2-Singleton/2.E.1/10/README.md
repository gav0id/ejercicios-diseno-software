# Ejercicio 10: Contenedor de Estado Global y Auditoría de Seguridad

## Explicación del Proyecto y Lógica
**Descripción:** Sistema centralizado de auditoría de seguridad donde distintos módulos críticos (`ModuloTransferencias` y `ModuloPrivilegios`) registran acciones de operadores con número de secuencia, marca temporal, identificador de sesión y descripción, garantizando una traza cronológica e inmutable.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó un **Singleton** de grado empresarial en `final AuditorSeguridad` para garantizar un único punto de registro inalterable en toda la aplicación. Combina *Double-Checked Locking* (`volatile`) para la inicialización segura en entornos multihilo, un `candado` privado para sincronizar el acceso al estado interno, protección contra clonación (`clone()`) y contra instanciación por *Reflection* en el constructor privado. Además, la traza se almacena mediante objetos inmutables (`RegistroAuditoria`) y se expone únicamente a través de copias defensivas.

## Evidencia de Diseño (UML)
<img width="509" height="264" alt="{68D7C619-4BD8-44AC-B255-6D81D2AA3305}" src="https://github.com/user-attachments/assets/b1a03ac1-4d67-4602-86ec-4fbbf74c7588" />

## Fragmento de Código Clave
```java
public final class AuditorSeguridad {
    private static volatile AuditorSeguridad instancia;
    private final Object candado = new Object();

    private AuditorSeguridad() {
        if (instancia != null) {
            throw new IllegalStateException("Ya existe un auditor");
        }
        registros = new RegistroAuditoria[2];
        cantidad = 0;
        ultimaSecuencia = 0;
    }

    public static AuditorSeguridad obtenerInstancia() {
        if (instancia == null) {
            synchronized (AuditorSeguridad.class) {
                if (instancia == null) {
                    instancia = new AuditorSeguridad();
                }
            }
        }
        return instancia;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        throw new CloneNotSupportedException("El auditor no se puede duplicar");
    }
}
```

## Evidencia de Ejecución
<img width="912" height="269" alt="{4D2747C2-6317-48B7-9F42-D7FE7A229759}" src="https://github.com/user-attachments/assets/61ae16b7-1067-4fa9-b094-74e45769404a" />

# Ejercicio 8: Refactorización de Código Acoplado hacia Factory Method

## Explicación del Proyecto y Lógica
**Descripción:** Refactorización de un servicio de respaldos que anteriormente utilizaba condicionales rígidos para instanciar compresores de archivos (`ZIP`, `TAR`, `GZ`). Mediante polimorfismo e inversión de control, `ServicioBackup` genera respaldos delegando la creación del compresor a una jerarquía de factorías.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** definiendo la interfaz `Compresor` y el creador abstracto `CreadorCompresor` con el método fábrica `crearCompresor()`. Las subclases (`CreadorZIP`, `CreadorTAR`, `CreadorGZ`) encapsulan la instanciación de cada formato, mientras que `ServicioBackup` recibe la factoría por constructor (inversión de control) y opera exclusivamente contra las abstracciones, cumpliendo con el Principio de Responsabilidad Única (SRP) al separar la selección del compresor de la lógica de compresión y respaldo.

## Evidencia de Diseño (UML)
<img width="1042" height="416" alt="08 drawio" src="https://github.com/user-attachments/assets/1121b90e-457e-42a6-b1d1-26e2e589ff79" />

## Fragmento de Código Clave
```java
public interface Compresor {
    String comprimir(String archivo);
}

public abstract class CreadorCompresor {
    public abstract Compresor crearCompresor();
}

public class ServicioBackup {
    private final CreadorCompresor creador;

    public ServicioBackup(CreadorCompresor creador) {
        this.creador = creador;
    }

    public void respaldar(String archivo) {
        Compresor compresor = creador.crearCompresor();
        System.out.println("Respaldo generado - " + compresor.comprimir(archivo));
    }
}
```

## Evidencia de Ejecución
<img width="494" height="228" alt="imagen" src="https://github.com/user-attachments/assets/b91a8519-e7ef-4dda-a936-f08cfddee65e" />


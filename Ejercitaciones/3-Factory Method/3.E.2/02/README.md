# Ejercicio 2: Creador de Conexiones a Repositorios de Archivos

## Explicación del Proyecto y Lógica
**Descripción:** Aplicación de escritorio que permite guardar y recuperar archivos multimedia tanto en almacenamiento local (`RepositorioLocal`) como en un servidor en la nube (`RepositorioNube`), unificando la creación del servicio de almacenamiento según la preferencia del usuario.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz `RepositorioArchivos` y la clase creadora abstracta `CreadorRepositorio`, que declara el método fábrica `crearRepositorio()` y centraliza la operación común `gestionarArchivo(String archivo)`. Las subclases `CreadorLocal` y `CreadorNube` encapsulan la instanciación y configuración de cada conector, mientras que la selección del creador queda separada de la lógica de negocio que gestiona los archivos.

## Evidencia de Diseño (UML)
<img width="1162" height="272" alt="02 drawio" src="https://github.com/user-attachments/assets/641c9e7e-d6ce-42c1-90a4-f29d996d331c" />

## Fragmento de Código Clave
```java
public interface RepositorioArchivos {
    void guardar(String archivo);
    String recuperar(String archivo);
}

public abstract class CreadorRepositorio {
    public abstract RepositorioArchivos crearRepositorio();

    public void gestionarArchivo(String archivo) {
        RepositorioArchivos repo = crearRepositorio();
        repo.guardar(archivo);
        System.out.println("Recuperado - " + repo.recuperar(archivo));
    }
}

public class CreadorLocal extends CreadorRepositorio {
    @Override
    public RepositorioArchivos crearRepositorio() {
        return new RepositorioLocal("C:/multimedia");
    }
}
```

## Evidencia de Ejecución
<img width="612" height="259" alt="{BBF7C94F-D033-4251-991F-3D212264C3C1}" src="https://github.com/user-attachments/assets/a3c91b8c-9bbb-4600-a094-c71318a55d16" />


# Ejercicio 5: Generador de Personajes para Videojuego de Rol

## Explicación del Proyecto y Lógica
**Descripción:** Sistema de creación de personajes para un videojuego de rol que permite instanciar distintas clases (`Guerrero`, `Mago` y `Arquero`), cada una con sus propias estadísticas base de vida, ataque y defensa, además de su arma inicial correspondiente asignada de forma automática.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** utilizando la clase abstracta `Personaje` como producto base (centralizando los atributos comunes y `toString()`) y `CreadorPersonaje` como creador abstracto con el método fábrica `crearPersonaje(String nombre)`. Cada creador concreto (`CreadorGuerrero`, `CreadorMago` y `CreadorArquero`) encapsula la instanciación y el equipamiento del arma inicial, mientras que `obtenerCreador(String tipo)` permite que el cliente solicite personajes indicando únicamente el tipo, eliminando bloques condicionales y el uso de `new` en `Main`.

## Evidencia de Diseño (UML)
<img width="1242" height="376" alt="05 drawio" src="https://github.com/user-attachments/assets/314c3740-4a06-46e2-933a-d9a5e557974d" />

## Fragmento de Código Clave
```java
public abstract class CreadorPersonaje {
    public abstract Personaje crearPersonaje(String nombre);

    public static CreadorPersonaje obtenerCreador(String tipo) {
        if (tipo.equalsIgnoreCase("guerrero")) return new CreadorGuerrero();
        if (tipo.equalsIgnoreCase("mago")) return new CreadorMago();
        if (tipo.equalsIgnoreCase("arquero")) return new CreadorArquero();
        throw new IllegalArgumentException("Tipo no soportado: " + tipo);
    }
}

public class CreadorGuerrero extends CreadorPersonaje {
    @Override
    public Personaje crearPersonaje(String nombre) {
        Personaje p = new Guerrero(nombre);
        p.setArma("Espada larga");
        return p;
    }
}
```

## Evidencia de Ejecución
<img width="685" height="224" alt="{955C80AD-E261-4B01-90A9-74EED95484A4}" src="https://github.com/user-attachments/assets/ec59574c-bdbf-48ef-812e-74f58d8b5161" />

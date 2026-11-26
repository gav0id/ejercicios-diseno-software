# Ejercicio 5: Generador de Enemigos para Videojuego de Acción

## Explicación del Proyecto y Lógica
**Descripción:** Módulo de generación dinámica de enemigos para un videojuego de disparos en primera persona (`SoldadoRaso` y `DronCombate`), donde cada tipo posee estadísticas propias de salud, daño y velocidad, junto con un comportamiento táctico inicial asignado automáticamente al momento de su creación.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** utilizando la clase abstracta `Enemigo` como producto base y `CreadorEnemigo` como creador abstracto. Cada subclase creadora (`CreadorSoldadoRaso` y `CreadorDronCombate`) sobrescribe el método fábrica `crearEnemigo(String nombre)` para instanciar su enemigo concreto y asignarle su táctica inicial. El cliente (`Main`) resuelve la creación indicando únicamente el tipo a `CreadorEnemigo.obtenerCreador(String tipo)`, evitando bloques condicionales y acoplamiento directo con las clases concretas.

## Evidencia de Diseño (UML)
<img width="1082" height="254" alt="05 drawio" src="https://github.com/user-attachments/assets/f7dfcfa2-721b-4bb0-af38-5065536b4854" />

## Fragmento de Código Clave
```java
public abstract class CreadorEnemigo {
    public abstract Enemigo crearEnemigo(String nombre);

    public static CreadorEnemigo obtenerCreador(String tipo) {
        if (tipo.equalsIgnoreCase("soldado")) return new CreadorSoldadoRaso();
        if (tipo.equalsIgnoreCase("dron")) return new CreadorDronCombate();
        throw new IllegalArgumentException("Tipo no soportado: " + tipo);
    }
}

public class CreadorSoldadoRaso extends CreadorEnemigo {
    @Override
    public Enemigo crearEnemigo(String nombre) {
        Enemigo e = new SoldadoRaso(nombre);
        e.setTactica("Flanqueo y cobertura");
        return e;
    }
}
```

## Evidencia de Ejecución
<img width="926" height="208" alt="{3693E1C1-4BA4-4D63-947A-60E7155247FF}" src="https://github.com/user-attachments/assets/24de1f87-fba0-4404-bcad-0e24439f5010" />

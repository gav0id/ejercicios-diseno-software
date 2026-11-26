# Ejercicio 3: Sistema de Alertas por Canal de Texto

## Explicación del Proyecto y Lógica
**Descripción:** Módulo de monitoreo de servidores que envía mensajes de alerta a los administradores a través de distintos canales de salida (`Consola` y `Archivo de Registro`), utilizando una interfaz unificada que permite emitir alertas sin acoplar el código consumidor al medio concreto.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** definiendo la interfaz `Alerta` y la clase creadora abstracta `CreadorAlerta`, que declara el método fábrica `crearAlerta()` y centraliza el envío mediante `alertar(String mensaje)`. Las factorías concretas (`CreadorConsola` y `CreadorArchivo`) encapsulan la instanciación de cada canal, asegurando que las clases consumidoras interactúen exclusivamente con las abstracciones.

## Evidencia de Diseño (UML)
<img width="1002" height="254" alt="03 drawio" src="https://github.com/user-attachments/assets/ff2fefef-ea1f-444e-988d-6cb800d7447b" />

## Fragmento de Código Clave
```java
public interface Alerta {
    void emitir(String mensaje);
}

public abstract class CreadorAlerta {
    public abstract Alerta crearAlerta();

    public void alertar(String mensaje) {
        Alerta canal = crearAlerta();
        canal.emitir(mensaje);
    }
}

public class CreadorConsola extends CreadorAlerta {
    @Override
    public Alerta crearAlerta() {
        return new AlertaConsola();
    }
}
```

## Evidencia de Ejecución
<img width="540" height="223" alt="imagen" src="https://github.com/user-attachments/assets/cb4a8226-6e1e-4b2a-bb22-09278938da20" />

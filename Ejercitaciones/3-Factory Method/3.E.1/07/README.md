# Ejercicio 7: Generador de Elementos de Interfaz Gráfica Adaptativa

## Explicación del Proyecto y Lógica
**Descripción:** Framework visual que renderiza componentes gráficos interactivos (Botones y Campos de Texto) adaptados al tema activo del entorno (Escritorio Oscuro o Móvil Minimalista), asegurando que todos los elementos mostrados en pantalla pertenezcan a una misma familia estética.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz común `ComponenteUI` y la clase creadora abstracta `CreadorUI`, que define los métodos fábrica abstractos `crearBoton()` y `crearCampoTexto()` junto con el flujo común `mostrarPantalla()`. Cada subclase creadora (`CreadorEscritorioOscuro` y `CreadorMovilMinimalista`) se especializa en instanciar únicamente los componentes compatibles con su estilo, manteniendo un contrato estricto de renderizado e interacción sin acoplar al cliente a las clases concretas.

## Evidencia de Diseño (UML)
<img width="1242" height="406" alt="07 drawio" src="https://github.com/user-attachments/assets/d4830a64-6727-4071-9fbd-e7738740cdb5" />

## Fragmento de Código Clave
```java
public interface ComponenteUI {
    void renderizar();
    void interactuar(String entrada);
}

public abstract class CreadorUI {
    public abstract ComponenteUI crearBoton();
    public abstract ComponenteUI crearCampoTexto();

    public void mostrarPantalla(String texto, String accionBoton) {
        ComponenteUI campo = crearCampoTexto();
        ComponenteUI boton = crearBoton();
        campo.renderizar();
        boton.renderizar();
        campo.interactuar(texto);
        boton.interactuar(accionBoton);
    }
}

public class CreadorEscritorioOscuro extends CreadorUI {
    @Override
    public ComponenteUI crearBoton() { return new BotonOscuro(); }
    @Override
    public ComponenteUI crearCampoTexto() { return new CampoTextoOscuro(); }
}
```

## Evidencia de Ejecución
<img width="568" height="327" alt="{F8867423-1800-4DC1-8932-227F8F1E2F3A}" src="https://github.com/user-attachments/assets/cf8f6746-36c9-4bcc-b937-e15f2fdc5044" />
<img width="568" height="327" alt="{F8867423-1800-4DC1-8932-227F8F1E2F3A}" src="https://github.com/user-attachments/assets/a830344c-cce3-4dff-9a31-c36bb94b2339" />

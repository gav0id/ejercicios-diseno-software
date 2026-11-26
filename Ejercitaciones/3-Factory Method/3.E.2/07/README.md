# Ejercicio 7: Generador de Componentes de Interfaz Gráfica Minimalista

## Explicación del Proyecto y Lógica
**Descripción:** Framework gráfico ligero que renderiza componentes de interfaz (`Botones Interactivos` y `Etiquetas de Texto`) adaptados visualmente a distintas resoluciones de pantalla (`Modo Escritorio 1920x1080` y `Modo Dispositivo Móvil 390x844`), respondiendo a eventos propios de cada entorno sin mezclar estilos incompatibles.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz `ComponenteGrafico` y la clase creadora abstracta `CreadorInterfaz`. Esta superclase centraliza el flujo de armado en `construirVista(String textoEtiqueta, String accionBoton)` y delega la instanciación de cada familia coherente en los métodos fábrica abstractos `crearBoton()` y `crearEtiqueta()`, sobrescritos por `CreadorEscritorio` y `CreadorMovil`. De este modo, el código cliente trabaja únicamente contra la abstracción garantizando coherencia estética y propiedades geométricas homogéneas por entorno.

## Evidencia de Diseño (UML)
<img width="1282" height="416" alt="07 drawio" src="https://github.com/user-attachments/assets/e192936f-1cd9-466a-893f-6d94d91ac0bc" />

## Fragmento de Código Clave
```java
public interface ComponenteGrafico {
    void renderizar();
    void interactuar(String evento);
}

public abstract class CreadorInterfaz {
    public abstract ComponenteGrafico crearBoton();
    public abstract ComponenteGrafico crearEtiqueta();

    public void construirVista(String textoEtiqueta, String accionBoton) {
        ComponenteGrafico etiqueta = crearEtiqueta();
        ComponenteGrafico boton = crearBoton();
        etiqueta.renderizar();
        boton.renderizar();
        etiqueta.interactuar(textoEtiqueta);
        boton.interactuar(accionBoton);
    }
}

public class CreadorEscritorio extends CreadorInterfaz {
    @Override
    public ComponenteGrafico crearBoton() { return new BotonEscritorio(); }
    @Override
    public ComponenteGrafico crearEtiqueta() { return new EtiquetaEscritorio(); }
}
```

## Evidencia de Ejecución
<img width="683" height="336" alt="imagen" src="https://github.com/user-attachments/assets/df56ba07-26ba-42b9-a73b-df027658de19" />

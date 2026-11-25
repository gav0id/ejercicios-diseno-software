# Ejercicio 1: Generador Básico de Documentos de Texto

## Explicación del Proyecto y Lógica
**Descripción:** Aplicación de ofimática que permite crear y abrir distintos formatos de documentos editables (texto plano `.txt` y documento enriquecido `.rtf`) sin acoplar el código cliente a las clases concretas de cada formato.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** definiendo una interfaz común (`Documento`) y una clase creadora abstracta (`CreadorDocumento`) que centraliza la operación de apertura (`abrirDocumento()`) y declara el método fábrica abstracto `crearDocumento()`. De este modo, la instanciación concreta con `new` queda delegada exclusivamente en las subclases especializadas (`CreadorTextoPlano` y `CreadorDocumentoEnriquecido`), permitiendo que el cliente opere contra las abstracciones sin acoplarse a los formatos concretos.

## Evidencia de Diseño (UML)
<img width="468" height="314" alt="01 drawio" src="https://github.com/user-attachments/assets/11906da0-d5c0-4bf7-9ae7-609cb658e7e0" />

## Fragmento de Código Clave
```java
public interface Documento {
    void abrir();
}

public abstract class CreadorDocumento {
    public abstract Documento crearDocumento();

    public void abrirDocumento() {
        Documento doc = crearDocumento();
        doc.abrir();
    }
}

public class CreadorTextoPlano extends CreadorDocumento {
    @Override
    public Documento crearDocumento() {
        return new DocumentoTextoPlano();
    }
}
```

## Evidencia de Ejecución
<img width="520" height="202" alt="{F569D2EE-0DC9-418B-B600-20978D813969}" src="https://github.com/user-attachments/assets/f3a4c704-49f4-4235-b04d-8c3a9b8152de" />

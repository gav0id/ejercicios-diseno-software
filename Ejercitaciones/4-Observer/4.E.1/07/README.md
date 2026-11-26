# Ejercicio 7: Sistema de Suscripción a Canales de Noticias Digitales

## Explicación del Proyecto y Lógica
**Descripción:** Portal de noticias en línea (`PortalNoticias`) que permite a los lectores (`UsuarioLector`) suscribirse y desuscribirse de categorías temáticas específicas (`Tecnología`, `Deportes`, `Economía`), asegurando que al publicarse un nuevo artículo (`Noticia`) solo los usuarios suscritos a esa categoría reciban la notificación.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** con filtrado por tópicos mediante la interfaz `LectorNoticias`, el objeto inmutable `Noticia` y el gestor central `PortalNoticias`. `PortalNoticias` actúa como intermediario manteniendo un diccionario `Map<String, List<LectorNoticias>>` indexado por categoría, y al invocarse `publicar(Noticia noticia)` despacha el aviso exclusivamente a los suscriptores registrados en esa clave temática a través de la interfaz unificada `recibirNoticia(Noticia noticia)`.

## Evidencia de Diseño (UML)
<img width="852" height="380" alt="07 drawio" src="https://github.com/user-attachments/assets/7f9bcc73-562c-4e63-8053-ebd3e658d618" />

## Fragmento de Código Clave
```java
public interface LectorNoticias {
    void recibirNoticia(Noticia noticia);
}

public record Noticia(String categoria, String titulo, String resumen) {}

public void publicar(Noticia noticia) {
    if (noticia == null || noticia.categoria() == null) return;
    List<LectorNoticias> lista = canales.get(noticia.categoria().trim().toUpperCase());
    if (lista == null) return;
    for (LectorNoticias lector : new ArrayList<>(lista)) {
        lector.recibirNoticia(noticia);
    }
}
```

## Evidencia de Ejecución
<img width="884" height="252" alt="{60DEBC8D-5901-4D93-8437-1D0798A7E7E1}" src="https://github.com/user-attachments/assets/52282ba9-b0a6-4177-9496-b23a42271ade" />

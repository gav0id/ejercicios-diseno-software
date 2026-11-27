# Ejercicio 7: Sistema de Suscripción a Canales de Video Streaming

## Explicación del Proyecto y Lógica
**Descripción:** Plataforma de contenidos multimedia (`PlataformaStreaming`) que permite a los usuarios (`UsuarioStreaming`) suscribirse y desuscribirse de canales de creadores (`@TeamCherry`, `@DECO27`), enviando alertas únicamente a los seguidores del canal cuando se publica un nuevo `Video`.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** con filtrado por identificador de canal mediante la interfaz `SuscriptorCanal`, el `record` inmutable `Video` y el gestor `PlataformaStreaming`. `PlataformaStreaming` mantiene un `Map<String, List<SuscriptorCanal>>` indexado por `canalId` y notifica de forma selectiva a los seguidores suscritos a ese emisor a través de `recibirAlerta(Video)`, desacoplando la publicación de videos de la recepción de alertas.

## Evidencia de Diseño (UML)
<img width="882" height="366" alt="07 drawio" src="https://github.com/user-attachments/assets/63c54d32-de83-4bdc-bb0e-caf97fe7f424" />

## Fragmento de Código Clave
```java
public record Video(String canalId, String titulo, int duracionMin) {}

public interface SuscriptorCanal {
    void recibirAlerta(Video video);
}

public void publicarVideo(Video video) {
    if (video == null || video.canalId() == null) return;
    List<SuscriptorCanal> seguidores = canales.get(video.canalId().trim().toLowerCase());
    if (seguidores == null) return;
    for (SuscriptorCanal u : new ArrayList<>(seguidores)) {
        u.recibirAlerta(video);
    }
}
```

## Evidencia de Ejecución
<img width="895" height="254" alt="imagen" src="https://github.com/user-attachments/assets/886b3b7b-61fc-4938-bc0e-0e858f35a798" />

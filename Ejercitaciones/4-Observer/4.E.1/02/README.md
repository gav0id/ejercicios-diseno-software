# Ejercicio 2: Sincronizador de Interfaz para Reproductor Multimedia

## Explicación del Proyecto y Lógica
**Descripción:** Reproductor multimedia que sincroniza en tiempo real tres vistas de interfaz (`BarraControl`, `VentanaMiniatura` y `WidgetPrincipal`) ante cambios de estado (`REPRODUCIENDO`, `PAUSADO`, `DETENIDO`) y de canción, permitiendo suscribir y desuscribir vistas dinámicamente.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `VistaReproductor` y el sujeto observable `ReproductorMultimedia`. El motor encapsula la colección de vistas (`List<VistaReproductor>`) y notifica pasivamente a cada suscriptor mediante `actualizar(estado, cancion)` al invocar `cambiarEstado()`, evitando consultas periódicas (*polling*) y desacoplando el estado del reproductor de sus representaciones visuales.

## Evidencia de Diseño (UML)
<img width="962" height="214" alt="02 drawio" src="https://github.com/user-attachments/assets/999470ae-a884-4b67-b47c-762b46730303" />

## Fragmento de Código Clave
```java
public interface VistaReproductor {
    void actualizar(String estado, String cancion);
}

public void cambiarEstado(String nuevoEstado, String nuevaCancion) {
    this.estado = nuevoEstado;
    this.cancion = nuevaCancion;
    notificar();
}

private void notificar() {
    for (VistaReproductor v : new ArrayList<>(vistas)) {
        v.actualizar(estado, cancion);
    }
}
```

## Evidencia de Ejecución
<img width="621" height="338" alt="{F7D2F70D-DF61-42A9-BEE2-DCE12ACA687B}" src="https://github.com/user-attachments/assets/07b3e0cf-c22e-4c4d-addf-082722e84086" />

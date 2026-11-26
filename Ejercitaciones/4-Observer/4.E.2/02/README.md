# Ejercicio 2: Indicador de Estado de Conexión de Red

## Explicación del Proyecto y Lógica
**Descripción:** Aplicación de escritorio que supervisa el estado de la red local (`MonitorRed`) y sincroniza de manera automática y simultánea un icono en la barra de herramientas (`IconoBarraHerramientas`) y un mensaje en la barra de estado (`BarraEstado`) ante cambios entre `Conectado`, `Intermitente` y `Desconectado`.

**Lenguaje:** Java

**Justificación Lógica (Patrón Observer):**  
Se implementó el patrón **Observer** mediante la interfaz `ObservadorRed` y el sujeto observable `MonitorRed`. `MonitorRed` encapsula la colección `List<ObservadorRed>` y notifica pasivamente a los componentes suscritos mediante `actualizar(estado)` al ejecutarse `cambiarEstado()`, evitando consultas periódicas (*polling*) y desacoplando el estado de la red de sus representaciones visuales.

## Evidencia de Diseño (UML)
<img width="832" height="266" alt="02 drawio" src="https://github.com/user-attachments/assets/4343abbd-3eb7-4835-8ccf-5f159237b72b" />

## Fragmento de Código Clave
```java
public interface ObservadorRed {
    void actualizar(String estado);
}

public void cambiarEstado(String nuevoEstado) {
    if (nuevoEstado == null || nuevoEstado.isBlank() || nuevoEstado.equalsIgnoreCase(estado)) return;
    this.estado = nuevoEstado;
    notificar();
}

private void notificar() {
    for (ObservadorRed o : new ArrayList<>(observadores)) {
        o.actualizar(estado);
    }
}
```

## Evidencia de Ejecución
<img width="540" height="276" alt="imagen" src="https://github.com/user-attachments/assets/83b9d40a-ba5d-41ff-b4fa-805ddee6bc92" />

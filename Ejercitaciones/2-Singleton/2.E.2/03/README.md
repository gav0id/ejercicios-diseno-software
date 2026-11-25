# Ejercicio 3: Catálogo Compartido de Libros

## Explicación del Proyecto y Lógica
**Descripción:** Sistema de gestión para una biblioteca compuesto por dos pantallas (`PantallaPersonal` y `PantallaConsulta`) que operan sobre un catálogo compartido de libros. Permite incorporar títulos con código único, actualizar la cantidad de ejemplares, listar el catálogo completo y buscar por título o autor, reflejando los cambios de forma inmediata en ambas vistas.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** en `CatalogoLibros` para centralizar la colección de libros en una única instancia compartida durante toda la ejecución, evitando listas desincronizadas entre pantallas. El acceso controlado mediante `obtenerInstancia()` garantiza que los cambios realizados desde `PantallaPersonal` sean visibles al instante desde `PantallaConsulta`, mientras que la encapsulación del arreglo interno, las copias defensivas en las búsquedas y la inmutabilidad de `Libro` protegen la integridad de los datos.

## Evidencia de Diseño (UML)
<img width="882" height="534" alt="03 drawio" src="https://github.com/user-attachments/assets/0b6554ed-265b-4dfd-9e28-c483fdf88711" />


## Fragmento de Código Clave
```java
public class CatalogoLibros {
    private static CatalogoLibros instancia;
    private Libro[] libros;
    private int cantidad;

    private CatalogoLibros() {
        libros = new Libro[3];
        cantidad = 0;
    }

    public static CatalogoLibros obtenerInstancia() {
        if (instancia == null) {
            instancia = new CatalogoLibros();
        }
        return instancia;
    }

    public Libro[] obtenerTodos() {
        Libro[] copia = new Libro[cantidad];
        for (int i = 0; i < cantidad; i++) {
            copia[i] = libros[i];
        }
        return copia;
    }
}
```

## Evidencia de Ejecución
<img width="624" height="460" alt="imagen" src="https://github.com/user-attachments/assets/de678566-5561-4e16-8dcd-5b56845a2790" />

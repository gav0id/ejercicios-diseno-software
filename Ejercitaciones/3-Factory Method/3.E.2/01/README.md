# Ejercicio 1: Generador Básico de Perfiles de Usuario

## Explicación del Proyecto y Lógica
**Descripción:** Módulo de registro para una red social universitaria que permite crear distintos tipos de perfiles de usuario (`Estudiante` y `Docente`), consultando el rol y los permisos básicos asignados a cada uno sin acoplar la interfaz de registro a las clases concretas.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** definiendo la interfaz común `PerfilUsuario` y la clase creadora abstracta `CreadorPerfil`, que centraliza el flujo de registro (`registrarUsuario(String nombre)`) y declara el método fábrica abstracto `crearPerfil()`. Las subclases `CreadorEstudiante` y `CreadorDocente` encapsulan la instanciación concreta de cada perfil, evitando el uso directo de `new` sobre los productos desde el código cliente.

## Evidencia de Diseño (UML)
<img width="1042" height="236" alt="01 drawio" src="https://github.com/user-attachments/assets/02c814b3-5b9d-4680-b7e5-1a0affe15a0a" />

## Fragmento de Código Clave
```java
public interface PerfilUsuario {
    String getRol();
    String getPermisos();
}

public abstract class CreadorPerfil {
    public abstract PerfilUsuario crearPerfil();

    public void registrarUsuario(String nombre) {
        PerfilUsuario perfil = crearPerfil();
        System.out.println("Registro - " + nombre + " - Rol: " + perfil.getRol() + " - Permisos: " + perfil.getPermisos());
    }
}

public class CreadorEstudiante extends CreadorPerfil {
    @Override
    public PerfilUsuario crearPerfil() { return new PerfilEstudiante(); }
}
```

## Evidencia de Ejecución
<img width="826" height="217" alt="{68B6748D-88BB-40B6-AA59-6C3DD55F772A}" src="https://github.com/user-attachments/assets/49fe633e-3ed6-4438-bea9-68aac5fe7c25" />

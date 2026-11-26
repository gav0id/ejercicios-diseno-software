# Ejercicio 10: Sistema Empresarial de Emisión de Títulos de Propiedad

## Explicación del Proyecto y Lógica
**Descripción:** Sistema notarial automatizado multi-jurisdicción (`Mendoza - MZA` y `Córdoba - CBA`) que valida el contexto registral, el DNI del titular y la matrícula catastral, y emite títulos de propiedad electrónicos inmutables con código notarial, firma digital SHA-256 y texto documental oficial en entornos multihilo.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz `EmisorNotarial` y la clase creadora abstracta `CreadorTitulo`. El método `final` `emitirTitulo()` valida previamente la jurisdicción del contexto y delega la creación del emisor en el método fábrica protegido `crearEmisor()`, sobrescrito por `CreadorMendoza` y `CreadorCordoba`. La inmutabilidad del `record` `TituloPropiedad`, el uso de clases y métodos `final`, la ausencia de estado mutable compartido y la validación estricta de datos garantizan seguridad ante concurrencia y protección frente a parámetros inválidos.

## Evidencia de Diseño (UML)
<img width="1202" height="536" alt="10 drawio" src="https://github.com/user-attachments/assets/f72701ff-b1cd-49a9-a838-499dba52ee66" />

## Fragmento de Código Clave
```java
public interface EmisorNotarial {
    TituloPropiedad emitir(String jurisdiccion, String dniTitular, String matricula);
}

public abstract class CreadorTitulo {
    private final String jurisdiccion;

    protected CreadorTitulo(String jurisdiccion) {
        if (jurisdiccion == null || jurisdiccion.isBlank()) {
            throw new IllegalArgumentException("Jurisdiccion obligatoria");
        }
        this.jurisdiccion = jurisdiccion.trim().toUpperCase();
    }

    protected abstract EmisorNotarial crearEmisor();

    public final TituloPropiedad emitirTitulo(String contextoJurisdiccion, String dniTitular, String matricula) {
        if (contextoJurisdiccion == null || !contextoJurisdiccion.trim().equalsIgnoreCase(jurisdiccion)) {
            throw new IllegalArgumentException("Contexto invalido - se esperaba " + jurisdiccion + " y llego " + contextoJurisdiccion);
        }
        EmisorNotarial emisor = Objects.requireNonNull(crearEmisor(), "Emisor nulo");
        return emisor.emitir(
                jurisdiccion,
                dniTitular == null ? null : dniTitular.trim(),
                matricula == null ? null : matricula.trim().toUpperCase()
        );
    }
}
```

## Evidencia de Ejecución
<img width="1021" height="259" alt="{F7E7AFB6-2FAD-43CB-A816-0F7488689198}" src="https://github.com/user-attachments/assets/a08dc420-9934-4cb5-9669-793a345984b1" />

# Ejercicio 10: Sistema Empresarial de Emisión de Comprobantes Fiscales

## Explicación del Proyecto y Lógica
**Descripción:** Sistema empresarial de facturación electrónica multi-jurisdicción (`Argentina` y `Chile`) que valida el contexto tributario, verifica identificadores fiscales (CUIT/RUT) y montos, y genera comprobantes inmutables con código de autorización, sello digital SHA-256 y formato XML específico de cada país en entornos multihilo.

**Lenguaje:** Java

**Justificación Lógica (Patrón Factory Method):**  
Se implementó el patrón **Factory Method** mediante la interfaz `EmisorFiscal` y la clase creadora abstracta `CreadorComprobante`. El método `final` `generarComprobante()` garantiza la validación previa de la jurisdicción y delega la instanciación del emisor en el método fábrica protegido `crearEmisor()`, sobrescrito por `CreadorArgentina` y `CreadorChile`. Al combinar clases `final`, el `record` inmutable `ComprobanteFiscal` y emisores sin estado mutable compartido, se asegura ejecución segura bajo concurrencia y protección contra configuraciones inválidas.

## Evidencia de Diseño (UML)
<img width="1163" height="582" alt="10 drawio" src="https://github.com/user-attachments/assets/50440b89-2bd4-454b-974b-96d13a07a6c0" />

## Fragmento de Código Clave
```java
public interface EmisorFiscal {
    ComprobanteFiscal emitir(String pais, String cuit, double monto);
}

public abstract class CreadorComprobante {
    private final String jurisdiccion;

    protected CreadorComprobante(String jurisdiccion) {
        if (jurisdiccion == null || jurisdiccion.isBlank()) {
            throw new IllegalArgumentException("Jurisdiccion obligatoria");
        }
        this.jurisdiccion = jurisdiccion.trim().toUpperCase();
    }

    protected abstract EmisorFiscal crearEmisor();

    public final ComprobanteFiscal generarComprobante(String pais, String cuit, double monto) {
        if (pais == null || !pais.trim().equalsIgnoreCase(jurisdiccion)) {
            throw new IllegalArgumentException("Contexto invalido - se esperaba " + jurisdiccion + " y llego " + pais);
        }
        EmisorFiscal emisor = Objects.requireNonNull(crearEmisor(), "Emisor nulo");
        return emisor.emitir(jurisdiccion, cuit == null ? null : cuit.trim(), monto);
    }
}
```

## Evidencia de Ejecución
<img width="1128" height="219" alt="{04671319-83B4-47F5-B4D3-1BFCEBD35F94}" src="https://github.com/user-attachments/assets/75e400fc-34c5-4688-9268-f4a733223869" />

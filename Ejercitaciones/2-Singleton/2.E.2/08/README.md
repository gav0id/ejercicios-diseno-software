# Ejercicio 8: Registro Compartido de Movimientos de Insumos

## Explicación del Proyecto y Lógica
**Descripción:** Sistema de control de inventario hospitalario donde los sectores de depósito, consultorios y laboratorio (`SectorHospital`) registran entradas y salidas de insumos (`TipoMovimiento`) en un historial unificado. Permite calcular el saldo actual de cada insumo a partir de sus movimientos, consultar el historial completo y filtrarlo por código de insumo y/o sector.

**Lenguaje:** Java

**Justificación Lógica (Patrón Singleton):**  
Se implementó el patrón **Singleton** en `RegistroInsumos` para centralizar todos los movimientos en una única instancia compartida entre los tres sectores, evitando registros dispersos e inconsistencias de stock. Al encapsular la colección interna, exponer consultas mediante copias defensivas y modelar cada `Movimiento` como un objeto inmutable sin operaciones de edición ni eliminación, se garantiza la trazabilidad y la integridad del historial.

## Evidencia de Diseño (UML)
<img width="1012" height="413" alt="08 drawio" src="https://github.com/user-attachments/assets/7b4d112c-7b2d-4680-ad27-a7bc50c187b2" />

## Fragmento de Código Clave
```java
public class RegistroInsumos {
    private static RegistroInsumos instancia;
    private Movimiento[] movimientos = new Movimiento[4];
    private int cantidad = 0;

    private RegistroInsumos() {}

    public static RegistroInsumos obtenerInstancia() {
        if (instancia == null) {
            instancia = new RegistroInsumos();
        }
        return instancia;
    }

    public boolean registrar(String codigoInsumo, TipoMovimiento tipo, int monto, String sector, LocalDate fecha) {
        if (codigoInsumo == null || codigoInsumo.isBlank() || tipo == null
                || monto <= 0 || sector == null || sector.isBlank() || fecha == null) {
            return false;
        }
        String codigo = codigoInsumo.trim();
        if (tipo == TipoMovimiento.SALIDA && monto > calcularSaldo(codigo)) {
            return false;
        }
        if (cantidad == movimientos.length) {
            Movimiento[] nuevo = new Movimiento[movimientos.length * 2];
            for (int i = 0; i < cantidad; i++) nuevo[i] = movimientos[i];
            movimientos = nuevo;
        }
        movimientos[cantidad++] = new Movimiento(codigo, tipo, monto, sector.trim(), fecha);
        return true;
    }
}
```

## Evidencia de Ejecución
<img width="485" height="455" alt="{F33894EC-4596-41CF-B052-5DE1820839E3}" src="https://github.com/user-attachments/assets/dff7f063-e93f-47db-b5ee-c9477ac5bfac" />

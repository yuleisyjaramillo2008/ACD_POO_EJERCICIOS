<h1 align="center">Principios SOLID en Java</h1>

<h2 align="center">SRP – Principio de Responsabilidad Única</h2>

---

## ❌ SRP · Violación

`Factura` tiene tres motivos de cambio: calcular el total, guardar en la base de datos y generar el PDF.

```java
class Factura {
    double subtotal = 100;

    double calcularTotal() {
        return subtotal * 1.15;
    }

    void guardarEnBaseDeDatos() {
        System.out.println("Guardando factura en la base de datos...");
    }

    void generarPDF() {
        System.out.println("Generando PDF de la factura...");
    }
}

public class Main {
    public static void main(String[] args) {
        Factura f = new Factura();
        System.out.println("Total: " + f.calcularTotal());
        f.guardarEnBaseDeDatos();
        f.generarPDF();
    }
}
```

---

## ✅ SRP · Refactorización

Cada responsabilidad pasa a su propia clase: `Factura` calcula, `FacturaRepository` guarda y `FacturaPrinter` genera el PDF.

```java
class Factura {
    private double subtotal;

    Factura(double subtotal) {
        this.subtotal = subtotal;
    }

    double calcularTotal() {
        return subtotal * 1.15;
    }
}

class FacturaRepository {
    void guardar(Factura factura) {
        System.out.println("Guardando en la base de datos. Total: " + factura.calcularTotal());
    }
}

class FacturaPrinter {
    void generarPDF(Factura factura) {
        System.out.println("Generando PDF. Total: " + factura.calcularTotal());
    }
}

public class Main {
    public static void main(String[] args) {
        Factura factura = new Factura(100);
        new FacturaRepository().guardar(factura);
        new FacturaPrinter().generarPDF(factura);
    }
}
```

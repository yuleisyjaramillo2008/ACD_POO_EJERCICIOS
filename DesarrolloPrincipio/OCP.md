<h1 align="center">Principios SOLID en Java</h1>

<h2 align="center">OCP – Principio Abierto/Cerrado</h2>

---

## ❌ OCP · Violación

Cada nuevo tipo de cliente obliga a modificar `calcular()` agregando otro `if`.

```java
class CalculadoraDescuentos {
    double calcular(double total, String tipoCliente) {
        if (tipoCliente.equals("NORMAL")) {
            return total;
        }

        if (tipoCliente.equals("VIP")) {
            return total * 0.9;
        }

        if (tipoCliente.equals("EMPRESA")) {
            return total * 0.85;
        }

        return total;
    }
}

public class Main {
    public static void main(String[] args) {
        CalculadoraDescuentos calc = new CalculadoraDescuentos();
        System.out.println("VIP: " + calc.calcular(100, "VIP"));
        System.out.println("EMPRESA: " + calc.calcular(100, "EMPRESA"));
    }
}
```

---

## ✅ OCP · Refactorización

Se crea la interfaz `Descuento`; cada tipo de cliente es una clase nueva y `CalculadoraDescuentos` no se modifica.

```java
interface Descuento {
    double aplicar(double total);
}

class DescuentoNormal implements Descuento {
    public double aplicar(double total) {
        return total;
    }
}

class DescuentoVIP implements Descuento {
    public double aplicar(double total) {
        return total * 0.9;
    }
}

class DescuentoEmpresa implements Descuento {
    public double aplicar(double total) {
        return total * 0.85;
    }
}

class CalculadoraDescuentos {
    double calcular(double total, Descuento descuento) {
        return descuento.aplicar(total);
    }
}

public class Main {
    public static void main(String[] args) {
        CalculadoraDescuentos calc = new CalculadoraDescuentos();
        System.out.println("VIP: " + calc.calcular(100, new DescuentoVIP()));
        System.out.println("EMPRESA: " + calc.calcular(100, new DescuentoEmpresa()));
    }
}
```

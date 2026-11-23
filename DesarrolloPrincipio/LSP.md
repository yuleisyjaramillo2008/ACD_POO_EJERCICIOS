<h1 align="center">Principios SOLID en Java</h1>

<h2 align="center">LSP – Principio de Sustitución de Liskov</h2>

---

## ❌ LSP · Violación

`Pinguino` hereda `volar()` de `Ave`, pero lanza una excepción: no puede reemplazar a su clase base.

```java
class Ave {
    void volar() { System.out.println("Vuela"); }
}

class Pinguino extends Ave {
    @Override
    void volar() {
        throw new UnsupportedOperationException();
    }
}

public class Main {
    public static void main(String[] args) {
        Ave ave = new Pinguino();
        ave.volar(); // lanza excepción
    }
}
```

---

## ✅ LSP · Refactorización

`volar()` pasa a `AveVoladora`; `Pinguino` hereda solo de `Ave` y el compilador impide usarlo como ave voladora.

```java
class Ave {
    void comer() { System.out.println("Come"); }
}

class AveVoladora extends Ave {
    void volar() { System.out.println("Vuela"); }
}

class Gorrion extends AveVoladora { }
class Pinguino extends Ave { }

public class Main {
    public static void main(String[] args) {
        AveVoladora ave = new Gorrion();
        ave.volar(); // Vuela
    }
}
```

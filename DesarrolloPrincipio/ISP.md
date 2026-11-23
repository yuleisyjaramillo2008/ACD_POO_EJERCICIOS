<h1 align="center">Principios SOLID en Java</h1>

<h2 align="center">ISP – Principio de Segregación de Interfaces</h2>

---

## ❌ ISP · Violación

`ImpresoraBasica` está obligada a implementar `escanear()` y `enviarFax()`, que no usa, y los resuelve lanzando una excepción.

```java
interface DispositivoMultifuncion {
    void imprimir(String doc);
    void escanear(String doc);
    void enviarFax(String doc);
}

class ImpresoraBasica implements DispositivoMultifuncion {
    public void imprimir(String doc) {
        System.out.println("Imprimiendo: " + doc);
    }

    public void escanear(String doc) {
        throw new UnsupportedOperationException();
    }

    public void enviarFax(String doc) {
        throw new UnsupportedOperationException();
    }
}

public class Main {
    public static void main(String[] args) {
        DispositivoMultifuncion impresora = new ImpresoraBasica();
        impresora.imprimir("Informe.pdf");
        impresora.escanear("Informe.pdf"); // lanza excepción
    }
}
```

---

## ✅ ISP · Refactorización

La interfaz grande se divide en `Imprimible`, `Escaneable` y `EnviableFax`; cada clase implementa solo las que necesita.

```java
interface Imprimible {
    void imprimir(String doc);
}

interface Escaneable {
    void escanear(String doc);
}

interface EnviableFax {
    void enviarFax(String doc);
}

class ImpresoraBasica implements Imprimible {
    public void imprimir(String doc) {
        System.out.println("Imprimiendo: " + doc);
    }
}

class ImpresoraMultifuncion implements Imprimible, Escaneable, EnviableFax {
    public void imprimir(String doc) {
        System.out.println("Imprimiendo: " + doc);
    }

    public void escanear(String doc) {
        System.out.println("Escaneando: " + doc);
    }

    public void enviarFax(String doc) {
        System.out.println("Enviando fax: " + doc);
    }
}

public class Main {
    public static void main(String[] args) {
        Imprimible basica = new ImpresoraBasica();
        basica.imprimir("Informe.pdf");

        ImpresoraMultifuncion multifuncion = new ImpresoraMultifuncion();
        multifuncion.imprimir("Informe.pdf");
        multifuncion.escanear("Informe.pdf");
        multifuncion.enviarFax("Informe.pdf");
    }
}
```

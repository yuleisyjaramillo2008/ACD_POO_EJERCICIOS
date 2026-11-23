<h1 align="center">Principios SOLID en Java</h1>

<h2 align="center">DIP – Principio de Inversión de Dependencias</h2>

---

## ❌ DIP · Violación

`ServicioPedidos` crea directamente un `EmailService` con `new`: depende de una clase concreta y no se puede cambiar a SMS sin modificarlo.

```java
class EmailService {
    void enviarCorreo(String mensaje) {
        System.out.println("Enviando correo: " + mensaje);
    }
}

class ServicioPedidos {
    private EmailService email = new EmailService();

    void confirmarPedido(String cliente) {
        email.enviarCorreo("Pedido confirmado");
    }
}

public class Main {
    public static void main(String[] args) {
        ServicioPedidos servicio = new ServicioPedidos();
        servicio.confirmarPedido("Tatiana");
    }
}
```

---

## ✅ DIP · Refactorización

`ServicioPedidos` depende de la interfaz `Notificador` y recibe la implementación por el constructor (inyección de dependencias).

```java
interface Notificador {
    void enviar(String mensaje);
}

class EmailNotificador implements Notificador {
    public void enviar(String mensaje) {
        System.out.println("Enviando correo: " + mensaje);
    }
}

class SmsNotificador implements Notificador {
    public void enviar(String mensaje) {
        System.out.println("Enviando SMS: " + mensaje);
    }
}

class ServicioPedidos {
    private final Notificador notificador;

    ServicioPedidos(Notificador notificador) {
        this.notificador = notificador;
    }

    void confirmarPedido(String cliente) {
        notificador.enviar("Pedido confirmado");
    }
}

public class Main {
    public static void main(String[] args) {
        ServicioPedidos porEmail = new ServicioPedidos(new EmailNotificador());
        porEmail.confirmarPedido("Tatiana");

        ServicioPedidos porSms = new ServicioPedidos(new SmsNotificador());
        porSms.confirmarPedido("Josselyn");
    }
}
```

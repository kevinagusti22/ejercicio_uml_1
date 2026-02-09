### Ejemplo de código

```java
import java.util.Scanner;

/**
 * Clase que define la estructura de un Producto
 */
class Producto {
    private String nombre;
    private double precioBase;

    public Producto(String nombre, double precioBase) {
        this.nombre = nombre;
        this.precioBase = precioBase;
    }

    public double getPrecioBase() { return precioBase; }
}

/**
 * Clase con la lógica de negocio (Impuestos)
 */
class CalculadoraIVA {
    private final double IVA = 0.21;

    public double calcularPrecioFinal(double precio) {
        return precio * (1 + IVA);
    }
}

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System. some);
        CalculadoraIVA calc = new CalculadoraIVA();

        System.out.print("Nombre del producto: ");
        String n = sc.nextLine();
        System.out.print("Precio base: ");
        double p = sc.nextDouble();

        Producto prod = new Producto(n, p);
        System.out.println("Total con IVA: " + calc.calcularPrecioFinal(prod.getPrecioBase()));
    }
}
```

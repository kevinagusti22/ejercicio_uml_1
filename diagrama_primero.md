## Diagrama de Clases Calculadora completa
```mermaid
classDiagram
direction LR

class Main {
  +main(args: String[]) void
}

class ControladorCalculadora {
  -entrada: IEntradaDatos
  -salida: ISalida
  -gestor: GestorOperaciones
  +ejecutar() void
}

class GestorOperaciones {
  -calculadora: Calculadora
  +resolver(op: String, a: double, b: double) double
}

class Calculadora {
  +sumar(a: double, b: double) double
  +restar(a: double, b: double) double
  +multiplicar(a: double, b: double) double
  +dividir(a: double, b: double) double
}

class DivisionPorCeroException {
}

class IEntradaDatos {
  <<interface>>
  +leerNumero(mensaje: String) double
  +leerOperacion(mensaje: String) String
}

class EntradaConsola {
  +leerNumero(mensaje: String) double
  +leerOperacion(mensaje: String) String
}

class ISalida {
  <<interface>>
  +mostrarResultado(resultado: double) void
  +mostrarError(mensaje: String) void
}

class SalidaConsola {
  +mostrarResultado(resultado: double) void
  +mostrarError(mensaje: String) void
}

class VistaGUI {
  +mostrarResultado(resultado: double) void
  +mostrarError(mensaje: String) void
}

Main --> ControladorCalculadora : inicia
ControladorCalculadora ..> IEntradaDatos : usa
ControladorCalculadora ..> ISalida : usa
ControladorCalculadora --> GestorOperaciones : delega
GestorOperaciones --> Calculadora : invoca

EntradaConsola ..|> IEntradaDatos
SalidaConsola ..|> ISalida
VistaGUI ..|> ISalida

Calculadora ..> DivisionPorCeroException : lanza
GestorOperaciones ..> DivisionPorCeroException : propaga
ControladorCalculadora ..> DivisionPorCeroException : captura

```
## Diagrama de Clases Calculadora simplificada:
```mermaid
classDiagram
direction LR

class Main {
  +main(args: String[]) void
}

class ControladorCalculadora {
  -entrada: EntradaConsola
  -salida: SalidaConsola
  -gestor: GestorOperaciones
  +ejecutar() void
}

class GestorOperaciones {
  -calculadora: Calculadora
  +resolver(op: String, a: double, b: double) double
}

class Calculadora {
  +sumar(a: double, b: double) double
  +restar(a: double, b: double) double
  +multiplicar(a: double, b: double) double
  +dividir(a: double, b: double) double
}

class DivisionPorCeroException {
}

class EntradaConsola {
  +leerNumero(mensaje: String) double
  +leerOperacion(mensaje: String) String
}

class SalidaConsola {
  +mostrarResultado(resultado: double) void
  +mostrarError(mensaje: String) void
}

Main --> ControladorCalculadora : inicia
ControladorCalculadora --> EntradaConsola : usa
ControladorCalculadora --> SalidaConsola : usa
ControladorCalculadora --> GestorOperaciones : delega
GestorOperaciones --> Calculadora : invoca

Calculadora ..> DivisionPorCeroException : lanza
GestorOperaciones ..> DivisionPorCeroException : propaga
ControladorCalculadora ..> DivisionPorCeroException : captura
```
## Diagrama de secuencia Calculadora sencilla
```mermaid
sequenceDiagram
autonumber

actor Usuario
participant Main
participant Controlador as ControladorCalculadora
participant Entrada as EntradaConsola
participant Gestor as GestorOperaciones
participant Calc as Calculadora
participant Salida as SalidaConsola

Main->>Controlador: ejecutar()

Controlador->>Entrada: leerNumero("Introduce el primer número")
Entrada-->>Controlador: a

Controlador->>Entrada: leerNumero("Introduce el segundo número")
Entrada-->>Controlador: b

Controlador->>Entrada: leerOperacion("Elige operación (+,-,*,/)")
Entrada-->>Controlador: op

Controlador->>Gestor: resolver(op, a, b)
Gestor->>Calc: ejecutarOperación(op, a, b)

alt op == "+"
  Calc-->>Gestor: sumar(a,b)
else op == "-"
  Calc-->>Gestor: restar(a,b)
else op == "*"
  Calc-->>Gestor: multiplicar(a,b)
else op == "/"
  alt b == 0
    Calc--x Gestor: DivisionPorCeroException
    Gestor--x Controlador: DivisionPorCeroException
    Controlador->>Salida: mostrarError("No se puede dividir entre cero")
  else b != 0
    Calc-->>Gestor: dividir(a,b)
  end
else operación no válida
  Gestor-->>Controlador: error operación inválida
  Controlador->>Salida: mostrarError("Operación no válida")
end

opt resultado calculado correctamente
  Gestor-->>Controlador: resultado
  Controlador->>Salida: mostrarResultado(resultado)
end
```
## Diagrama de estados
```mermaid
stateDiagram-v2
[*] --> Inicio

Inicio --> PidiendoNumero1
PidiendoNumero1 --> PidiendoNumero2 : número1 válido
PidiendoNumero1 --> ErrorEntrada : número1 inválido

PidiendoNumero2 --> PidiendoOperacion : número2 válido
PidiendoNumero2 --> ErrorEntrada : número2 inválido

PidiendoOperacion --> ValidandoOperacion
ValidandoOperacion --> Calculando : operación válida
ValidandoOperacion --> ErrorOperacion : operación no válida

Calculando --> MostrandoResultado : cálculo OK
Calculando --> ErrorDivisionCero : división entre cero

MostrandoResultado --> Fin
ErrorEntrada --> Fin
ErrorOperacion --> Fin
ErrorDivisionCero --> Fin

Fin --> [*]
```mermaid

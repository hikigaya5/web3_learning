En Solidity, los **errores** son mecanismos para manejar situaciones excepcionales, revertir transacciones y proporcionar información sobre fallos. Existen dos enfoques principales:

[[Require, Revert y Assert|1. Funciones clásicas (`require`, `revert`, `assert`).]]
    
[[Errors|2. Errores personalizados (introducidos en Solidity 0.8.4, más eficientes en gas).]]

### **Errores personalizados (Custom Errors)**

Introducidos en Solidity 0.8.4, permiten definir errores con parámetros y ahorrar gas.

#### **Definición y uso**:

```Solidity
// Definir un error personalizado
error SaldoInsuficiente(address usuario, uint256 saldoActual);

function retirar(uint256 _monto) public {
    if (_monto > balance[msg.sender]) {
        revert SaldoInsuficiente(msg.sender, balance[msg.sender]);
    }
    // Lógica de retiro
}
```

#### **Ventajas**:

- **Menor costo en gas**: No almacenan strings largos en la blockchain.
    
- **Parámetros útiles**: Proporcionan datos específicos para depuración (ej: dirección, saldo).
    
- **Legibilidad**: Mejor estructuración del código.

Los errores pueden definirse tanto dentro como fuera del contrato en función de si necesitas reutilizarlos o mantenerlos encapsulados. Un ejemplo con herencia:

```Solidity
error ErrorBase(string motivo); // Definido globalmente

contract Base {
    error ErrorContratoBase(); // Definido dentro del contrato

    function fallar() public {
        revert ErrorBase("Falló en Base");
    }
}

contract Derivado is Base {
    function fallarDerivado() public {
        revert ErrorContratoBase(); // Usa el error heredado
        // También puede usar ErrorBase
    }
}
```

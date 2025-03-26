En Solidity, los **errores** son mecanismos para manejar situaciones excepcionales, revertir transacciones y proporcionar información sobre fallos. Existen dos enfoques principales:

[[Require, Revert y Assert|1. Funciones clásicas (`require`, `revert`, `assert`).]]
    
[[Errors|2. Errores personalizados (introducidos en Solidity 0.8.4, más eficientes en gas).]]

### **Funciones clásicas de manejo de errores**

**`require(condición, mensaje)`**

- **Uso**: Valida condiciones de entrada o precondiciones. Si falla, revierte la transacción.
    
- **Ejemplo**:
    
```Solidity
function transferir(uint256 _monto) public {
	require(_monto <= balance[msg.sender], "Saldo insuficiente");
	// Lógica de transferencia
}
```

- **Características**:
    
    - Devuelve el gas no utilizado al llamante.
        
    - Ideal para validar inputs o estados externos.

**`revert(mensaje)`**

- **Uso**: Revierte la transacción directamente, útil para condiciones complejas.
    
- **Ejemplo**:

```Solidity
function retirar(uint256 _monto) public {
    if (_monto > balance[msg.sender]) {
        revert("Monto excede el saldo");
    }
    // Lógica de retiro
}
```

- **Características**:
    
    - Similar a `require`, pero sin una condición explícita.
        
    - Útil en lógica condicional más compleja (ej: dentro de bucles).

`assert(condición)`

- **Uso**: Verifica invariantes internos (errores de lógica del contrato).
    
- **Ejemplo**:

```Solidity
function actualizarBalance(uint256 _nuevoBalance) public {
    balance[msg.sender] = _nuevoBalance;
    assert(balance[msg.sender] >= 0); // Nunca debería fallar
}
```

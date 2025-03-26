En Solidity, las funciones **`receive`** y **`fallback`** son mecanismos especiales para manejar transacciones que no coinciden con ninguna función definida en el contrato. Su uso es clave para gestionar transferencias de Ether y llamadas inesperadas.

### **1. Función `receive()`**

- **Propósito**:  
    Manejar transferencias **simples de Ether** (transacciones sin datos asociados).
    
- **Sintaxis**:
    
```Solidity
receive() external payable {
	// Lógica al recibir Ether sin datos
}
```
    
- **¿Cuándo se ejecuta?**
    
    - Cuando se **envía una transacción** al contrato y no se especifica una función en **calldata (msg.data)**  (ej: `send()` o `transfer()`).
        
    - Si no existe una función `receive()` o hay datos en **calldata (msg.data)**, Solidity busca una función `fallback() payable` y da error si no la encuentra y la transacción se revierte.
- Ejemplo:

```Solidity
contract Wallet {
    // Recibe Ether cuando no hay datos en la transacción
    receive() external payable {
        emit Deposito(msg.sender, msg.value);
    }

    event Deposito(address sender, uint256 monto);
}
```

### **2. Función `fallback()`**

- **Propósito**:  
    Manejar:
    
    - Llamadas a **funciones que no existen** en el contrato.
        
    - Transacciones con **datos** que no coinciden con ninguna función.
        
    - Transferencias de Ether **con datos** (si `receive()` no está definido).
        
- **Sintaxis**:

```Solidity
fallback() external payable {
    // Lógica para llamadas desconocidas o con datos
}
```

- **¿Cuándo se ejecuta?**
    
    - Cuando se llama a una **función inexistente**.
        
    - Cuando se envía Ether **con datos** y no hay `receive()`.
        
    - Si la transacción incluye Ether y `fallback()` no es `payable`, la transacción se revierte.
        
- **Ejemplo**:

```Solidity
contract Proxy {
    // Maneja llamadas a funciones no definidas
    fallback() external payable {
        // Lógica para redirigir llamadas (ej: delegatecall)
    }
}
```
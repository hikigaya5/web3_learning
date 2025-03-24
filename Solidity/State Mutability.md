El **State Mutability** (Mutabilidad del Estado) en Solidity se refiere a cómo una función interactúa con el estado de la blockchain y los datos del contrato. Define si una función puede **modificar el estado** (escribir en la blockchain), **leer el estado** (acceder a variables de estado), o **no interactuar con el estado** en absoluto. Esto es clave para optimizar costos de gas y garantizar seguridad.

### **Funciones `view`**

- **Leen** variables de estado o datos de la blockchain (ej: `block.number`).
    
- **No modifican** el estado.
	
- **Las siguientes declaraciones se consideran modificaciones del estado:**

	1. Escribir en variables de estado (almacenamiento persistente y almacenamiento transitorio).
    
	2. Emitir eventos.
    
	3. Crear otros contratos.
    
	4. Usar `selfdestruct` (autodestruir el contrato).
    
	5. Enviar Ether mediante llamadas (calls).
    
	6. Llamar a cualquier función no marcada como `view` o `pure`.
    
	7. Usar llamadas de bajo nivel (low-level calls).
    
	8. Usar ensamblado en línea (inline assembly) que contiene ciertos opcodes.
     
- **Uso típico**: Consultar datos almacenados en el contrato.


```Solidity
function obtenerSaldo() public view returns (uint) {
    return saldo;
}
```

---
### **Funciones `pure`**

- **No acceden** a variables de estado ni a datos de la blockchain (como `block.timestamp`).
    
- **No modifican** el estado.
	
- **Además de la lista de declaraciones que modifican el estado explicadas anteriormente, lo siguiente se considera lectura del estado:**

	1. Leer variables de estado (storage y transient storage).
	    
	2. Acceder a `address(this).balance` o `<address>.balance` (consultar el saldo de una dirección).
	    
	3. Acceder a cualquiera de los miembros de `block`, `tx`, `msg` (excepto `msg.sig` y `msg.data`).
	    
	4. Llamar a cualquier función no marcada como `pure`.
	    
	5. Usar ensamblado en línea (inline assembly) que contiene ciertos opcodes. 
	
- **Uso típico**: Cálculos matemáticos, operaciones con parámetros de entrada.


```Solidity
function sumar(uint a, uint b) public pure returns (uint) {
    return a + b;
}
```

---

### **Funciones `payable`**

- **Reciben ETH** a través de `msg.value`.
    
- **Pueden modificar** el estado.
    
- **Uso típico**: Procesar pagos, enviar ETH a otros contratos o direcciones.


```Solidity
function depositar() public payable {
    saldo += msg.value;
}
```
---

### **Funciones sin modificador (non-payable)**

- **Modifican el estado** (actualizan variables de estado).
    
- **No pueden recibir ETH** a menos que se marquen como `payable`.
    
- **Uso típico**: Actualizar datos en la blockchain.
    

```Solidity
function actualizarSaldo(uint nuevoSaldo) public {
    saldo = nuevoSaldo;
}
```
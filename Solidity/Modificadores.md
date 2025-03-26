En Solidity, los **modificadores** son bloques de código reutilizables que permiten añadir condiciones o lógica adicional a las funciones de un contrato inteligente. Actúan como "decoradores" que modifican el comportamiento de una función, generalmente para validar parámetros, verificar permisos o ejecutar acciones antes o después de la función principal.

### **Sintaxis básica**

```Solidity
// Definición del modificador
modifier nombreModificador {
    // Lógica previa a la ejecución de la función
    _; // Indica dónde se ejecuta el cuerpo de la función
    // Lógica posterior a la ejecución de la función
}

// Uso en una función
function miFuncion() public nombreModificador {
    // Cuerpo de la función
}
```

### **Características clave**

1. **`_;`**:
    
    - Indica dónde se inserta el código de la función modificada.
        
    - Si no se incluye `_;`, la función no se ejecutará (útil para bloquear acciones).

2. **Parámetros en modificadores**:  
	Los modificadores pueden recibir argumentos.
    
    ```Solidity
	modifier mayorQue(uint256 _valor) {
		require(msg.value > _valor, "Valor insuficiente");
		_;
	}
    
    function comprar() public payable mayorQue(0.1 ether) {
        // Lógica de compra
    }
    ```
    
3. **Combinación de modificadores**:  
    Se pueden aplicar múltiples modificadores a una función.
    
    ```Solidity
	function retirarFondos(uint256 _monto) 
		public 
		onlyOwner 
		validarMonto(_monto) 
	{
		// Lógica de retiro
	}
	```
    
4. **Herencia**:  
    Los modificadores se heredan y pueden ser sobreescritos en contratos hijos (si están marcados como `virtual`). Si deseas acceder a un modificador `m` definido en un contrato `C`, puedes usar `C.m` para referenciarlo sin necesidad de búsqueda virtual (_virtual lookup_). Solo es posible usar modificadores definidos en el contrato actual o en sus contratos base. Los modificadores también pueden definirse en bibliotecas (_libraries_), pero su uso está limitado a las funciones de la misma biblioteca.

### **Casos de uso comunes**

- **Control de acceso**: verificar que solo ciertas direcciones (como el `owner`) puedan ejecutar una función.

```Solidity
address public owner;

modifier onlyOwner() {
    require(msg.sender == owner, "Solo el owner puede llamar esta funcion");
    _;
}

function cambiarOwner(address _nuevoOwner) public onlyOwner {
    owner = _nuevoOwner;
}
```

- **Validación de inputs**: asegurar que los parámetros cumplan ciertas condiciones.

```Solidity
modifier validarMonto(uint256 _monto) {
    require(_monto > 0, "El monto debe ser mayor a cero");
    _;
}

function transferir(uint256 _monto) public validarMonto(_monto) {
    // Lógica de transferencia
}
```

- **Lógica pre/post ejecución**: ejecutar acciones antes o después de la función (ej: actualizar un estado).

```Solidity
modifier registrarAcceso() {
    emit AccesoRegistrado(msg.sender, block.timestamp);
    _;
    emit OperacionCompletada(msg.sender);
}

function operacionCritica() public registrarAcceso {
    // Lógica importante
}
```


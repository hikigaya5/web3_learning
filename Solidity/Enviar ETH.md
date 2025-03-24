En Solidity, las funciones **`transfer`**, **`send`**, y **`call`** se utilizan para enviar Ether desde un contrato inteligente a una dirección (externa o contrato). Sin embargo, tienen diferencias clave en su comportamiento, manejo de errores y uso de gas.

### **`transfer`**

- **Función**:

    ```Solidity
	destinatario.transfer(cantidad);
	```
    
- **Características**:
    
    - **Límite de gas**: Añade un límite fijo de **2300 gas** para la ejecución, suficiente para registrar un evento pero insuficiente para operaciones complejas.
        
    - **Manejo de errores**: Si el envío falla (ej: el destinatario es un contrato que consume más de 2300 gas), **revierte** toda la transacción.
        
    - **Recomendación**: Usar para envíos simples a direcciones externas (EOA) o contratos que no requieran lógica compleja.
        
- **Ejemplo**:
    
    ```Solidity
	function enviarEther(address payable destinatario) public {
		destinatario.transfer(1 ether); // Revierte si falla
	}
	```
---
### **`send`**

- **Función**:
    
    ```Solidity
	bool exito = destinatario.send(cantidad);
	```
    
- **Características**:
    
    - **Límite de gas**: Igual que `transfer` (**2300 gas**).
        
    - **Manejo de errores**: No revierte. En su lugar, devuelve un `bool` indicando éxito (`true`) o fallo (`false`).
        
    - **Recomendación**: Evitar su uso, ya que requiere manejo manual de errores y es propenso a olvidos.
        
- **Ejemplo**:

    ```Solidity
	function enviarEther(address payable destinatario) public {
		bool sendSuccess = destinatario.send(1 ether);
		require(sendSuccess, "El envio falló"); // Manejo manual del error
	}
	```
---
### **`call`**

- **Función**:
    
	```Solidity
	(bool sendSuccess, bytes memory callData) = destinatario.call{value: cantidad}("");
	```
    
- **Características**:
    
    - **Límite de gas**: Permite especificar gas personalizado (si no se especifica, usa todo el gas restante).
        
    - **Manejo de errores**: Devuelve `exito` como `bool`, pero **no revierte** automáticamente.
        
    - **Flexibilidad**: Puede ejecutar funciones de otros contratos (mediante `datos`).
        
    - **Riesgo**: Propenso a ataques de **reentrancy** si no se usa con precaución.
        
    - **Recomendación**: Usar para interactuar con contratos que necesitan más gas o cuando se requiere flexibilidad.
        
- **Ejemplo seguro**:
    
    ```Solidity
	function enviarEther(address payable destinatario) public {
		(bool sendSuccess, ) = destinatario.call{value: 1 ether, gas: 50000}("");
		require(sendSuccess, "El envio falló");
	}
	```
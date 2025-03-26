**Foundry** es un **framework de desarrollo para Ethereum** diseñado para facilitar la escritura, prueba, implementación y depuración de contratos inteligentes. Es conocido por su **alto rendimiento** y su enfoque en la simplicidad, utilizando Rust como lenguaje base. Fue creado por la empresa **Paradigm** y se ha popularizado por su velocidad y herramientas innovadoras.

### **Componentes principales de Foundry**

1. **[[Forge]]**:
    
    - **Herramienta de testing** para escribir y ejecutar pruebas en Solidity (en lugar de JavaScript/TypeScript).
        
    - Soporta **fuzzing** (pruebas con entradas aleatorias) y **debugging avanzado**.
        
    - Ejemplo:
	    
	```Solidity
	function testTransferencia() public {
		vm.prank(usuario); // Simula una llamada desde "usuario"
		token.transfer(destino, 100);
		assertEq(token.balanceOf(destino), 100);
	}
	```
        
2. **Cast**:
    
    - **CLI para interactuar con Ethereum** (llamar a contratos, enviar transacciones, consultar datos).
        
    - Ejemplo:
        
	```Bash
	cast send 0x... "transfer(address,uint256)" 0x... 100 --private-key ...
	```
        
3. **[[Anvil]]**:
    
    - **Nodo Ethereum local** para desarrollo, similar a Ganache o Hardhat Network.
        
    - Permite minar bloques instantáneamente y configurar cuentas con fondos.
        
4. **Chisel**:
    
    - **REPL de Solidity** (consola interactiva) para prototipar código rápidamente.
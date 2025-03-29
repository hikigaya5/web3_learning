**Forge** es la herramienta de **testing y despliegue** del ecosistema **Foundry**, diseñada para desarrollar, probar y desplegar contratos inteligentes en Ethereum de manera rápida y eficiente. A diferencia de frameworks como Hardhat o Truffle, Forge permite escribir pruebas directamente en **Solidity**, lo que facilita la coherencia entre el código del contrato y las pruebas.

### **Características principales**

1. **Testing en Solidity**:
    
    - Escribe pruebas en el mismo lenguaje que los contratos (Solidity), evitando cambios de contexto a JavaScript/TypeScript.
        
    - Ejemplo:
    
    ```Solidity
    function testTransferencia() public {
	    token.transfer(alice, 100);
	    assertEq(token.balanceOf(alice), 100);
	}
	```

2. **Fuzzing integrado**:

	- Ejecuta pruebas con **entradas aleatorias** para detectar casos extremos y vulnerabilidades.
	    
	- Ejemplo (prueba con parámetros generados automáticamente):

	```Solidity
	function testFuzzTransferencia(uint256 amount) public {
	vm.assume(amount <= 1000); // Restricción para valores válidos
		token.transfer(alice, amount);
		assertEq(token.balanceOf(alice), amount);
	}
	```

3. **Cheatcodes**:
    
    - Funciones especiales (prefijo `vm.`) para manipular el entorno de prueba:
        
        - Cambiar el `msg.sender` (`vm.prank`).
            
        - Modificar el timestamp del bloque (`vm.warp`).
            
        - Simular transacciones desde direcciones específicas.
            
	```Solidity
	function testSoloOwner() public {
		vm.prank(owner); // Simula que el owner llama a la función
		contrato.restringida();
	}
	```

4. **Velocidad superior**:
    
    - Forge está escrito en **Rust**, lo que lo hace mucho más rápido que herramientas basadas en Node.js (ej: Hardhat).
        
    - Ejecuta miles de pruebas en segundos.

5. **Gas snapshots**:

	- Genera informes del **consumo de gas** de las funciones para optimizar costos.

6. **Scripting en Solidity**:

	- Automatiza despliegues y tareas con scripts escritos en Solidity.

	```Solidity
	contract DeployScript {
	    function run() external {
	        Token token = new Token();
	        vm.addToDeployment(address(token));
	    }
	}
	```


### **Utilidades de línea de comandos**

#### **1. `forge test`**

**Propósito**: Ejecutar pruebas unitarias y de integración escritas en Solidity.  
**Opciones útiles**:

- `-vvv`: Muestra trazas detalladas de transacciones (útil para debugging).
    
- `--match-test <nombre>`: Ejecuta solo pruebas que coincidan con un nombre (ej: `--match-test testTransfer`).
    
- `--gas-report`: Genera un informe del gas consumido por cada función.
    
- `--fork-url <URL>`: Ejecuta pruebas en un fork de una red (Mainnet, testnets).
    

**Ejemplo**:

bash

Copy

forge test -vvv --gas-report --fork-url https://eth-mainnet.g.alchemy.com/v2/TU_KEY

---

#### **2. `forge build`**

**Propósito**: Compila los contratos en la carpeta `src/` y genera los artifacts en `out/`.  
**Opciones**:

- `--extra-output`: Incluye metadatos adicionales (ABI, storage layout, etc.).
    
- `--sizes`: Muestra el tamaño de los contratos compilados.
    

**Ejemplo**:

bash

Copy

forge build --extra-output metadata

---

#### **3. `forge script`**

**Propósito**: Ejecutar scripts de despliegue o tareas automatizadas escritos en Solidity.  
**Opciones clave**:

- `--broadcast`: Envía transacciones reales a la red.
    
- `--verify`: Verifica el contrato en Etherscan después del despliegue.
    
- `--slow`: Simula transacciones sin enviarlas (`dry-run`).
    

**Ejemplo**:

bash

Copy

forge script script/Deploy.s.sol:Deploy --rpc-url mainnet --broadcast --verify

---

#### **4. `forge snapshot`**

**Propósito**: Genera o compara **informes de gas** para optimizar costos.  
**Uso**:

- `forge snapshot`: Crea un archivo `.gas-snapshot` con el consumo actual.
    
- `forge snapshot --check`: Compara el gas usado con el último snapshot.
    

**Ejemplo**:

bash

Copy

forge test --gas-report && forge snapshot

---

#### **5. `forge inspect`**

**Propósito**: Obtener metadatos de los contratos compilados (ABI, bytecode, storage layout).  
**Sintaxis**:

bash

Copy

forge inspect <NOMBRE_CONTRATO> <PROPIEDAD>

**Ejemplos**:

bash

Copy

forge inspect Token abi         # Muestra el ABI del contrato Token
forge inspect Token bytecode    # Muestra el bytecode

---

#### **6. `forge create`**

**Propósito**: Desplegar un contrato directamente desde la terminal.  
**Opciones**:

- `--constructor-args`: Pasar argumentos al constructor.
    
- `--value`: Enviar ETH durante el despliegue (si el constructor es `payable`).
    

**Ejemplo**:

bash

Copy

forge create src/Token.sol:Token --rpc-url mainnet --private-key $PK

---

#### **7. `forge verify-contract`**

**Propósito**: Verificar un contrato en **Etherscan** o **Blockscout**.  
**Parámetros clave**:

- `--chain`: ID de la red (ej: `1` para Mainnet, `5` para Goerli).
    
- `--compiler-version`: Versión de Solidity usada (ej: `v0.8.20`).
    

**Ejemplo**:

bash

Copy

forge verify-contract 0x... src/Token.sol:Token --chain 1 --etherscan-api-key $API_KEY

---

#### **8. `forge install` / `forge update`**

**Propósito**: Gestionar dependencias (ej: OpenZeppelin, Solmate).

- `forge install <repo>`: Instala una librería (ej: `forge install OpenZeppelin/openzeppelin-contracts`).
    
- `forge update`: Actualiza todas las dependencias.
    

---

#### **9. `forge coverage`**

**Propósito**: Generar un informe de **cobertura de pruebas** (qué líneas de código se ejecutan en las pruebas).  
**Ejemplo**:

bash

Copy

forge coverage --report lcov  # Genera un informe en formato LCOV

---

#### **10. `forge clean`**

**Propósito**: Eliminar carpetas `out/` y `cache/` para limpiar el proyecto.

bash

Copy

forge clean

---

#### **Bonus: `forge bind`**

**Propósito**: Generar **bindings** en otros lenguajes (Rust, TypeScript) para interactuar con contratos desde fuera de Solidity.

bash

Copy

forge bind --lang ts --out-dir ./frontend/src/types
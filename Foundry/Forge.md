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

### **[[Estructura de un proyecto]]**

Un proyecto típico de Foundry sigue esta estructura de carpetas y archivos:

```Bash
mi_proyecto/
├── foundry.toml       # Configuración de Foundry (compilador, remappings, etc.)
├── lib/               # Dependencias instaladas (ej: OpenZeppelin)
├── src/               # Contratos inteligentes (.sol)
├── test/              # Pruebas escritas en Solidity (.t.sol)
├── script/            # Scripts de despliegue y tareas (.s.sol)
├── out/               # Artifacts de compilación (generado automáticamente)
└── cache/
```
### **Ejemplo completo**

1. **Contrato**:
```Solidity
// src/Token.sol
contract Token {
    mapping(address => uint256) public balanceOf;
    
    constructor() {
        balanceOf[msg.sender] = 1000;
    }
    
    function transfer(address to, uint256 amount) public {
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
    }
}
```

2. **Prueba**

```Solidity
// test/Token.t.sol
import "forge-std/Test.sol";
import "../src/Token.sol"

contract TokenTest is Test {
    Token token;
    address alice = address(1);
    
    function setUp() public {
        token = new Token();
        token.transfer(alice, 100); // Estado inicial
    }
    
    function testBalanceDeAlice() public {
        assertEq(token.balanceOf(alice), 100);
    }
    
    function testFuzzTransferencia(uint256 amount) public {
        vm.assume(amount <= 100);
        vm.prank(alice);
        token.transfer(address(2), amount);
        assertEq(token.balanceOf(address(2)), amount);
    }
}
```

3. **Script de deploy**:

```Solidity
// script/DeployToken.s.sol
import "forge-std/Script.sol";
import "../src/Token.sol";

contract DeployToken is Script {
    function run() external {
        vm.startBroadcast(); // Inicia el contexto de transacción
        Token token = new Token();
        vm.stopBroadcast();
        
        // Opcional: Guardar la dirección del contrato desplegado
        string memory json = vm.serializeAddress("deployments", "Token", address(token));
        vm.writeJson(json, "./deployments.json");
    }
}
```
3. **Resultado de `forge test`**:

```Bash
[PASS] testBalanceDeAlice() (gas: 12345)
[PASS] testFuzzTransferencia(uint256) (runs: 256, μ: 54321, ~: 56789)
```
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
        
2. **[[Cast]]**:
    
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
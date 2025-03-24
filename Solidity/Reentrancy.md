# **¿Qué es un Ataque de Reentrancy?**

Un ataque de reentrancy ocurre cuando un contrato malicioso **llama repetidamente a una función** antes de que la ejecución inicial haya terminado. Esto permite al atacante drenar fondos o manipular el estado del contrato de forma no autorizada.

## **Ejemplo de Ataque (Código Vulnerable)**

```Solidity
// Contrato Vulnerable
contract Vulnerable {
    mapping(address => uint) public saldos;

    function retirar() public {
        uint saldo = saldos[msg.sender];
        // ❌ Interacción ANTES de actualizar el saldo
        (bool exito, ) = msg.sender.call{value: saldo}("");
        require(exito, "Fallo en el envio");
        saldos[msg.sender] = 0; // Actualización después de la interacción
    }
}
```

### **Cómo lo explota un atacante:**

1. **Paso 1**: El atacante deposita Ether en el contrato `Vulnerable`.
    
2. **Paso 2**: Llama a `retirar()` para retirar su saldo.
    
3. **Paso 3**: Cuando el contrato envía Ether al atacante, este tiene un **fallback function** malicioso que **vuelve a llamar a `retirar()`** antes de que `saldos[msg.sender]` se actualice a 0.
    
4. **Resultado**: El contrato envía Ether múltiples veces, aunque el saldo real solo era suficiente para una retirada.
    

---

## **Solución: Checks-Effects-Interactions (CEI)**

El patrón **Checks-Effects-Interactions** garantiza que el contrato actualice su estado interno **antes** de interactuar con contratos externos. Así se evita que un atacante manipule el estado durante una llamada recursiva.

### **Aplicación del Patrón CEI:**

1. **Checks** (Verificaciones): Valida las condiciones necesarias.
    
    ```Solidity
	uint saldo = saldos[msg.sender];
    require(saldo > 0, "Saldo insuficiente");
	```
    
2. **Effects** (Efectos): Actualiza el estado del contrato.
    
    ```Solidity
	saldos[msg.sender] = 0; // Actualiza el saldo ANTES de enviar Ether
	```
    
3. **Interactions** (Interacciones): Realiza llamadas externas.
    
    ```Solidity
	(bool exito, ) = msg.sender.call{value: saldo}("");
	require(exito, "Fallo en el envio");
	```
    

### **Código Seguro:**

```Solidity
contract Seguro {
    mapping(address => uint) public saldos;

    function retirar() public {
        // Checks
        uint saldo = saldos[msg.sender];
        require(saldo > 0, "Saldo insuficiente");

        // Effects (actualiza el estado ANTES de la interacción)
        saldos[msg.sender] = 0;

        // Interactions
        (bool exito, ) = msg.sender.call{value: saldo}("");
        require(exito, "Fallo en el envio");
    }
}
```

---

## **¿Por qué funciona esto?**

- Al actualizar el saldo **antes** de enviar el Ether, el contrato malicioso no puede retirar fondos adicionales, ya que su saldo ya es 0.
    
- Si el atacante intenta reingresar a `retirar()`, la verificación `require(saldo > 0)` fallará.
    

---

## **Ejemplo de Contrato Malicioso (Atacante)**

```Solidity
contract Atacante {
    Vulnerable contratoVulnerable;

    constructor(address _direccionVulnerable) {
        contratoVulnerable = Vulnerable(_direccionVulnerable);
    }

    // Función para depositar y activar el ataque
    function atacar() external payable {
        contratoVulnerable.depositar{value: msg.value}();
        contratoVulnerable.retirar();
    }

    // Fallback function que relanza retirar()
    receive() external payable {
        if (address(contratoVulnerable).balance >= 1 ether) {
            contratoVulnerable.retirar();
        }
    }
}
```

---

## **Consecuencias Reales**

El ataque de reentrancy más famoso fue el **hack de The DAO (2016)**, donde se robaron 3.6 millones de ETH (≈ $50 millones en ese momento). Esto llevó a un hard fork de Ethereum, creando Ethereum (ETH) y Ethereum Classic (ETC).

---

## **Buenas Prácticas**

1. **Siempre usa Checks-Effects-Interactions**:  
    Actualiza el estado antes de interactuar con contratos externos.
    
2. **Usa modificadores anti-reentrancy**:  
    Como `nonReentrant` de OpenZeppelin:
    
    ```Solidity
	import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
	    
	contract MiContrato is ReentrancyGuard {
		function retirar() public nonReentrant {
			// Lógica segura
		}
	}
	```
    
3. **Evita lógica compleja en `receive`/`fallback`**:  
    Limita lo que un contrato puede hacer al recibir Ether.
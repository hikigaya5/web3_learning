En Solidity, el **constructor** es una función especial que se ejecuta **una única vez** durante el despliegue del contrato en la blockchain. Su objetivo principal es inicializar el estado del contrato (variables de estado, configuraciones iniciales, permisos, etc.).

1. **Ejecución única**:  
    Se ejecuta automáticamente al desplegar el contrato y **no puede ser llamado después**.
    
2. **Sin nombre**:  
    Desde Solidity 0.7.0, se define con la palabra clave `constructor` (antes usaba el nombre del contrato).
    
3. **Visibilidad**:  
    Puede ser `public` (por defecto) o `internal` (si el contrato es abstracto).
    
4. **Parámetros**:  
    Acepta parámetros para personalizar la configuración inicial del contrato.
    
5. **Herencia**:  
    Si el contrato hereda de otros contratos, los constructores de los padres se ejecutan en orden de herencia.

#### **1. Inicializar Variables de Estado**

```Solidity
contract Token {
    string public name;
    uint256 public totalSupply;

    constructor(string memory _name, uint256 _supply) {
        name = _name;
        totalSupply = _supply;
    }
}
// Al desplegar: new Token("MiToken", 1000000);
```

#### **2. Establecer el Propietario del Contrato**
```Solidity
contract Ownable {
    address public owner;

    constructor() {
        owner = msg.sender; // El despliegador es el propietario
    }
}
```
#### **3. Configurar Contratos Hijo en Herencia**
```Solidity
contract Base {
    uint256 public valorBase;

    constructor(uint256 _valorBase) {
        valorBase = _valorBase;
    }
}

contract Hijo is Base {
    constructor(uint256 _valorHijo) Base(_valorHijo * 2) {
        // Inicializa Base con (_valorHijo * 2)
    }
}
```
#### **4. Contratos Abstractos (Constructores Internos)**
```Solidity
abstract contract Configuracion {
    address public admin;

    constructor(address _admin) internal {
        admin = _admin;
    }
}

contract MiContrato is Configuracion {
    constructor() Configuracion(msg.sender) {}
}
```
#### **5. Constructor `payable` (Recibir ETH al Desplegar)**
```Solidity
contract Vault {
    uint256 public balanceInicial;

    constructor() payable {
        balanceInicial = msg.value; // ETH enviado al desplegar
    }
}
// Desplegar con 1 ETH: new Vault{value: 1 ether}();
```
#### **6. Desplegar Otros Contratos**
```Solidity
contract Factory {
    address public contratoHijo;

    constructor() {
        contratoHijo = address(new ContratoHijo()); // Despliega otro contrato
    }
}

contract ContratoHijo {}
```

- **Orden de herencia**:  
    Los constructores de los contratos base se ejecutan en el orden en que se heredan.
    
    ```Solidity
	contract A { constructor() {} }
    contract B { constructor() {} }
    contract C is A, B { constructor() A() B() {} } // Ejecuta A → B → C
    contract C is B, A { constructor() A() B() {} } // Ejecuta B → A → C
	```
    
- **Parámetros obligatorios**:  
    Si un contrato base tiene un constructor con parámetros, el contrato hijo **debe inicializarlo explícitamente**.
    
    ```Solidity
	contract Base { constructor(uint a) {} }
    contract Hijo is Base { constructor() Base(100) {} } // ✅ Correcto
	```
    
- **Visibilidad `internal`**:  
    Si el constructor es `internal`, el contrato se marca como **abstracto** y no puede ser desplegado.
    
    ```Solidity
	abstract contract Abstracto { constructor() internal {} }
	```
### **Ejemplo de Caso Real: Token ERC-20**

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract MiToken is ERC20 {
    constructor(
        string memory nombre,
        string memory simbolo,
        uint256 suministroInicial
    ) ERC20(nombre, simbolo) {
        _mint(msg.sender, suministroInicial); // Inicializa el suministro
    }
}
```

## Ejemplo

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Librería para operaciones seguras (ejemplo clásico)
library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: suma con desbordamiento");
        return c;
    }
}

// Interfaz para operaciones básicas de un token
interface IERC20 {
    function transfer(address to, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

// Contrato abstracto que define la estructura base de un token
abstract contract TokenBase is IERC20 {
    using SafeMath for uint256; // Usamos la librería SafeMath

    string public name;
    string public symbol;
    uint256 public totalSupply;
    mapping(address => uint256) internal _balances;

    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;
    }

    // Función abstracta que DEBE ser implementada por contratos hijos
    function transfer(address to, uint256 amount) public virtual override returns (bool);

    // Función implementada (heredada de IERC20)
    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    // Función interna común para mintear tokens
    function _mint(address account, uint256 amount) internal {
        _balances[account] = _balances[account].add(amount); // Usa SafeMath
        totalSupply = totalSupply.add(amount);
    }
}

// Contrato hijo que implementa el token ERC-20 completo
contract MiToken is TokenBase("MiToken", "MTK") {
    using SafeMath for uint256;

    constructor(uint256 initialSupply) {
        _mint(msg.sender, initialSupply); // Llama a la función interna del abstracto
    }

    // Implementación de la función abstracta transfer
    function transfer(address to, uint256 amount) public override returns (bool) {
        require(_balances[msg.sender] >= amount, "Saldo insuficiente");
        _balances[msg.sender] = _balances[msg.sender].add(amount * -1); // Usa SafeMath
        _balances[to] = _balances[to].add(amount);
        return true;
    }

    // Función adicional para quemar tokens
    function burn(uint256 amount) public {
        require(_balances[msg.sender] >= amount, "Saldo insuficiente");
        _balances[msg.sender] = _balances[msg.sender].add(amount * -1);
        totalSupply = totalSupply.add(amount * -1);
    }
}
```

## **Herencia y Polimorfismo**

La herencia permite que un contrato herede propiedades y funciones de otro contrato, mientras que el polimorfismo permite sobrescribir funciones en contratos derivados. En Solidity, esto se logra con las palabras clave `virtual` (en la función base) y `override` (en la función derivada).

### **Ejemplo de Herencia y Polimorfismo**

```Solidity
// Contrato Base
contract Animal {
    function hacerSonido() public pure virtual returns (string memory) {
        return "Sonido genérico";
    }
}

// Contrato Derivado
contract Perro is Animal {
    function hacerSonido() public pure override returns (string memory) {
        return "Guau!";
    }
}

// Contrato Derivado con Herencia Múltiple
contract Gato is Animal {
    function hacerSonido() public pure override returns (string memory) {
        return "Miau!";
    }
}
```

### **Características Clave**

- **`virtual`**: Marca una función como modificable en contratos derivados.
    
- **`override`**: Indica que la función sobrescribe una función `virtual` del contrato base.
    
- **Herencia Múltiple**: Un contrato puede heredar de varios contratos base separados por comas:  
    `contract MiContrato is ContratoA, ContratoB { ... }`.
    

### **Casos de Uso**

- **Reutilizar lógica común**: Ejemplo: Contratos base para tokens (ERC20, ERC721).
    
- **Personalizar funciones**: Adaptar funciones para casos específicos (ej: tokens con impuestos).
    

---

## **Interfaces**

Las interfaces definen funciones que otros contratos deben implementar, sin incluir lógica. Son útiles para estandarizar interacciones entre contratos.

### **Ejemplo de Interfaz**

```Solidity
// Interfaz para un token ERC-20 mínimo
interface IERC20 {
    function transfer(address destino, uint256 monto) external returns (bool);
    function balanceOf(address cuenta) external view returns (uint256);
}

// Contrato que implementa la interfaz
contract MiToken is IERC20 {
    mapping(address => uint256) private _balances;

    function transfer(address destino, uint256 monto) external override returns (bool) {
        require(_balances[msg.sender] >= monto, "Saldo insuficiente");
        _balances[msg.sender] -= monto;
        _balances[destino] += monto;
        return true;
    }

    function balanceOf(address cuenta) external view override returns (uint256) {
        return _balances[cuenta];
    }
}
```

### **Reglas de las Interfaces**

- **Funciones externas**: Todas las funciones deben ser `external`.
    
- **Sin implementación**: No incluyen código en las funciones.
    
- **Sin variables de estado**: No pueden declarar variables de estado ni constructores.
    

### **Casos de Uso**

- **Integrar protocolos externos**: Como Uniswap, Aave, o Chainlink.
    
- **Estándares de tokens**: ERC-20, ERC-721, ERC-1155.
    

---
## **Abstracción**

Un contrato abstracto es un contrato que contiene al menos una función sin implementación (una función `virtual` sin cuerpo) y no puede ser desplegado directamente en la blockchain. Su propósito es servir como **plantilla base** para otros contratos que hereden de él y proporcionar una estructura común que los contratos derivados deben implementar.


### **Ejemplo de un contrato abstracto**

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

abstract contract Animal {
    string public especie; // Variable de estado
    uint256 public edad;

    // Función abstracta (sin implementación)
    function hacerSonido() public virtual returns (string memory);

    // Función implementada
    function envejecer() public {
        edad += 1;
    }
}

// Contrato hijo que implementa la función abstracta
contract Perro is Animal {
    constructor() {
        especie = "Canino";
        edad = 0;
    }

    function hacerSonido() public pure override returns (string memory) {
        return "Guau!";
    }
}
```

### **Características de un clave**

- **Funciones sin implementar**:
    
    - Declaradas con `function nombre(...) virtual;` (sin cuerpo `{}`).
        
    - Deben ser implementadas (`override`) en contratos hijos.
        
- **No se puede desplegar**:
    
    - Si un contrato tiene funciones abstractas, debe marcarse con `abstract contract`.
        
- **Puede tener funciones implementadas**:
    
    - A diferencia de las interfaces, un contrato abstracto puede incluir funciones con lógica.
        
- **Puede tener variables de estado**:
    
    - Las interfaces no permiten variables de estado, pero los contratos abstractos sí.

### **Casos de Uso**

- **Definir estándares parciales**:  
    Ejemplo: Un contrato abstracto para tokens ERC-20 que implementa `balanceOf` pero deja `transfer` como abstracto.
    
- **Reutilizar lógica común**:  
    Funciones compartidas entre múltiples contratos (ej: manejo de permisos).
    
- **Forzar implementaciones específicas**:  
    Garantizar que los contratos hijos definan ciertas funciones.


---
## **Librerías**

Las librerías son contratos sin estado que agrupan funciones reutilizables. Pueden ser adjuntadas a tipos de datos para extender su funcionalidad.

### **Ejemplo de Librería**

```Solidity
library Math {
    // Función para calcular el máximo entre dos números
    function max(uint256 a, uint256 b) internal pure returns (uint256) {
        return a > b ? a : b;
    }

    // Función para calcular la suma de un array
    function sumar(uint256[] memory arr) internal pure returns (uint256) {
        uint256 total = 0;
        for (uint256 i = 0; i < arr.length; i++) {
            total += arr[i];
        }
        return total;
    }
}


// Uso de la librería en un contrato
contract Calculadora {
    using Math for uint256; // Extiende el tipo uint256 con las funciones de Math
    using Math for uint256[]; // Extiende arrays de uint256

    function calcularMaximo(uint256 a, uint256 b) public pure returns (uint256) {
        return a.max(b); // Llama a Math.max(a, b)
    }

    function sumarArray(uint256[] memory arr) public pure returns (uint256) {
        return arr.sumar(); // Llama a Math.sumar(arr)
    }
}
```

### **Características Clave**

- **Sin estado**: No almacenan datos (no tienen variables de estado).
    
- **Funciones internas**: Las funciones suelen ser `internal` para ahorrar gas.
    
- **Extensión de tipos**: Se pueden adjuntar a tipos como `uint256` o `address` usando `using ... for ...`
    

### **Casos de Uso**

- **Operaciones matemáticas**: Cálculos seguros (ej: multiplicación sin overflow).
    
- **Manipulación de datos**: Funciones para strings, arrays, o estructuras complejas.
    
- **Reutilización de lógica**: Funciones comunes a múltiples contratos.
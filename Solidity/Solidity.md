
**Solidity** es un **lenguaje de programación de alto nivel** diseñado específicamente para escribir **smart contracts** en blockchains compatibles con la **Ethereum Virtual Machine (EVM)**, como Ethereum, Polygon, Binance Smart Chain o Avalanche. Su objetivo principal es permitir la creación de **aplicaciones descentralizadas** que ejecutan lógica automatizada y transparente sin intermediarios. Aquí te lo explico de manera sencilla y estructurada.

**Solidity es un lenguaje compilado**, no interpretado. Esto significa que el código que escribes en Solidity no se ejecuta directamente en la blockchain, sino que primero se compila en **bytecode** (código de bajo nivel) que la **Ethereum Virtual Machine (EVM)** puede entender y ejecutar.

# **Estructura de un Smart Contract**

A continuación voy a dejar el ejemplo de un **smart contract** sobre el que voy a ir trabajando la estructura y la sintaxis de estos. Llamémoslo **Example.sol**:

```Solidity
// 1. Licencia
// SPDX-License-Identifier: MIT

// 2. Versión del compilador
pragma solidity ^0.8.0;

// 3. Importación de librerías (opcional)
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

// 4. Definición del contrato
contract Example {

    // 5. Variables de estado
    string public mensaje;
    uint256 public contador;

    // 6. Eventos
    event MensajeActualizado(string nuevoMensaje);

    // 7. Constructor
    constructor(string memory _mensajeInicial) {
        mensaje = _mensajeInicial;
        contador = 0;
    }

    // 8. Funciones
    function actualizarMensaje(string memory _nuevoMensaje) public {
        require(bytes(_nuevoMensaje).length > 0, "El mensaje no puede estar vacío");
        mensaje = _nuevoMensaje;
        contador += 1;
        emit MensajeActualizado(_nuevoMensaje);
    }

    // 9. Funciones de solo lectura (view/pure)
    function obtenerContador() public view returns (uint256) {
        return contador;
    }
}
```


## 1. Licencia

```Solidity
// SPDX-License-Identifier: MIT
```

- **Qué es**: Especifica la licencia bajo la cual se distribuye el contrato.
    
- **Por qué es importante**: Ayuda a definir los derechos de uso del código.
    
- **Ejemplo común**: `MIT`, `GPL-3.0`, o `Unlicense`.

---
## 2. Versión del compilador

```Solidity
pragma solidity ^0.8.0;
```

- **Qué es**: Define la versión del compilador de Solidity que debe usarse.
    
- **Por qué es importante**: Asegura que el contrato se compile correctamente y evita problemas de compatibilidad.
    
- **Sintaxis**:
    
    - `^0.8.0`: Usa cualquier versión de la 0.8.0 en adelante, pero menor a 0.9.0.
        
    - `>=0.8.0 <0.9.0`: Define un rango específico.

---
## [[Importación de librerías| 3. Importación de librerías]]

```Solidity
import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
```

- **Qué es**: Permite usar código de otros contratos o librerías.
    
- **Por qué es importante**: Facilita la reutilización de código y el uso de estándares (como ERC-20 para tokens).
    
- **Ejemplo**: OpenZeppelin es una librería popular para contratos seguros.

---
## [[Definición del contrato|4. Definición del contrato]]

```Solidity
contract Example {}
```

- **Qué es**: Define el inicio del contrato.
    
- **Por qué es importante**: Todo el código del contrato va dentro de este bloque.
    
- **Sintaxis**: `contract NombreDelContrato { ... }`.

---
## [[Variables|5. Variables de estado]]

```Solidity
string public mensaje;
uint256 public contador;
```

- **Qué es**: Variables que almacenan datos en la blockchain.
    
- **Por qué es importante**: Mantienen el estado del contrato entre transacciones.
    
- **[[Visibilidad]]**:
    
    - `public`: Accesible desde fuera del contrato (genera un getter automático).
        
    - `private`: Solo accesible dentro del contrato.
        
    - `internal`: Accesible dentro del contrato y contratos derivados.
        

---
## 6. Eventos

```Solidity
event MensajeActualizado(string nuevoMensaje);
```

- **Qué es**: Registros inmutables que se emiten durante la ejecución del contrato.
    
- **Por qué es importante**: Permiten a las aplicaciones externas (como frontends) reaccionar a cambios en el contrato.
    
- **Uso**: Se emiten con `emit NombreDelEvento(arg1, arg2);`.
    

---

## 7. Constructor

```Solidity
constructor(string memory _mensajeInicial) {
    mensaje = _mensajeInicial;
    contador = 0;
}
```

- **Qué es**: Función especial que se ejecuta una sola vez al desplegar el contrato.
    
- **Por qué es importante**: Inicializa el estado del contrato.
    
- **Parámetros**: Puede recibir argumentos para configurar el contrato.
    

---

## 8. Funciones

```Solidity
function actualizarMensaje(string memory _nuevoMensaje) public {
    require(bytes(_nuevoMensaje).length > 0, "El mensaje no puede estar vacío");
    mensaje = _nuevoMensaje;
    contador += 1;
    emit MensajeActualizado(_nuevoMensaje);
}
```

- **Qué es**: Bloques de código que ejecutan lógica.
    
- **Por qué es importante**: Permiten interactuar con el contrato.
    
- **Visibilidad**:
    
    - `public`: Accesible desde fuera del contrato.
        
    - `private`: Solo accesible dentro del contrato.
        
    - `external`: Solo accesible desde fuera del contrato.
        
    - `internal`: Accesible dentro del contrato y contratos derivados.
        
- **Modificadores**:
    
    - `require(condición, "mensaje")`: Revisa una condición y revierte la transacción si no se cumple.
        
    - `emit`: Emite un evento.
        

---

## 9. Funciones de solo lectura (view/pure)

```Solidity
function obtenerContador() public view returns (uint256) {
    return contador;
}
```

- **Qué es**: Funciones que no modifican el estado de la blockchain.
    
- **Por qué es importante**: No consumen gas cuando se llaman desde fuera de la blockchain.
    
- **Tipos**:
    
    - `view`: Lee datos del estado del contrato.
        
    - `pure`: No lee ni escribe datos (solo realiza cálculos).
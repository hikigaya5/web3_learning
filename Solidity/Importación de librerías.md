### **Importar un Archivo Local**

Usa una ruta relativa o absoluta para incluir archivos de tu proyecto.

```Solidity
// Importar desde una carpeta local
import "./utils/Helpers.sol";                  // Ruta relativa
import "/proyecto/contracts/Tokens/ERC20.sol"; // Ruta absoluta (poco común)
```
### **Importar con Alias**

Asigna un nombre alternativo al contrato/libro importado para evitar conflictos.


```Solidity
import {ERC20 as TokenStandard} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
```

### **Importar desde Dependencias (npm/Node Modules)**

Si usas herramientas como **Hardhat** o **Truffle**, puedes importar desde `node_modules`.

```Solidity
// Importar desde OpenZeppelin (instalado via npm)
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
```

### **Importar desde una URL**

Algunos entornos (como Remix) permiten importar directamente desde GitHub u otros repositorios.

```Solidity
// Importar desde GitHub (Raw Content)
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol";
```

### **Importar Contenido Global**

Importa todos los contratos de un archivo (no recomendado por posibles conflictos).

```Solidity
import * as Helpers from "./utils/Helpers.sol";
// Uso: Helpers.ContratoX
```
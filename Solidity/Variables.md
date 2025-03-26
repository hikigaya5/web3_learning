```Solidity
string public mensaje;
uint256 public contador;
```
### **Tipos básicos**

- **Enteros**:
    
    - `int`: Entero con signo (puede ser positivo o negativo).  
        
    - `uint`: Entero sin signo (solo positivo).
    - El default value es `0`.  
        
    - **Números al final**: Indican el tamaño en bits (ej: `uint256` usa 256 bits, `uint8` usa 8 bits).
    - `int` y `uint` son alias de `int256` y `uint256`  
        
    - Ejemplo: `uint256 public contador;` (almacena números de 0 a 2²⁵⁶ - 1).
- **Bytes**:
    
    - `bytes`: Secuencia de bytes de tamaño dinámico.  
        
    - `bytes1` a `bytes32`: Secuencias de bytes de tamaño fijo.
    - El default value es un array vacío para `bytes` y para `bytes1` a `bytes32` será el default value del tipo de elemento del array.
    - Ejemplo: `bytes32 public hash;`  
        
    - Puede almacenar strings.
- **Booleanos**:
    
    - `bool`: Almacena `true` o `false`.  
        
    - El default value es `false`.  
        
    - Ejemplo: `bool public activado = true;`.
- **Direcciones**:
    
    - `address`: Almacena una dirección de Ethereum (20 bytes).  
        
    - `address payable`: Similar a `address`, pero permite recibir ETH.
    - El default value es un `byte20` todo a 0.
        - Ejemplo: `address public propietario;`.
- **Strings**:
    
    - `string`: Almacena texto (UTF-8).
    - El default value es una cadena vacía.
        - Ejemplo: `string public mensaje = "Hola";`.  
            

### **Tipos compuestos**

- **Arrays**:
    
    - `uint[]`: Array dinámico de enteros.  
        
    - `uint[5]`: Array fijo de 5 enteros.
    - El default value es lo mismo que con `bytes` y `bytes1` a `bytes32`.
	- Ejemplo: 
		```Solidity
		uint[] public numeros;
		```  
            
- **Structs**:
    
    - Permiten definir tipos personalizados.
    - El default value de un `struct` son los default value del tipo de variable de los valores de la `struct`.  
        
    - Ejemplo:
        
        ```solidity
        struct Usuario { 
        	string nombre; 
        	uint edad; 
        } 
        Usuario public usuario;
        ```
        
- **Mappings**:
    
    - Son como diccionarios o tablas hash.
    - Los maps son inicializados virtualmente para que cada posible clave exista y esté mapeada a un valor que sea el default value del tipo de elemento del valor.
	- Ejemplo:
		```Solidity
		mapping(address => uint) public balances;
		```  
            

### **Tipos especiales**

- **Enums**:
    
    - Lista de valores predefinidos.
    - El default value es el primer elemento en la definición.  
        
    - Ejemplo:
        
        ```solidity
        enum Estado { Inactivo, Activo, Pausado }
        Estado public estado = Estado.Activo
        ```
        
- **Constantes e inmutables**:
    
    - `constant`: Valor fijo que no puede cambiar (se define en tiempo de compilación).  
        
    - `immutable`: Valor que se define una vez en el constructor y no cambia después.  
        
    - Ejemplo:
        
        ```solidity
        uint256 public constant MAX_SUPPLY = 1000;
        address public immutable propietario;
        
        constructor(address _propietario) {
            propietario = _propietario;
        }
        ```

### **[[Visibilidad]]**:

- `public`: Accesible desde fuera del contrato (genera un getter automático). 
- `private`: Solo accesible dentro del contrato.  
- `internal`: Accesible dentro del contrato y contratos derivados.

### **Notaciones para los nombres**

#### 1. Camel Case

- **Uso común**: Para nombres de variables y funciones.  
    
- **Ejemplo**: `miVariable`, `contadorDeTransacciones`.  
    

#### 2. Pascal Case

- **Uso común**: Para nombres de contratos y structs.  
    
- **Ejemplo**: `MiContrato`, `Usuario`.  
    

#### 3. Snake Case

- **Uso común**: Menos frecuente en Solidity, pero puede usarse para constantes.  
    
- **Ejemplo**: `MAX_SUPPLY`, `MIN_BALANCE`.  
    

#### 4. Prefijos y sufijos comunes

- **`_` al inicio**: Para variables privadas o internas (ej: `_saldoPrivado`) o parámetros en la definición de una función.
	- Ejemplo:
		```Solidity
		uint256 private _saldoPrivado;
		string public cadenaDePrueba;

		function store(string _cadenaDePrueba) public {
			cadenaDePrueba = _cadenaDePrueba;
		}
		```
    
- **`public`**: Para variables accesibles desde fuera del contrato (genera un getter automático).  
    
- **`constant`/`immutable`**: Para valores fijos o inmutables (ej: `MAX_SUPPLY`).


## **Variables globales**

Existen variables y funciones especiales que siempre están presentes en el espacio de nombres global y se utilizan principalmente para proporcionar información sobre la blockchain o como funciones de utilidad general.

### **Propiedades del Bloque y la Transacción**

- **`blockhash(uint blockNumber)`** → `bytes32`:  
    Devuelve el hash del bloque dado cuando `blocknumber` es uno de los 256 bloques más recientes; de lo contrario, devuelve cero.
    
- **`blobhash(uint index)`** → `bytes32`:  
    Devuelve el hash versionado del _index-ésimo_ blob asociado a la transacción actual. Un hash versionado consiste en un byte que representa la versión (actualmente `0x01`), seguido de los últimos 31 bytes del hash SHA256 del compromiso KZG (EIP-4844). Devuelve cero si no existe un blob con el índice dado.
    
- **`block.basefee`** (`uint`):  
    Tarifa base del bloque actual (EIP-3198 y EIP-1559).
    
- **`block.blobbasefee`** (`uint`):  
    Tarifa base de blobs del bloque actual (EIP-7516 y EIP-4844).
    
- **`block.chainid`** (`uint`):  
    ID de la cadena actual.
    
- **`block.coinbase`** (`address payable`):  
    Dirección del minero del bloque actual.
    
- **`block.difficulty`** (`uint`):  
    Dificultad del bloque actual (EVM < París). Para otras versiones de EVM, actúa como un alias obsoleto para `block.prevrandao` (EIP-4399).
    
- **`block.gaslimit`** (`uint`):  
    Límite de gas del bloque actual.
    
- **`block.number`** (`uint`):  
    Número del bloque actual.
    
- **`block.prevrandao`** (`uint`):  
    Número aleatorio proporcionado por la cadena de beacon (EVM >= París).
    
- **`block.timestamp`** (`uint`):  
    Marca de tiempo del bloque actual en segundos desde el epoch de Unix.
    

### **Funciones de Utilidad**

- **`gasleft()`** → `uint256`:  
    Devuelve el gas restante.
    

### **Propiedades del Mensaje (`msg`)**

- **`msg.data`** (`bytes calldata`):  
    Calldata completo de la llamada.
    
- **`msg.sender`** (`address`):  
    Remitente del mensaje (llamada actual).
    
- **`msg.sig`** (`bytes4`):  
    Primeros cuatro bytes del calldata (es decir, identificador de función).
    
- **`msg.value`** (`uint`):  
    Cantidad de wei enviados con el mensaje.
    

### **Propiedades de la Transacción (`tx`)**

- **`tx.gasprice`** (`uint`):  
    Precio del gas de la transacción.
    
- **`tx.origin`** (`address`):  
    Remitente original de la transacción (cadena completa de llamadas).
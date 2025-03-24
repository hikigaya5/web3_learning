La **EVM (Ethereum Virtual Machine)** guarda información en diferentes ubicaciones de almacenamiento, cada una con características específicas en términos de **persistencia**, **costo de gas** y **ámbito de uso**.

### **Storage (Almacenamiento Persistente)**

- **Qué es**:
    
    - Es el almacenamiento **persistente** del contrato, guardado directamente en la blockchain.
	    
	- Los datos aquí almacenados **sobreviven entre transacciones** y son accesibles durante toda la vida del contrato.
        
	- Equivale a un "disco duro" en la blockchain.
        
- **Características**:
    
    - **Costoso en gas**: Modificar datos aquí consume mucho gas:
		
	    - **Lectura**: Moderado (≈ 200-800 gas por operación).
		- **Escritura**: Muy alto (≈ 20,000-100,000 gas).
        
    - **Persistente**: Los datos permanecen para siempre.
        
    - **Acceso**: Variables de estado declaradas en el contrato.
	    
	- **Mutabilidad**: Los datos pueden leerse y modificarse.
        
- **Ejemplo**:
    
    ```Solidity
	contract EjemploStorage {
	    uint256 public saldo; // Variable en storage
	
	    function actualizarSaldo(uint256 nuevoSaldo) public {
	        saldo = nuevoSaldo; // Modifica storage (costoso en gas)
	    }
	}
	```
    

---

### **Memory (Memoria Temporal)**

- **Qué es**:
    
    - Es una zona de memoria **temporal** que solo existe durante la ejecución de una función.
        
    - Equivale a la "RAM" del contrato.
        
- **Características**:
	    
    - **Económico en gas**: Leer/escribir aquí es más barato que en storage:
	    
	    - **Lectura/Escritura**: Muy bajo (≈ 3-10 gas por operación).
		
	- **No persistente**: Los datos se borran al finalizar la función. 
		
	- **Acceso**: Parámetros de funciones, variables locales y estructuras temporales.
		
	- **Mutabilidad**: Los datos pueden leerse y modificarse.
        
- **Ejemplo**:
    
    ```Solidity
	function sumar(uint256 a, uint256 b) public pure returns (uint256) {
		uint256 resultado = a + b; // Variable temporal en memory
		return resultado;
	}
	```
    

---

### **Calldata (Datos de Llamada)**

- **Qué es**:
    
    - Una zona de memoria **solo de lectura** que almacena los parámetros de una llamada externa.
        
    - Es similar a `memory`, pero no permite modificaciones.
        
- **Características**:
    
    - **Muy económico**: Ideal para parámetros de funciones externas:
	    
		- **Lectura**: Mínimo (≈ 3-10 gas por operación).
		- **Escritura**: No permitida (solo lectura).
	    
	- **No persistente**: Existe solo durante la ejecución de la función. 
		
	- **Mutabilidad**: Inmutable (no se puede modificar).
        
- **Ejemplo**:
    
    ```Solidity
	function procesarDatos(bytes calldata datos) external {
		// "datos" se lee desde calldata (no se puede modificar)
	}
	```
    

---

### **Stack (Pila de Ejecución)**

- **Qué es**:
    
    - Una estructura **LIFO (Last-In-First-Out)** donde la EVM almacena valores temporales durante la ejecución de operaciones.
        
    - Se usa para cálculos intermedios.
        
- **Características**:
    
    - **Máximo 1024 elementos**: Cada "slot" es de 32 bytes.
        
    - **Volátil**: Los datos se pierden al finalizar la función.
        
    - **Acceso rápido**: Optimizado para operaciones aritméticas o lógicas.
        
- **Ejemplo**:
    
    ```Solidity
	function calcular() public pure returns (uint256) {
        uint256 a = 5; // Se almacena en el stack
        uint256 b = 10;
        return a + b; // Operación realizada en el stack
    }
	```
    

---

### **Transient Storage (Almacenamiento Transitorio - EIP-1153)**

- **Qué es**:
    
    - Una nueva ubicación introducida en Ethereum (EIP-1153) para datos **temporales que persisten durante una transacción**.
        
    - Similar al `storage`, pero se borra al finalizar la transacción.
        
- **Características**:
    
    - **Más barato que storage**: No se guarda en la blockchain a largo plazo.
        
    - **Persistencia transaccional**: Útil para datos que necesitan sobrevivir entre llamadas internas.
        
- **Ejemplo**:
    
    ```Solidity
	// Uso de transient storage (sintaxis hipotética)
    transient uint256 temporalVar;
    
    function ejecutar() public {
        temporalVar = 100; // Solo existe durante la transacción
    }
	```
    

---

### **Code (Código del Contrato)**

- **Qué es**:
    
    - El **bytecode compilado** del contrato, almacenado en la blockchain.
        
- **Características**:
    
    - **Inmutable**: No se puede modificar después del despliegue (a menos que se use un proxy).
        
    - **Acceso**: Solo para ejecución de funciones.
        

---

### **Logs (Registros de Eventos)**

- **Qué es**:
    
    - Datos emitidos a través de **eventos**, almacenados fuera de la blockchain pero accesibles en los recibos de transacciones.
        
- **Características**:
    
    - **Económico**: Emitir eventos es más barato que escribir en storage.
        
    - **Indexable**: Permite filtrar eventos por parámetros indexados.
        
- **Ejemplo**:

    ```Solidity
	event Transfer(address indexed from, address indexed to, uint256 value);
    
    function transferir(address to, uint256 monto) public {
        emit Transfer(msg.sender, to, monto); // Datos guardados en logs
    }
	```

---

## **Reglas Clave y Errores Comunes**

###  **Tipos Simples vs Complejos**:

- Tipos simples (`uint`, `bool`, `address`) no requieren especificar `memory`/`storage`.
    
- Tipos complejos (`array`, `struct`, `bytes`, `string`) **sí deben especificar su ubicación**.

### **Asignaciones Peligrosas**:
    
- Asignar una variable `storage` a otra crea una **referencia** (no una copia)

	```Solidity
	struct Usuario {
		uint saldo;
	}
	Usuario storage usuario1 = usuarios[0];
	Usuario storage usuario2 = usuario1; // ¡Ambas apuntan al mismo dato!
	usuario2.saldo = 100; // Modifica usuario1.saldo
	```
	
### **Calldata vs Memory en Funciones**:
    
- En funciones `external`, usa `calldata` para parámetros complejos (ahorra gas).
	```Solidity
	// ✅ Correcto: parámetro en calldata
	function procesarDatos(bytes calldata datos) external {
	    // ...
	}
	
	// ❌ Error: falta especificar calldata/memory
	function procesarDatos(bytes datos) external {
	    // ...
	}
	```
	
- En funciones `public`, el compilador maneja automáticamente `memory` o `calldata` (usa `memory` directamente y el compilador hace conversión implícita en función de si la llamada es interna o externa).
        
### **Modificar Calldata**:
    
- Error común: Intentar modificar un parámetro `calldata`.
	
	```Solidity
	function procesar(bytes calldata datos) external {
		datos[0] = 0x00; // ❌ Error: calldata es inmutable
	}
	```
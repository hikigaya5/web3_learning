**Anvil** es una herramienta del ecosistema **Foundry** que funciona como un **nodo Ethereum local** para desarrollo y pruebas. Es similar a **Ganache** o **Hardhat Network**, pero diseñado para integrarse nativamente con las herramientas de Foundry (como **Forge** y **Cast**). Su objetivo es facilitar el desarrollo de contratos inteligentes al proporcionar un entorno de blockchain local rápido y configurable.

### **Características principales**

1. **Entorno de desarrollo local**:
    
    - Permite ejecutar una red Ethereum en tu máquina para probar contratos sin conexión a redes reales (como Mainnet o testnets).
        
    - Ideal para **pruebas rápidas**, **debugging** y **simulación de transacciones**.
        
2. **Cuentas preconfiguradas con fondos**:
    
    - Genera automáticamente **10 cuentas de prueba** con 10,000 ETH cada una (balance configurable).
        
    - Las claves privadas son **determinísticas** (predecibles), lo que facilita las pruebas.
        
3. **Minado instantáneo de bloques**:
    
    - Los bloques se minan **automáticamente** cuando hay transacciones pendientes.
        
    - También permite minar bloques manualmente con un intervalo específico (ej: 1 bloque por segundo).
        
4. **Forking de redes existentes**:
    
    - Puedes clonar ("fork") el estado de una red como Ethereum Mainnet o una testnet (ej: Sepolia) para probar contratos en un entorno realista.
	    ```Bash
	    anvil --fork-url https://eth-mainnet.alchemyapi.io/v2/tu_api_key
		```

5. **Cheatcodes integrados**:
    
    - Soporta funciones especiales (como las de **Forge**) para manipular el entorno:
        
        - Cambiar el timestamp del bloque.
            
        - Modificar el balance de una cuenta.
            
        - Simular llamadas desde direcciones específicas.
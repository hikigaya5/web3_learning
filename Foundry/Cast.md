**Cast** es la herramienta de **línea de comandos (CLI)** del ecosistema **Foundry**, diseñada para interactuar con la red Ethereum y contratos inteligentes de manera rápida y eficiente. Permite enviar transacciones, consultar datos de la blockchain, manipular ABIs y realizar operaciones avanzadas sin necesidad de escribir código. Es ideal para desarrolladores que prefieren trabajar desde la terminal o integrar comandos en scripts de automatización.

### **Características Principales**

1. **Interacción con contratos**:
    
    - Llamar a funciones (`view`/`pure`) y enviar transacciones (`cast send` y `cast call`).
        
    - Decodificar datos de transacciones, eventos o errores.
        
    - Consultar balances, nonces, storage slots, y más.
        
2. **Conversión de formatos**:
    
    - Convertir entre representaciones de datos (hexadecimal, decimal, strings).
        
    - Calcular hashes (keccak, addresses de contratos, selectores de funciones).
        
    - Formatear unidades (wei ⇨ ether, timestamps ⇨ fechas).
        
3. **Integración con redes**:
    
    - Funciona con cualquier red Ethereum (local como **Anvil**, testnets o mainnet).
        
    - Soporta múltiples RPC endpoints (Infura, Alchemy, nodos locales).
        
4. **Seguridad y flexibilidad**:
    
    - Gestión de claves privadas (usando `--private-key` o variables de entorno).
        
    - Simulación de transacciones (`--dry-run`) antes de enviarlas.

### **Comandos Clave**

#### **1. Consultas básicas de la blockchain**

- **Obtener el balance de una dirección**:

```Bash
cast balance <DIRECCIÓN> --rpc-url <URL_RPC>
```

- **Ver el nonce de una cuenta**:

```Bash
cast nonce <DIRECCIÓN>
```

#### **2. Interactuar con contratos**

- **Llamar a una función `view`**:
    
```Bash
cast call <DIRECCIÓN_CONTRATO> "nombreFuncion(tipoParam1,tipoParam2)(tipoRetorno)" <ARG1> <ARG2> --rpc-url <URL_RPC>
```
    
Ejemplo:
    
```Bash
cast call 0x... "balanceOf(address)(uint256)" 0x... --rpc-url http://localhost:8545
```

- **Enviar una transacción**:
    
```Bash
cast send <DIRECCIÓN_CONTRATO> "funcion(address,uint256)" <ARG1> <ARG2> \
  --private-key <CLAVE_PRIVADA> \
  --rpc-url <URL_RPC>
```

#### **3. Codificar/Decodificar datos**

- **Calcular el selector de una función**:

```Bash
cast sig "transfer(address,uint256)"
# Output: 0xa9059cbb
```

- **Decodificar un calldata**:

```Bash
cast 4byte-decode <CALLDATA>
```

- **Decodificar un evento**:

```Bash
cast pretty-calldata <DATA> --input <ABI_EVENTO>
```

#### **4. Operaciones con ABIs**

- **Generar el ABI de un contrato**:

```Bash
cast abi <ARCHIVO_SOL> <NOMBRE_CONTRATO>
```

- **Codificar parámetros para una función**:

```Bash
cast abi-encode "funcion(tipo1,tipo2)" <ARG1> <ARG2>
```

#### **5. Gestión de transacciones**

- **Simular una transacción (dry-run)**:

```Bash
# Calcular la dirección del contrato antes de desplegarlo
cast create2 --starts-with 0xdead --deployer <DIRECCIÓN_DEPLOYER>
```

- **Obtener detalles de una transacción**:

```Bash
cast tx <HASH_TX> --rpc-url <URL_RPC>
```


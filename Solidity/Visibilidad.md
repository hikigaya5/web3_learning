La **visibilidad** en Solidity determina desde dónde se puede acceder a una **variable de estado** o **función**. Es crucial para controlar el acceso a los datos y la lógica de tu contrato.

## **Visibilidad default**

### **Variables de Estado**

- **Visibilidad predeterminada**: **`internal`** (accesible dentro del contrato y contratos heredados).
    
- **Ejemplo**:
    
    ```Solidity
	uint256 miVariable; // Equivale a `uint256 internal miVariable`    
	```
### **Funciones**

- **Visibilidad default**: **No hay valor predeterminado**.
    
    - A partir de **Solidity 0.5.0**, es **obligatorio especificar explícitamente** la visibilidad de las funciones.
        
    - Si no se especifica, el compilador arrojará un error.

## **`public`**

- **Variables**:
    
    - **Acceso**: Desde cualquier lugar (dentro del contrato, contratos heredados y externamente).
        
    - **Getter automático**: El compilador genera una función para leer su valor.
        
    - **Ejemplo**:
        ```Solidity
        uint256 public contador; // Se puede leer con contador()
        ```
        
- **Funciones**:
    
    - **Acceso**: Desde cualquier lugar.
        
    - **Ejemplo**:
        ```Solidity
        function incrementar() public {
            contador += 1;
        }
        ```


## **`private`**

- **Variables**:
    
    - **Acceso**: Solo dentro del contrato donde se declaran (no en contratos heredados).
        
    - **Uso común**: Para datos sensibles que no deben exponerse.
        
    - **Ejemplo**:
        
        ```Solidity
        uint256 private saldoSecreto;
        ```
        
- **Funciones**:
    
    - **Acceso**: Solo dentro del contrato donde se declaran.
        
    - **Ejemplo**:
        ```Solidity
        function _calcularSaldo() private view returns (uint256) {
            return saldoSecreto * 2;
        }
        ```


## **`internal`**

- **Variables**:
    
    - **Acceso**: Dentro del contrato y contratos heredados (pero no externamente).
        
    - **Ejemplo**:
        ```Solidity
        uint256 internal saldoInterno;
        ```
        
- **Funciones**:
    
    - **Acceso**: Igual que las variables.
        
    - **Ejemplo**:
		```Solidity
        function _actualizarSaldo() internal {
            saldoInterno += 10;
        }
        ```


## **`external`** (solo para funciones)

- **Funciones**:
    
    - **Acceso**: Solo desde fuera del contrato (no pueden ser llamadas internamente, a menos que se use `this`).
        
    - **Ventaja**: Más eficientes en gas al evitar copiar parámetros a memoria.
        
    - **Ejemplo**:
        ```Solidity
        function enviarETH(address destino) external payable {
            payable(destino).transfer(msg.value);
        }
        ```
        

El **gas** en Ethereum es una unidad de medida que representa el costo computacional necesario para ejecutar operaciones o transacciones en la red de Ethereum. Cada acción en la blockchain de Ethereum, como enviar tokens, ejecutar **smart contracts** o interactuar con aplicaciones descentralizadas (**dApps**), requiere una cierta cantidad de gas.

## **¿Cómo funciona?**

- **Costo de las operaciones:** Cada operación (las cuales se pueden descomponer en **Opcodes**) tiene un costo en gas, que depende de su complejidad. Por ejemplo, una transacción simple (como enviar ETH) consume menos gas que una operación más compleja (como ejecutar un contrato inteligente).

- **Precio del gas (gas price)**: Es la cantidad de **Gwei** (una fracción de ETH, donde 1 Gwei = 0.000000001 ETH) que estás dispuesto a pagar por cada unidad de gas. Los mineros priorizan las transacciones con un precio de gas más alto.

- **Límite de gas (gas limit)**: Es la cantidad máxima de gas que estás dispuesto a gastar en una transacción. Si la operación requiere más gas que el límite establecido, la transacción falla y el gas gastado no se devuelve.

### **Fórmula del costo total**

`Costo total = Gas usado * Precio del gas (en Gwei)`

### **Ejemplo**

Si una transacción usa **21,000 unidades de gas** y el precio del gas es **50 Gwei**, el costo total sería:

`21,000 * 50 Gwei = 1,050,000 Gwei = 0.00105 ETH`

## **¿Por qué existe el gas?**

- **Evitar el spam**: El gas asegura que los usuarios paguen por el uso de la red, evitando que se sature con operaciones innecesarias.

- **Incentivar a los validadores**: Los validadores reciben el gas como recompensa por procesar transacciones y mantener la red segura.

- **Predecir costos**: Permite a los usuarios estimar el costo de sus operaciones antes de ejecutarlas.
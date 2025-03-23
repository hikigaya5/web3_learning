# **¿Qué es una transacción?**

En el mundo de **blockchain**, una **transacción** es una operación registrada en la cadena de bloques que involucra el **envío, recepción o ejecución de datos** sin necesidad de intermediarios. Puede ser desde una simple transferencia de criptomonedas hasta la ejecución de **smart contracts**.

## **Propiedades de una transacción**

| **Campo**                  | **Descripción**                                                     |
| -------------------------- | ------------------------------------------------------------------- |
| **`from`**                 | Dirección del remitente (quién envía los fondos).                   |
| **`to`**                   | Dirección del destinatario (quién recibe los fondos).               |
| **`value`**                | Monto de criptomonedas transferido (Ej: 0.5 BTC, 1 ETH).            |
| **`nonce`**                | Número único para evitar que una transacción se repita.             |
| **`gasLimit`**             | Máximo de gas permitido para ejecutar la transacción.               |
| **`gasPrice`**             | Tarifa por unidad de gas (en Gwei o satoshis).                      |
| **`maxFeePerGas`**         | *(Ethereum 2.0)* Tarifa máxima dispuesta a pagar por unidad de gas. |
| **`maxPriorityFeePerGas`** | *(Ethereum 2.0)* Tarifa extra para incentivar validadores.          |
| **`transactionFee`**       | Costo total de la transacción (gas usado × gas price).              |
| **`hash`**                 | Identificador único de la transacción (TxID).                       |
| **`signature`**            | Firma digital generada con la clave privada del remitente.          |
| **`blockNumber`**          | Número del bloque donde se incluyó la transacción.                  |
| **`timestamp`**            | Fecha y hora en que se creó la transacción.                         |
| **`input`** *(Opcional)*   | Datos extra (Ej: interacción con un smart contract).                |

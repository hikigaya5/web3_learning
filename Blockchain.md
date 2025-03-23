# **¿Qué es blockchain?**

Blockchain es una tecnología de **registro distribuido** que permite almacenar información de forma **segura, transparente e inmutable** sin necesidad de una autoridad central. Se basa en una cadena de bloques que contienen transacciones o datos, y cada bloque está vinculado al anterior mediante criptografía.

##  **¿Cómo funciona?**

Aquí dejo un enlace a un ejemplo bastante bueno de cómo funciona.
https://andersbrownworth.com/blockchain/hash

Blockchain es un software que se ejecuta en distintos nodos conectados a internet al mismo tiempo. Cada bloque tiene las siguientes características:

- **Nº de bloque:** el número del bloque en la secuencia de la cadena de bloques. Al primer bloque se le llama bloque de génesis.

- **Nonce**: el **nonce** es un número que se utiliza en la función hash de la blockchain para encriptar los datos.

- **Data:** son los datos que se almacenan en la blockchain y que también se utilizan en la función hash. Generalmente código Solidity.

- **Prev:** un código hash que apunta al código hash del anterior bloque. El **prev** del bloque génesis es todo ceros.

- **Hash:** el código hash del bloque. Este se saca combinando el **Nº de bloque**, el **nonce**, el **data** y el **prev** en el algoritmo de encriptación **Keccak256** de la familia de los SHA256.

La cuestión es que en la blockchain todos los hashes tienen que tener una característica especial que **varía en función de la cadena**. En el ejemplo del enlace todos los hashes empiezan con 4 ceros, entonces cada vez que se cambia el campo **data** porque hay una nueva transacción el **hash** tiene que volver a ser recalculado porque ya no cumple con la característica de la cadena, y para recalcular el **hash** y que vuelva a tener los 4 ceros delante hay que buscar un nuevo **nonce** que pasándoselo a la función hash junto con el **data** devuelva un hash que cumpla con los 4 ceros. A este proceso de recalcular el hash se le llama minar y los nodos mineros ganan **ETH** con ello.

Cada nodo en el que se está ejecutando la blockchain tiene su propia versión de la blockchain, entonces para saber qué nodo es malicioso y está intentando modificar los datos de la cadena de bloques los nodos llegan a un consenso comparando los **hashes** del último bloque de todos los nodos (ya que si se modifica algo en mitad de la cadena modifica los hashes de todos los bloques en cascada) y se decide que el nodo que tenga el bloque con el hash diferente al de la mayoría de nodos que están en la cadena es expulsado.

## **Tipos de blockchain:**

Una **blockchain Layer 1** no es suficiente por sí sola debido a sus limitaciones de **escalabilidad** (pocas transacciones por segundo), **altos costos** (tarifas elevadas durante la congestión) y **falta de especialización** para casos de uso específicos. Además, las L1 suelen ser **poco eficientes** en términos de energía y almacenamiento de datos. Por eso, se necesitan **soluciones Layer 2**, **sidechains** y otras tecnologías complementarias para mejorar la velocidad, reducir costos y habilitar aplicaciones más complejas y masivas.
### **Blockchain Layer 1 (L1)**

Una **blockchain Layer 1** es la cadena de bloques principal y base de toda la red, como Ethereum, Bitcoin o Solana. Esta capa es responsable de la seguridad, el consenso y la ejecución de transacciones, utilizando mecanismos como **Proof of Work (PoW)** o **Proof of Stake (PoS)**. Sin embargo, las **L1** suelen enfrentar problemas de escalabilidad, como bajas transacciones por segundo y altos costos durante momentos de congestión, lo que limita su uso para aplicaciones masivas. A pesar de estas limitaciones, son fundamentales porque proporcionan la descentralización y seguridad que sustentan todo el ecosistema.

### **Blockchain Layer 2 (L2)**

Las **blockchains Layer 2** son soluciones construidas sobre una **L1** para mejorar su escalabilidad y eficiencia. Estas capas procesan transacciones fuera de la cadena principal (**off-chain**) y luego registran los resultados en la **L1**, lo que reduce la congestión y los costos. Ejemplos incluyen los **rollups**, que agrupan múltiples transacciones en una sola, y los **canales de pago**, como el **Lightning Network** de Bitcoin, que permiten transacciones instantáneas y económicas. Las **L2** dependen de la seguridad de la **L1** pero ofrecen una experiencia más rápida y económica para los usuarios.

### **Rollups (L2)**

Los **rollups** son una categoría de soluciones **L2** que "empaquetan" múltiples transacciones en una sola y las envían a la **L1** para su validación. Esto reduce drásticamente los costos y aumenta la velocidad de las transacciones. Existen dos tipos principales: los **Optimistic Rollups**, que asumen que las transacciones son válidas a menos que se demuestre lo contrario (usando pruebas de fraude), como Arbitrum y Optimism; y los **ZK-Rollups**, que utilizan pruebas criptográficas (zk-SNARKs o zk-STARKs) para demostrar la validez de las transacciones sin revelar todos los detalles, como zkSync y StarkWare. Ambos tipos mantienen la seguridad de la **L1** mientras mejoran su escalabilidad.

### **Sidechains (L2)**

Las **sidechains** son blockchains independientes pero conectadas a una **L1** mediante puentes (bridges) que permiten la transferencia de activos entre ambas. A diferencia de las **L2**, las sidechains tienen su propio mecanismo de consenso y reglas, lo que las hace más flexibles para casos de uso específicos, como transacciones rápidas y económicas. Un ejemplo destacado es **Polygon**, una sidechain de Ethereum que permite a los desarrolladores crear aplicaciones escalables sin sacrificar la interoperabilidad con la red principal. Aunque no dependen directamente de la seguridad de la **L1**, las sidechains son una parte importante del ecosistema blockchain.

### **Canales de pago (L2)**

Los **canales de pago** son soluciones **L2** que permiten transacciones instantáneas y de bajo costo fuera de la cadena principal, entre dos o más partes. Estas transacciones solo se registran en la **L1** cuando el canal se cierra, lo que reduce significativamente la congestión y los costos. Un ejemplo clásico es el **Lightning Network** de Bitcoin, que permite micropagos rápidos y económicos. Aunque son menos comunes en blockchains como Ethereum, los canales de pago son una herramienta poderosa para mejorar la escalabilidad en redes centradas en transacciones de valor.



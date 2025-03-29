
El consenso es el mecanismo que permite a los participantes de una blockchain ponerse de acuerdo sobre el estado de la red, como qué transacciones son válidas y en qué orden. Sin un intermediario central, este proceso es crucial para mantener la integridad y seguridad de la red. El consenso utilizado en Bitcoin y Ethereum se llama **Nakamoto Consensus** que combina **Proof of Work (PoW)** o **Proof of Stake (PoS)** (dependiendo de la blockchain) y la **Longest Chain Rule**. **PoW** y **PoS** son dos algoritmos de **Chain Selection** para decidir quién propone un nuevo bloque, por tanto quién tiene la cadena más larga y por lo tanto la cadena correcta. Tanto **PoW** como **PoS** tienen un mecanismo de **Sybil Resistance** lo que los hace resistentes a posibles ataques a la blockchain.

## **Chain Selection y Longest Chain Rule**

Cuando hay múltiples versiones de la cadena de bloques (por ejemplo, debido a un fork), los nodos deben decidir cuál es la cadena "correcta". La cadena correcta será la cadena más larga (**Longest Chain Rule**) y esta será la del nodo que haya añadido el último bloque siguiendo alguno de los mecanismos de consenso para proponer el bloque. Hay 2 mecanismos de consenso: **Proof of Work** y **Proof of Stake**. En **PoW**, esto se hace siguiendo la cadena con mayor **trabajo acumulado**, es decir, la que ha conseguido minar antes el bloque. En **PoS**, se sigue la cadena con mayor **participación económica**, es decir, la que tiene más tokens apostados. Este proceso asegura que todos los nodos estén de acuerdo sobre el estado actual de la red.
### **Proof of Work (PoW)**

En **Proof of Work**, los participantes (mineros) compiten para resolver problemas matemáticos complejos que requieren un gran poder computacional. El primero en resolver el problema propone un nuevo bloque y recibe una recompensa. Este proceso, aunque seguro, consume mucha energía y limita la escalabilidad de la red. Bitcoin es el ejemplo más conocido de **PoW**.

### **Proof of Stake (PoS)**

Por otro lado, en **Proof of Stake**, los validadores son seleccionados para proponer bloques en función de la cantidad de tokens que han "apostado" (**stake**) en la red. En lugar de competir con poder computacional, la probabilidad de ser seleccionado depende de la participación económica. Si un nodo se comporta como no es debido puede perder todo lo apostado. Este mecanismo es más eficiente energéticamente y escalable, pero puede llevar a cierta centralización si unos pocos validadores acumulan muchos tokens. Ethereum, después de su transición a Ethereum 2.0, es un ejemplo destacado de **PoS**.

## **Sybil Resistance y Sybil Attack**

La **Sybil Resistance** es la capacidad de una red para resistir **Sybil Attacks**, donde un atacante crea múltiples identidades falsas para ganar control sobre esta. En **PoW**, la **Sybil Resistance** se logra mediante el alto costo computacional de minar bloques, lo que hace que crear múltiples identidades sea económicamente inviable. En **PoS**, se logra mediante el requisito de apostar tokens, lo que hace que crear múltiples identidades sea costoso en términos económicos. Sin una buena resistencia Sybil, una red sería vulnerable a la manipulación por parte de un atacante.

### **51% Attack**

Un **51% Attack** ocurre cuando un atacante controla más del 50% del poder de minería (en **PoW**) o de los tokens apostados (en **PoS**), lo que le permite manipular la red. Por ejemplo, podría revertir transacciones o realizar doble gasto. La **Sybil Resistance** y los mecanismos de consenso como **PoW** y **PoS** están diseñados para hacer que estos ataques sean extremadamente costosos o prácticamente imposibles de ejecutar.
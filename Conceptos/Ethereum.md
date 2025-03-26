
**Ethereum** es una plataforma de **blockchain descentralizada y de código abierto** diseñada para permitir la creación y ejecución de **smart contracts** y **aplicaciones descentralizadas**. A diferencia de Bitcoin, que se centra en ser un sistema de dinero digital, Ethereum es una red programable que amplía las posibilidades de la tecnología blockchain más allá de las transacciones financieras. El **Ether (ETH)** es la criptomoneda de Ethereum, utilizada para pagar las tarifas de transacción (**gas fees**) y recompensar a los validadores.

## **EIP 1559**

El **EIP 1559** es una actualización de Ethereum que reformó el sistema de tarifas para hacerlo **más predecible**, aunque no necesariamente más barato. Antes, las tarifas dependían de una subasta competitiva, lo que causaba picos caóticos durante la congestión. Con el EIP 1559, se introdujo una **base fee** (tarifa base) que se ajusta dinámicamente **bloque a bloque**, siguiendo estas reglas:

- Cada bloque tiene un **objetivo de 15 millones de gas** y un **límite máximo de 30 millones**.
    
- Si un bloque supera el objetivo (p. ej., usa 16M de gas), la base fee del siguiente bloque aumenta en un porcentaje proporcional al exceso, pero **nunca más del 12.5%**, incluso si el bloque está lleno al máximo (30M).
    
- Si un bloque está por debajo del objetivo, la base fee disminuye proporcionalmente, también con un **límite máximo del 12.5%** por bloque.
    

Este diseño **autorregula la demanda**: en congestión extrema, la base fee sube exponencialmente (hasta +12.5% por bloque), hasta que los usuarios, al ver tarifas demasiado altas, dejan de enviar transacciones, lo que reduce la congestión y permite que la base fee baje. Aunque las tarifas pueden seguir siendo altas en picos de actividad, ahora son **predecibles**, ya que los monederos (como MetaMask) estiman la base fee en tiempo real y permiten a los usuarios establecer un **fee cap** (límite máximo que pagarán, incluyendo una **fee tip** opcional para incentivar a los validadores a priorizar su transacción). Si la suma de la **base fee + fee tip** supera este límite durante la inclusión en un bloque, la transacción se cancela, evitando sorpresas.

La **fee tip** va completamente a los validadores mientras que la **base fee se quema** (se destruye ETH), lo que, en períodos de alta demanda, puede hacer que Ethereum sea **deflacionario** (cuando se quema más ETH del que se emite por nuevos bloques). Sin embargo, el objetivo principal del EIP 1559 no es reducir las tarifas, sino **eliminar las conjeturas** al pagar por transacciones, ofreciendo un equilibrio entre eficiencia económica y experiencia de usuario. Las tarifas bajas dependen más de la escalabilidad de la red (con soluciones como Layer 2) que de este mecanismo.
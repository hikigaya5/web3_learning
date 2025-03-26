
Una key es una cadena de carácteres (String) que en el caso de blockchain sirve para cifrar transacciones. Hay de varios tipos:

- **Private key:** es la clave privada que se crea para la cuenta que tenemos en nuestra cartera. Se deriva a partir de la **secret phrase** combinada con un **nonce**.

- **Public key:** es la clave pública asociada a la cuenta que toda la gente puede ver. Se deriva de la clave privada con el algoritmo **ECSDA**.

- **Secret phrase:** 12 palabras que se crean cuando creamos nuestra wallet.

Cuando firmas una transacción con tu **private key** se crea un **hash** con el algoritmo de **ECDSA** que es la firma de transacción. Gracias a este algoritmo nadie puede derivar tu **private key** a partir de la firma de transacción, pero sí se puede verificar que la firma de transacción es correcta usando tu **public key**.

Si alguien descubre tu **private key** podrá enviar transacciones con la cuenta a la que está asociada esa **private key**.

Si alguien descubre tu **secret phrase** podrá enviar transacciones con todas las cuentas de tu walllet ya que podrá derivar todas las **private keys**.
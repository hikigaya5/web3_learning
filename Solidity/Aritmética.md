Un **desbordamiento** (overflow) o **subdesbordamiento** (underflow) es la situación en la que el valor resultante de una operación aritmética, al ejecutarse en un entero no restringido, cae fuera del rango del tipo de resultado.

Antes de Solidity 0.8.0, las operaciones aritméticas simplemente retornaban al valor del otro extremo (wrap) en caso de subdesbordamiento o desbordamiento, lo que llevó al uso generalizado de bibliotecas que introducen verificaciones adicionales (como SafeMath).

Desde Solidity 0.8.0, todas las operaciones aritméticas **revierte** (revert) por defecto en caso de desbordamiento o subdesbordamiento, haciendo innecesario el uso de estas bibliotecas.

Para obtener el comportamiento anterior (ajuste en lugar de revertir), se puede usar un **bloque `unchecked`**:

```Solidity
pragma solidity ^0.8.0;
contract C {
    function f(uint a, uint b) pure public returns (uint) {
        // This subtraction will wrap on underflow.
        unchecked { return a - b; }
    }
    function g(uint a, uint b) pure public returns (uint) {
        // This subtraction will revert on underflow.
        return a - b;
    }
}
```

Usar un bloque `unchecked` puede ahorrar [[Gas|gas]].
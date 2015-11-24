# ethereumjs-codesim
A simple CLI app for running EVM simulations. It supports loading raw EVM bytecode or Solidity source files.

## Usage

There are plenty of options for ```codesim```.

First of the source must be specified:
- ```-c``` for specifying the actual code in the commandline
- ```-f``` for supplying a filename

By default it assumes the source is an EVM bytecode. To compile with ```solc``` pass ```-C```. Optionally, ```-optimize``` will turn on optimizations in ```solc```. Also a Solidity source code might contain multiple contracts, choose the desired one with ```--use-contract```.

It is assumed, the bytecode is a contract deployment code which deploys the actual code. This means ```codesim``` will execute two simulations: deployment, followed by the actual contract execution. To disabled this, use ```-R``` for setting runtime mode.

Raw data can be passed to the contract with ```-D <data>```.

Alternatively use the ABI encoder with ```---callmethod```.

For the following Solidity ```example.sol```:
```js
contract Test {
  function method1() returns (bool) {
    return true;
  }
  function method2(bool test) returns (bool) {
    return test;
  }
  function method3(string test) returns (string) {
    return test;
  }
}
```

Example invocations and outputs:
- ```codesim -C -f example.sol --callmethod 'method1()'```:  ```0000000000000000000000000000000000000000000000000000000000000001``` (true)
- ```codesim -C -f example.sol --callmethod 'method2(bool)' true```: ```0000000000000000000000000000000000000000000000000000000000000001``` (true)
- ```codesim -C -f example.sol --callmethod 'method3(string)' 'Hello world'```: ```0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000b48656c6c6f20776f726c64000000000000000000000000000000000000000000``` (ABI encoded 'Hello world')

For more commandline options see ```codesim -h```.

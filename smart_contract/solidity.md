# Notes from CryptoZombies, etc

## Version Pragma

All solidity source code should start with a "version pragma" — a declaration of the version of the Solidity compiler this code should use. This is to prevent issues with future compiler versions potentially introducing changes that would break your code.

To compile our smart contracts with any compiler version in the range of 0.5.0 (inclusive) to 0.6.0 (exclusive). It looks like this:

```solidity
pragma solidity >=0.5.0 <0.6.0;
```

## Contract

Starting with the absolute basics:

Solidity's code is encapsulated in contracts. A contract is the fundamental building block of Ethereum applications — all variables and functions belong to a contract, and this will be the starting point of all your projects.

An empty contract named HelloWorld would look like this:

```solidity
contract HelloWorld {

}
```

## State variables

State Variables are permanently stored in contract storage. This means they're written to the Ethereum blockchain. Think of them like writing to a DB.

```solidity
contract Example {
  // This will be stored permanently in the blockchain
  uint myUnsignedInteger = 100;
}
```

In this example contract, we created a uint called myUnsignedInteger and set it equal to 100.

## Unsigned Integers: uint

The uint data type is an unsigned integer, meaning its value must be non-negative. There's also an int data type for signed integers.

Note: In Solidity, uint is actually an alias for uint256, a 256-bit unsigned integer. You can declare uints with less bits — uint8, uint16, uint32, etc.. But in general you want to simply use uint except in specific cases, which we'll talk about in later lessons.

## Math Operations

Math in Solidity is pretty straightforward. The following operations are the same as in most programming languages:

- Addition: x + y
- Subtraction: x - y,
- Multiplication: x \* y
- Division: x / y
- Modulus / remainder: x % y (for example, 13 % 5 is 3, because if you divide 5 into 13, 3 is the remainder)
- Exponent: x**y OR x^y (uint x = 5 ** 2; // equal to 5^2 = 25)

## Structs (like class)

Sometimes you need a more complex data type. For this, Solidity provides structs:

```solidity
struct Person {
  uint age;
  string name;
}
```

Structs allow you to create more complicated data types that have multiple properties.

Note that we just introduced a new type, string. Strings are used for arbitrary-length UTF-8 data. Ex. string greeting = "Hello world!"

## Arrays

Fixed arrays and dynamic arrays:

Fixed:

- Array with a fixed length of 2 elements:
  uint[2] fixedArray;
- another fixed Array, can contain 5 strings:
  string[5] stringArray;

Dynamic:

- a dynamic Array - has no fixed size, can keep growing:
  uint[] dynamicArray;
- You can also create an array of structs. Using the previous chapter's Person struct:
- Person[] people; // dynamic Array, we can keep adding to it

State variables are stored permanently in the blockchain.

- So creating a dynamic array of structs like this can be useful for storing structured data in your contract, kind of like a database.

Public Arrays:

Auto-create `getter`

- You can declare an array as public, and Solidity will automatically create a getter method for it.

```solidity
Person[] public people;
```

- Other contracts would then be able to read from, but not write to, this array. So this is a useful pattern for storing public data in your contract.

## Function Declarations

A function declaration in solidity looks like the following:

```solidity
function eatHamburgers(string memory _name, uint _amount) public {

}
```

- Note that we're specifying the function visibility as public.

- We're also providing instructions about where the \_name variable should be stored- in memory. This is required for all reference types such as arrays, structs, mappings, and strings.

### Reference types:

There are two ways in which you can pass an argument to a Solidity function:

- By value, which means that the Solidity compiler creates a new copy of the parameter's value and passes it to your function. This allows your function to modify the value without worrying that the value of the initial parameter gets changed.

- By reference, which means that your function is called with a reference to the original variable. Thus, if your function changes the value of the variable it receives, the value of the original variable gets changed.

### Underscore arguments

Note: It's convention (but not required) to start function parameter variable names with an underscore (\_) in order to differentiate them from global variables. We'll use that convention throughout our tutorial.

You would call this function like so:

```solidity
eatHamburgers("vitalik", 100);
```

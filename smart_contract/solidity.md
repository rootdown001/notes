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

## Create structs

For the following `struct`

```solidity
struct Person {
  uint age;
  string name;
}
```

create public array of Person struct:

```solidity
Person[] public people;
```

Create new Persons:

```solidity
Person satoshi = Person(172, "Satoshi");
```

Add that person to the Array:

```solidity
people.push(satoshi);
```

Or do in one line (bypass using `satoshi` name to create):

```solidity
people.push(Person(16, "Vitalik"));
```

Note that array.push() adds something to the end of the array, so the elements are in the order we added them. See the following example:

```solidity
uint[] numbers;
numbers.push(5);
numbers.push(10);
numbers.push(15);
// numbers is now equal to [5, 10, 15]
```

## Public / Private functions

- functions are public by default. This means anyone (or any other contract) can call your contract's function and execute its code.

It's good practice to mark your functions as private by default, and then only make public the functions you want to expose to the world.

Let's look at how to declare a private function:

```solidity
uint[] numbers;

function _addToArray(uint _number) private {
  numbers.push(_number);
}
```

- only other functions within our contract will be able to call this function and add to the numbers array.

## Functions: Return Values

To return a value from a function, the declaration looks like this:

```solidity
string greeting = "What's up dog";

function sayHello() public returns (string memory) {
  return greeting;
}
```

- the function declaration contains the type of the return value (in this case string).

## Functions: Function modifiers

### `view` - only views, doesn't modify

So in this case we could declare it as a view function, meaning it's only viewing the data but not modifying it:

```solidity
function sayHello() public view returns (string memory) {
  return greeting
}
```

### `pure` - only depends on function parameters, not accessing data in app

Solidity also contains pure functions, which means you're not even accessing any data in the app. Consider the following:

function \_multiply(uint a, uint b) private pure returns (uint) {
return a \* b;
}
This function doesn't even read from the state of the app — its return value depends only on its function parameters. So in this case we would declare the function as pure.

compiler helps us

- Note: It may be hard to remember when to mark functions as pure/view. Luckily the Solidity compiler is good about issuing warnings to let you know when you should use one of these modifiers.

## Keccak256

We want our \_generateRandomDna function to return a (semi) random uint. How can we accomplish this?

- Ethereum has the hash function keccak256 built in, which is a version of SHA3.
- A hash function basically maps an input into a random 256-bit hexadecimal number. A slight change in the input will cause a large change in the hash.

- It's useful for many purposes in Ethereum, but for right now we're just going to use it for pseudo-random number generation.

- keccak256 expects a single parameter of type bytes
- Needs to be "packed" using `abi.encodePacked("aaaab")`

Example:

```solidity

keccak256(abi.encodePacked("aaaab"));

//6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5

keccak256(abi.encodePacked("aaaac"));

//b1f078126895a1424524de5321b339ab00408010b7cf0e6ed451514981e58aa9

```

As you can see, the returned values are totally different despite only a 1 character change in the input.

Note: Secure random-number generation in blockchain is a very difficult problem. Our method here is insecure, but since security isn't top priority for our Zombie DNA, it will be good enough for our purposes.

## Typecasting

Typecasting - convert between data types. Take the following example:

```solidity
uint8 a = 5;
uint b = 6;
// throws an error because a * b returns a uint, not uint8:
uint8 c = a * b;
// we have to typecast b as a uint8 to make it work:
uint8 c = a * uint8(b);
```

In the above, a \* b returns a uint, but we were trying to store it as a uint8, which could cause potential problems. By casting it as a uint8, it works and the compiler won't throw an error.

## Events

Events are a way for your contract to communicate that something happened on the blockchain to your app front-end, which can be 'listening' for certain events and take action when they happen.

Example:

```solidity
// declare the event
event IntegersAdded(uint x, uint y, uint result);

function add(uint _x, uint _y) public returns (uint) {
  uint result = _x + _y;
  // fire an event to let the app know the function was called:
  emit IntegersAdded(_x, _y, result);
  return result;
}
```

Your app front-end could then listen for the event. A JavaScript implementation would look something like:

```javascript
YourContract.IntegersAdded(function (error, result) {
  // do something with result
});
```

## Web3.js

Ethereum has a JavaScript library called Web3.js.

In a later lesson, we'll go over in depth how to deploy a contract and set up Web3.js. But for now let's just look at some sample code for how Web3.js would interact with our deployed contract.

Don't worry if this doesn't all make sense yet.

```javascript
// Here's how we would access our contract:
var abi = /* abi generated by the compiler */
var ZombieFactoryContract = web3.eth.contract(abi)
var contractAddress = /* our contract address on Ethereum after deploying */
var ZombieFactory = ZombieFactoryContract.at(contractAddress)
// `ZombieFactory` has access to our contract's public functions and events

// some sort of event listener to take the text input:
$("#ourButton").click(function(e) {
  var name = $("#nameInput").val()
  // Call our contract's `createRandomZombie` function:
  ZombieFactory.createRandomZombie(name)
})

// Listen for the `NewZombie` event, and update the UI
var event = ZombieFactory.NewZombie(function(error, result) {
  if (error) return
  generateZombie(result.zombieId, result.name, result.dna)
})

// take the Zombie dna, and update our image
function generateZombie(id, name, dna) {
  let dnaStr = String(dna)
  // pad DNA with leading zeroes if it's less than 16 characters
  while (dnaStr.length < 16)
    dnaStr = "0" + dnaStr

  let zombieDetails = {
    // first 2 digits make up the head. We have 7 possible heads, so % 7
    // to get a number 0 - 6, then add 1 to make it 1 - 7. Then we have 7
    // image files named "head1.png" through "head7.png" we load based on
    // this number:
    headChoice: dnaStr.substring(0, 2) % 7 + 1,
    // 2nd 2 digits make up the eyes, 11 variations:
    eyeChoice: dnaStr.substring(2, 4) % 11 + 1,
    // 6 variations of shirts:
    shirtChoice: dnaStr.substring(4, 6) % 6 + 1,
    // last 6 digits control color. Updated using CSS filter: hue-rotate
    // which has 360 degrees:
    skinColorChoice: parseInt(dnaStr.substring(6, 8) / 100 * 360),
    eyeColorChoice: parseInt(dnaStr.substring(8, 10) / 100 * 360),
    clothesColorChoice: parseInt(dnaStr.substring(10, 12) / 100 * 360),
    zombieName: name,
    zombieDescription: "A Level 1 CryptoZombie",
  }
  return zombieDetails
}
```

What our JavaScript then does is take the values generated in zombieDetails above, and use some browser-based JavaScript magic (we're using Vue.js) to swap out the images and apply CSS filters. You'll get all the code for this in a later lesson.

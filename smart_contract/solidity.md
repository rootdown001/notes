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

## Truffle (note that Hardhat is more popular now)

Truffle is (WAS) the most popular blockchain development framework for good reason - it's packed with lots of useful features:

- easy smart contract compilation
- automated ABI generation
- integrated smart contract testing - there's even support for Mocha and Chai!
- support for multiple networks - code can be deployed to Rinkeby, Ethereum or even to Loom. We'll walk you through this later😉
  Provided that npm and node have been installed on your computer, we'll want you to install Truffle and make it available globally.

```bash
npm install package_name -g
```

### Getting Started with Truffle

Now that we've installed Truffle, it's time to initialize our new project by running truffle init. All it is doing is to create a set of folders and config files with the following structure:

```
├── contracts
    ├── Migrations.sol
├── migrations
    ├── 1_initial_migration.js
└── test
truffle-config.js
truffle.js
```

Truffle's Default Directory Structure

- contracts: this is the place where Truffle expects to find all our smart contracts. To keep the code organized, we can even create nested folders such as contracts/tokens. Pretty neat😉.

- Note: truffle init should automatically create a contract called Migrations.sol and the corresponding migration file. We'll explain them a bit later.

- migrations: a migration is a JavaScript file that tells Truffle how to deploy a smart contract.

- test: here we are expected to put the unit tests which will be JavaScript or Solidity files. Remember, once a contract is deployed it can't be changed, making it essential that we test our smart contracts before we deploy them.

- truffle.js and truffle-config.js: config files used to store the network settings for deployment. Truffle needs two config files because on Windows having both truffle.js and truffle.exe in the same folder might generate conflicts. Long story short - if you are running Windows, it is advised to delete truffle.js and use truffle-config.js as the default config file. Check out Truffle's official documentation to further your understanding.

1.  Truffle will not work as expected if you change the names of these folders.

2.  By adhering to this convention your projects will be easily understood by other developers. To put it short, using a standard folder structures and code conventions make it easier if you expand or change your team in the future.

truffle-hdwallet-provider

- In this lesson, we will be using Infura to deploy our code to Ethereum. This way, we can run the application without needing to set up our own Ethereum node or wallet. However, to keep things secure, Infura does not manage the private keys, which means it can't sign transactions on our behalf. Since deploying a smart contract requires Truffle to sign transactions, we are going to need a tool called truffle-hdwallet-provider. Its only purpose is to handle the transaction signing.

Note: Maybe you are asking why we chose not to install truffle-hdwallet-provider in the previous chapter using something like:

```
npm install truffle truffle-hdwallet-provider
```

- the truffle init command expects to find an empty directory. If there's any file there, it will error out. Thus, we need to do everything in the correct order and install truffle-hdwallet-provider after we run truffle init.

### Compiling the Source Code

The Ethereum Virtual Machine can't directly understand Solidity source code as we write it. Thus, we need to run a compiler that will "translate" our smart contract into machine-readable bytecode. The virtual machine then executes the bytecode, and completes the actions required by our smart contract.

Byte-code

"0x60806040526010600155600154600a0a6002556201518060035566038d7ea4c6800060085560006009556046600a55336000806101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1..."

Using the Solidity Compiler

Let's pretend we'd want to modify the definition of the add function included in SafeMath:

```
function add(uint16 a, uint16 b) internal returns (uint16) {
    uint16 c = a + b;
    assert(c >= a);
    return c;
}
```

If we're going to compile this function, the Solidity compiler will throw a warning:

```
safemath.sol:110:11: Warning: Function state mutability can be restricted to pure
          function add(uint16 a, uint16 b) internal returns (uint16) {
          ^ (Relevant source part starts here and spans across multiple lines).
```

What the compiler is trying to say is that our function does not read or write from/to the blockchain and that we should use the pure modifier.

**making a function pure or view saves us gas**

- Since these functions are not going to modify the state of the blockchain, there is no need for miners to execute them. To put it in a few words, pure and view functions can be called for free.

CryptoZombies- The Game
Remember, we've embedded our logic in a smart contract called ZombieOwnership.sol.

We can use inheritance to create a smart contract with the same actions and features with whatever name we choose.

```
pragma solidity ^0.4.24;

import "./zombieownership.sol";

contract CryptoZombies is ZombieOwnership
    {

    }
```

Next, we copied all our smart contracts into the ./contracts folder. Now the project structure should look like this:

```
├── contracts
    ├── Migrations.sol
    ├── CryptoZombies.sol
    ├── erc721.sol
    ├── ownable.sol
    ├── safemath.sol
    ├── zombieattack.sol
    ├── zombiefactory.sol
    ├── zombiefeeding.sol
    ├── zombiehelper.sol
    ├── zombieownership.sol
├── migrations
└── test
```

Compile...

```
truffle compile
```

- This command should create the build artifacts and place them in the ./build/contracts directory.

Note: The build artifacts are comprised of the "bytecode" versions of the smart contracts, ABIs, and some internal data Truffle is using to correctly deploy the code. Avoid editing these files, or Truffle might stop working correctly.

### Migrations

Normally at this point, before deploying to Ethereum, you would want to test your smart contract locally.

#### Ganache (sets up a local Ethereum network to test)

To deploy to Ethereum we will have to create something called a migration.

- Migrations are JavaScript files that help Truffle deploy the code to Ethereum. Note that truffle init created a special contract called Migrations.sol that keeps track of the changes you're making to your code.
- The way it works is that the history of changes is saved onchain. Thus, there's no way you will ever deploy the same code twice.

Creating a New Migration

We'll start from the file truffle init already created for us- `./contracts/1_initial_migration.js` Let's take a look at what's inside:

```javascript
var Migrations = artifacts.require("./Migrations.sol");
module.exports = function (deployer) {
  deployer.deploy(Migrations);
};
```

1.  the script tells Truffle that we'd want to interact with the Migrations contract.
2.  it exports a function that accepts an object called deployer as a parameter. This object acts as an interface between you (the developer) and Truffle's deployment engine.

## Configuration Files

There is still one more thing to do before we can deploy. We'll have to edit the configuration file to tell Truffle the networks we want to deploy to.

Ethereum Test Networks

- Several public Ethereum test networks let you test your contracts for free before you deploy them to the main net (remember once you deploy a contract to the main net it can't be altered). These test networks use a different consensus algorithm to the main net (usually PoA), and Ether is free to encourage thorough testing.

In this lesson, we will be using Rinkeby, a public test network created by The Ethereum Foundation.

### truffle.js configuration file

```
$ cat truffle.js
/*
 * NB: since truffle-hdwallet-provider 0.0.5 you must wrap HDWallet providers in a
 * function when declaring them. Failure to do so will cause commands to hang. ex:
 *
 * mainnet: {
 *     provider: function() {
 *       return new HDWalletProvider(mnemonic, 'https://mainnet.infura.io/<infura-key>')
 *     },
 *     network_id: '1',
 *     gas: 4500000,
 *     gasPrice: 10000000000,
 *   },
 */
```

It is just an empty shell. Thus, we'll be required to update this file to allow us to deploy contracts to Rinkeby and the Ethereum mainnet.

### Truffle's HD Wallet Provider

`truffle-hdwallet-provider` helps Truffle sign transactions.

Now, we want to edit our configuration file to use HDWalletProvider. To get this to work we'll add a line at the top of the file:

```
const HDWalletProvider = require("truffle-hdwallet-provider");
```

Next, we'll create a new variable to store our mnemonic:

```
const mnemonic = "onions carrots beans ...";
```

- we don't recommend storing secrets like a mnemonic or a private key in a configuration file.
- Config files are often pushed to GitHub, where anyone can see them, leaving you wide open to attack 😱! To avoid revealing your mnemonic (or your private key!), you should read it from a file and add that file to .gitignore. We'll show you how to do this later.
- To keep things simple in this case, we've copied the mnemonic and stored in a variable.

### Set up Truffle for Rinkeby and Ethereum main net

- to make sure Truffle "knows" the networks we want to deploy to, we will have to create two separate objects- one for Rinkeby and the other one for the Ethereum main net:

```javascript
networks: {
// Configuration for mainnet
mainnet: {
provider: function () {
// Setting the provider with the Infura Mainnet address and Token
return new HDWalletProvider(mnemonic, "https://mainnet.infura.io/v3/YOUR_TOKEN")
},
network_id: "1"
},
// Configuration for rinkeby network
rinkeby: {
// Special function to setup the provider
provider: function () {
// Setting the provider with the Infura Rinkeby address and Token
return new HDWalletProvider(mnemonic, "https://rinkeby.infura.io/v3/YOUR_TOKEN")
},
// Network id is 4 for Rinkeby
network_id: 4
}
```

Note: the provider value is wrapped in a function, which ensures that it won't get initialized until it's needed.

Now, let's put these piece together and have a look at how our config file should look:

```javascript
// Initialize HDWalletProvider
const HDWalletProvider = require("truffle-hdwallet-provider");

// Set your own mnemonic here
const mnemonic = "YOUR_MNEMONIC";

// Module exports to make this configuration available to Truffle itself
module.exports = {
  // Object with configuration for each network
  networks: {
    // Configuration for mainnet
    mainnet: {
      provider: function () {
        // Setting the provider with the Infura Mainnet address and Token
        return new HDWalletProvider(
          mnemonic,
          "https://mainnet.infura.io/v3/YOUR_TOKEN"
        );
      },
      network_id: "1",
    },
    // Configuration for rinkeby network
    rinkeby: {
      // Special function to setup the provider
      provider: function () {
        // Setting the provider with the Infura Rinkeby address and Token
        return new HDWalletProvider(
          mnemonic,
          "https://rinkeby.infura.io/v3/YOUR_TOKEN"
        );
      },
      // Network id is 4 for Rinkeby
      network_id: 4,
    },
  },
};
```

## Deploying Our Smart Contract

- actually deploying to Rinkeby is going to be straightforward. To do this, Truffle relies on something called a migration.

#### Migrations

- a migration is nothing more than a JavaScript file that tells Truffle how to modify the state of our smart contracts.
- Obviously, the first migration will just deploy the smart contract. Some other migrations deploy a new version of the code to add features or fix bugs.
- migrations provide a convenient way to keep track of the changes you make to your code.
- if you want to deploy more than one contract, a separate migration file must be created for each contract. Migrations are always executed in order- 1,2,3, etc.

Deploy to Rinkeby first and test the code.

#### Get some Ether

- Before doing the deployment, make sure there is enough Ether in your account. The easiest way to get Ether for testing purposes is through a service known as a faucet.
- one is the Authenticated Faucet running on Rinkeby. Follow the instructions, and within a few minutes, your address will be credited with some Ether.

#### Put it to the test

```
truffle migrate --network rinkeby
```

- deployment to the main net is not complicated at all. Once the smart contract is tested, you'll only have to run:

```
truffle migrate --network mainnet
```

- don't forget that you'll have to pay for gas! We trust you'll be able to do it.

If everything goes well, you're going to see a response that's similar to the one to the right.

## Use Truffle with Loom!

### Loom Basechain

- if you want to build DApps on Ethereum, there's one thing you should be aware of - on the main net, users are required to pay gas fees for every transaction. But this isn't ideal for a user-facing DApp or a game. It can easily ruin the user experience.
- on Loom, your users can have much speedier and gas-free transactions, making it a much better fit for games and other non-financial applications.
- deploying to Loom is no different from deploying to Rinkeby, or to the Ethereum main net. If you know how to do one, you also know how to do the other.

### loom-truffle-provider

- we at Loom are using Truffle to build, test, and deploy our smart contracts.
- to make our life easier, we developed something called a provider that lets Truffle deploy to Loom just like it deploys to Rinkeby or Ethereum main net.
- the provider works like a bridge that makes Web3 calls compatible with Loom. The beauty of it is that, to use it, you don't have to understand how it works under the hood.

Put it to the test:
We've made loom-truffle-provider available as an npm package. Let's install it.

`npm install loom-truffle-provider`

Note: This time, there's no need to make the package available globally.

## Deploy to Loom Testnet

- deploy our smart contract to the Loom Testnet, but before doing the deployment, some prep work is needed.

1.  we should create our own Loom private key. The easiest way to do it is by downloading and installing Loom according to this tutorial.
2.  creating a private key is as as simple as this:

```bash
$./loom genkey -a public_key -k private_key
local address: 0x42F401139048AB106c9e25DCae0Cf4b1Df985c39
local address base64: QvQBE5BIqxBsniXcrgz0sd+YXDk=
$cat private_key
/i0Qi8e/E+kVEIJLRPV5HJgn0sQBVi88EQw/Mq4ePFD1JGV1Nm14dA446BsPe3ajte3t/tpj7HaHDL84+Ce4Dg==
```

- Note: Never reveal your private keys! We are only doing this for simplicity's sake.

### Updating truffle.js

1.  first thing we are required to do is to initialize loom-truffle-provider. The syntax is similar to the one we've already used for HDWalletProvider:

```javascript
const LoomTruffleProvider = require("loom-truffle-provider");
```

2.  let Truffle know how to deploy on Loom testnet. To do so, let's add a new object to truffle.js

```javascript
loom_testnet: {
  provider: function() {
    const privateKey = 'YOUR_PRIVATE_KEY'
    const chainId = 'extdev-plasma-us1';
    const writeUrl = 'http://extdev-plasma-us1.dappchains.com:80/rpc';
    const readUrl = 'http://extdev-plasma-us1.dappchains.com:80/query';
    return new LoomTruffleProvider(chainId, writeUrl, readUrl, privateKey);
    },
  network_id: '9545242630824'
}
```

Now...

```javascript
// Initialize HDWalletProvider
const HDWalletProvider = require("truffle-hdwallet-provider");

const LoomTruffleProvider = require("loom-truffle-provider");

const mnemonic = "YOUR_MNEMONIC";

module.exports = {
  networks: {
    mainnet: {
      provider: function () {
        return new HDWalletProvider(
          mnemonic,
          "https://mainnet.infura.io/v3/YOUR_TOKEN"
        );
      },
      network_id: "1",
    },

    rinkeby: {
      provider: function () {
        return new HDWalletProvider(
          mnemonic,
          "https://rinkeby.infura.io/v3/YOUR_TOKEN"
        );
      },

      network_id: 4,
    },

    loom_testnet: {
      provider: function () {
        const privateKey = "YOUR_PRIVATE_KEY";
        const chainId = "extdev-plasma-us1";
        const writeUrl = "http://extdev-plasma-us1.dappchains.com:80/rpc";
        const readUrl = "http://extdev-plasma-us1.dappchains.com:80/query";
        return new LoomTruffleProvider(chainId, writeUrl, readUrl, privateKey);
      },
      network_id: "9545242630824",
    },
  },
};
```

#### deploy

```
truffle migrate --network loom_testnet
```

## Deploy to Basechain

- walk you through the process of deploying to Basechain (that is our Mainnet).

Here's a brief rundown of what you'll do in this chapter:

### Create a new private key

- Creating a new private key is pretty straightforward
- since we're talking about deploying to the main net, it's time to get more serious about security. - we'll show you how to securely pass the private key to Truffle.

1.  Tell Truffle how to deploy to Basechain by adding a new object to the truffle.js configuration file.
2.  Whitelist your deployment keys so you can deploy to Basechain.
3.  we wrap everything up by actually deploying the smart contract.

#### Create a new private key

- You already know how to create a private key. However, we must change the name of the file in which we're going to save it:

```javascript
./loom genkey -a mainnet_public_key -k mainnet_private_key
local address: 0x07419790A773Cc6a2840f1c092240922B61eC778
local address base64: B0GXkKdzzGooQPHAkiQJIrYex3g=
```

#### Securely pass the private key to Truffle

- prevent the private key file from being pushed to GitHub. To do so, let's create a new file called .gitignore:

```
touch .gitignore
```

- Now let's "tell" GitHub that we want it to ignore the file in which we saved the private key by entering the following command:

```
echo mainnet_private_key >> .gitignore
```

- Now that we made sure our secrets aren't going to be pushed to GitHub, we must edit the truffle.js configuration file and make it so that Truffle reads the private key from this file.
- Let's start by importing a couple of things:

```javascript
const { readFileSync } = require("fs");
const path = require("path");
const { join } = require("path");
```

- next, we would want to define a function that reads the private key from a file and initializes a new LoomTruffleProvider:

```javascript
function getLoomProviderWithPrivateKey(
  privateKeyPath,
  chainId,
  writeUrl,
  readUrl
) {
  const privateKey = readFileSync(privateKeyPath, "utf-8");
  return new LoomTruffleProvider(chainId, writeUrl, readUrl, privateKey);
}
```

Pretty straightforward, isn't it?

#### Tell Truffle how to deploy to Basechain

- Now, we must let Truffle know how to deploy to Basechain. To do so, let's add a new object to truffle.js

```javascript
basechain: {
  provider: function() {
    const chainId = 'default';
    const writeUrl = 'http://basechain.dappchains.com/rpc';
    const readUrl = 'http://basechain.dappchains.com/query';
    const privateKeyPath = path.join(__dirname, 'mainnet_private_key');
    const loomTruffleProvider = getLoomProviderWithPrivateKey(privateKeyPath, chainId, writeUrl, readUrl);
    return loomTruffleProvider;
    },
  network_id: '*'
}
```

- At this point, your truffle.js file should look something like the following:

```javascript
// Initialize HDWalletProvider
const HDWalletProvider = require("truffle-hdwallet-provider");

const { readFileSync } = require("fs");
const path = require("path");
const { join } = require("path");

// Set your own mnemonic here
const mnemonic = "YOUR_MNEMONIC";

function getLoomProviderWithPrivateKey(
  privateKeyPath,
  chainId,
  writeUrl,
  readUrl
) {
  const privateKey = readFileSync(privateKeyPath, "utf-8");
  return new LoomTruffleProvider(chainId, writeUrl, readUrl, privateKey);
}

// Module exports to make this configuration available to Truffle itself
module.exports = {
  // Object with configuration for each network
  networks: {
    // Configuration for mainnet
    mainnet: {
      provider: function () {
        // Setting the provider with the Infura Rinkeby address and Token
        return new HDWalletProvider(
          mnemonic,
          "https://mainnet.infura.io/v3/YOUR_TOKEN"
        );
      },
      network_id: "1",
    },
    // Configuration for rinkeby network
    rinkeby: {
      // Special function to setup the provider
      provider: function () {
        // Setting the provider with the Infura Rinkeby address and Token
        return new HDWalletProvider(
          mnemonic,
          "https://rinkeby.infura.io/v3/YOUR_TOKEN"
        );
      },
      // Network id is 4 for Rinkeby
      network_id: 4,
    },

    basechain: {
      provider: function () {
        const chainId = "default";
        const writeUrl = "http://basechain.dappchains.com/rpc";
        const readUrl = "http://basechain.dappchains.com/query";
        const privateKeyPath = path.join(__dirname, "mainnet_private_key");
        const loomTruffleProvider = getLoomProviderWithPrivateKey(
          privateKeyPath,
          chainId,
          writeUrl,
          readUrl
        );
        return loomTruffleProvider;
      },
      network_id: "*",
    },
  },
};
```

#### Whitelist your deployment keys

- before deploying to Basechain, you need to whitelist your keys by following the instructions from our Deploy to Mainnet guide. Don't worry about it right now, but keep in mind that you have to do this after you finish this tutorial.

We've gone ahead and performed all these steps and now we are ready to deploy to Basechain!

```
truffle migrate --network basechain
```

## Build an Oracle

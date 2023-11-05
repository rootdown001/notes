# [JS Questions & Answers](https://github.com/sudheerj/javascript-interview-questions#javascript-interview-questions--answers)

## [1. Different ways to create objects in JS](https://github.com/sudheerj/javascript-interview-questions#what-are-the-possible-ways-to-create-objects-in-javascript)

```jsx
// object literal
const myObject1 = { name: "Lance", age: 55 };
console.log("🚀 ~ file: App.jsx:5 ~ App ~ myObject1:", myObject1);

// object constructor
const myObject2 = new Object();
myObject2.name = "Lance";
myObject2.age = 55;
console.log("🚀 ~ file: App.jsx:8 ~ App ~ myObject2:", myObject2);

// function constructor
function Person(name, age) {
  this.name = name;
  this.age = age;
}

const myObject3 = new Person("Lance", 55);
console.log("🚀 ~ file: App.jsx:19 ~ App ~ myObject3:", myObject3);

// Object.create

const myObject4 = Object.create(Object.prototype, {
  name: { value: "Lance", writable: true },
  age: { value: 55, writable: true },
});
console.log("🚀 ~ file: App.jsx:27 ~ App ~ myObject4:", myObject4);

// object using class

class Person2 {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const myObject5 = new Person2("Lance", 55);
console.log("🚀 ~ file: App.jsx:39 ~ App ~ myObject5:", myObject5);
```

Explaination of Object.create

### **Syntax of Object.create()**

The `Object.create()` method is used to create a new object and set the prototype of this object to another object. Its syntax is:

```javascript
Object.create(proto, [propertiesObject]);
```

- `proto`: This is the prototype object that the new object should be based on.
- `propertiesObject` (Optional): This is an object specifying the properties of the new object.

### **Explanation of Object.create()**

1. **Creating an object with a specific prototype:**
   You can create a new object with a specific prototype object. For example:

   ```javascript
   const proto = {
     greet: function () {
       console.log("Hello!");
     },
   };
   const obj = Object.create(proto);
   obj.greet(); // Output: Hello!
   ```

   In this example, `obj` is created with `proto` as its prototype, meaning `obj` has access to the `greet` method.

2. **Defining properties:**
   You can also define properties for the new object, specifying property attributes like `value`, `writable`, `enumerable`, and `configurable`.

   ```javascript
   const obj = Object.create(Object.prototype, {
     name: {
       value: "John",
       writable: true,
       enumerable: true,
       configurable: true,
     },
     age: {
       value: 30,
       writable: true,
       enumerable: true,
       configurable: true,
     },
   });

   console.log(obj.name); // Output: John
   ```

   In this example, `obj` is created with properties `name` and `age` each having specific attributes.

### **Use Cases of Object.create()**

- **Prototypal Inheritance:**
  You can use `Object.create()` for prototypal inheritance where an object inherits properties and methods from another object.

- **Creating Objects without a Prototype:**
  Creating objects that do not inherit from `Object.prototype` by passing `null` as the prototype.

  ```javascript
  const obj = Object.create(null);
  console.log(obj.toString); // Output: undefined
  ```

- **Creating Utility Objects:**
  Creating objects that include utility methods or properties that can be shared across multiple instances.

### **Benefits and Features**

- **Control Over Properties:**
  You have granular control over property attributes like writability, enumerability, and configurability.

- **Flexibility with Prototypes:**
  `Object.create()` offers flexibility in setting the prototype of an object, aiding in implementing inheritance and other design patterns.

### **Conclusion**

`Object.create()` is a versatile method that allows creating objects with a high level of control over their prototype and properties, making it crucial in various scenarios like implementing inheritance and creating utility or service objects in JavaScript applications.

## [2. What is a prototype chain](https://github.com/sudheerj/javascript-interview-questions#what-is-a-prototype-chain)

- prototype on object instance is available through Object.`getPrototypeOf(object)` or `__proto__` property
- prototype on constructors function is available through `Object.prototype`
- prototype chain means it looks at object, then if it needs to, it looks at that object's prototype, and so on. It ends with `Object.prototype`, which has its `[[Prototype]]` set to `null`
- various base object methods, like `toString()` or `hasOwnProperty()`, are found in `Object.prototype`, making them available to all objects

### **What is a Prototype in JavaScript?**

Every object in JavaScript has an internal property called `[[Prototype]]`, commonly referred to as the object's prototype. The prototype is itself an object, and it has its prototype, forming a chain of objects linked together. This chain is what we call the prototype chain.

### **How Does the Prototype Chain Work?**

When you try to access a property or method on an object, JavaScript will first look for that property/method directly on the object. If it doesn’t find it, JavaScript will look for it on the object’s prototype. If it’s still not found, JavaScript will continue looking up the prototype chain, checking the prototype of the prototype, and so on, until it reaches the end of the chain. If the property/method is not found in the entire chain, JavaScript will return `undefined`.

### **Example:**

```javascript
function Animal(name) {
  this.name = name;
}

Animal.prototype.makeSound = function () {
  console.log("Some generic sound");
};

function Dog(name) {
  Animal.call(this, name);
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
Dog.prototype.makeSound = function () {
  console.log("Bark");
};

const dog = new Dog("Buddy");
dog.makeSound(); // Output: Bark

console.log(dog.name); // Output: Buddy
```

### **Explanation of the Example:**

- An `Animal` function (constructor) is defined with a `name` property and a `makeSound` method on its prototype.
- A `Dog` function (constructor) is defined that calls the `Animal` constructor and has its own `makeSound` method.
- The `Dog` prototype is set to a new object created from the `Animal` prototype. This establishes the prototype chain: `Dog` prototype -> `Animal` prototype -> `Object.prototype`.
- A `Dog` object is created, and when `makeSound` is called on it, JavaScript finds `makeSound` on the `Dog` prototype, so "Bark" is output.
- The `name` property is not found on the `Dog` object or its prototype, but it’s found on the instance itself because it’s set by the `Animal` constructor.

### **End of the Prototype Chain:**

The end of the prototype chain is usually the `Object.prototype`, which has its `[[Prototype]]` set to `null`. Various base object methods, like `toString()` or `hasOwnProperty()`, are found in `Object.prototype`, making them available to all objects.

### **Conclusion:**

The prototype chain is a powerful feature in JavaScript, enabling prototypal inheritance, where objects can inherit properties and methods from other objects, allowing for code reuse and more dynamic and flexible architectures. Understanding the prototype chain is essential for working effectively with objects and inheritance in JavaScript.

## [3. Difference between `call`, `apply`, and `bind`](https://github.com/sudheerj/javascript-interview-questions#what-is-the-difference-between-call-apply-and-bind)

`call()`:

- `call()` invokes a function with a given `this` value and arguments provided one by one

  `call()` syntax

  ```javascript
  function.call(thisArg, arg1, arg2, ...)
  ```

  `call()` example

  ```javascript
  function greet(message) {
    console.log(`${message}, ${this.name}!`);
  }

  greet.call({ name: "Alice" }, "Hello"); // Output: Hello, Alice!
  ```

`apply()`:

- `apply()` Invokes the function with a given `this` value and allows you to pass in arguments **as an array**

  `apply()` syntax

  ```javascript
  function.apply(thisArg, [argsArray])
  ```

  `apply()` example

  ```javascript
  function greet(message) {
    console.log(`${message}, ${this.name}!`);
  }

  greet.apply({ name: "Bob" }, ["Hello"]); // Output: Hello, Bob!
  ```

`bind()`:

- `bind()` **returns a new function** (from original function with `this` value) allowing you to pass any number of arguments

  `bind()` syntax

  ```javascript
  const boundFunction = function.bind(thisArg, arg1, arg2, ...)
  ```

  `bind()` example

  ```javascript
  function greet(message) {
    console.log(`${message}, ${this.name}!`);
  }

  const greetAlice = greet.bind({ name: "Alice" }, "Hello");
  greetAlice(); // Output: Hello, Alice!
  ```

THEREFORE... all three set the `this` value of a function, and all get passed an object as the first argument (which becomes the value of `this` inside the function)

SO... `call()` & `apply()` INVOKE a function with a `this` value (being set from passed object), but `apply()` receives arguments AS AN ARRAY.

- Remember `call()` (COMMAS) and `apply()` (ARRAY)

BUT... `bind()` RETURNS A NEW function with a `this` value (being set from passed object)

## [4. What is JSON and its common operations](https://github.com/sudheerj/javascript-interview-questions#what-is-json-and-its-common-operations)

JSON is a text-based data format following JavaScript object syntax, which was popularized by Douglas Crockford. It is useful when you want to transmit data across a network and it is basically just a text file with an extension of .json, and a MIME type of application/json

Parsing: Converting a string to a native object

`JSON.parse(text);`

Stringification: converting a native object to a string so it can be transmitted across the network

`JSON.stringify(object);`

NOTE about MIME type:

A MIME type, also known as media type or content type, is a standard that indicates the nature and format of a document or a file. It helps browsers or other web technologies to properly handle and display or execute the content based on its type.

When you see `application/json`, it refers to a specific MIME type:

- `application`: This is the media type. It indicates that the content is suitable for or to be interpreted as a specific kind of application.

- `json`: This is the subtype. It indicates that the format or structure of the content is JSON (JavaScript Object Notation).

### **Context of Use**

1. **HTTP Headers**:
   In the context of HTTP headers (like in HTTP responses and requests), specifying the MIME type as `application/json` tells the client (usually a web browser) that the content should be processed and handled as JSON data.

```http
Content-Type: application/json
```

2. **Ajax or Fetch API**:
   When making AJAX requests or using the Fetch API to communicate with APIs or servers, specifying the MIME type as `application/json` indicates that the sent or received data is in JSON format.

```javascript
fetch("api/data", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify(data),
});
```

## [5. What is the purpose of the array slice method](https://github.com/sudheerj/javascript-interview-questions#what-is-the-purpose-of-the-array-slice-method)

The `slice()` method...

- returns the selected elements in an array as a NEW array object
- it selects the elements starting at the given start argument, and ends at the given optional end argument WITHOUT including the last element
- if you omit the second argument then it selects till the end
- does NOT mutate the original array (so used in React state a lot)
- returns a subset of original array

```javascript
let arrayIntegers = [1, 2, 3, 4, 5, 6];
let arrayIntegers1 = arrayIntegers.slice(0, 2); // returns [1,2]
let arrayIntegers2 = arrayIntegers.slice(2, 3); // returns [3]
let arrayIntegers3 = arrayIntegers.slice(4); //returns [5, 6]
let arrayIntegers5 = arrayIntegers.slice(5); //returns [6]
```

## [6. What is the purpose of the array splice method](https://github.com/sudheerj/javascript-interview-questions#what-is-the-purpose-of-the-array-splice-method)

The `splice()` method...

- adds or removes items to or from an array
- returns the removed item
- FIRST argument specifies the array position for insertion or deletion
- optional SECOND argument indicates the NUMBER OF ELEMENTS to be deleted
- each additional argument is added to the array

Therefore...

- it DOES mutate array
- unlike `slice()`, second argument is NUMBER OF ELEMENTS to remove
- RETURNS removed item

```javascript
let arrayIntegersOriginal1 = [1, 2, 3, 4, 5];
let arrayIntegersOriginal2 = [1, 2, 3, 4, 5];
let arrayIntegersOriginal3 = [1, 2, 3, 4, 5];

let arrayIntegers1 = arrayIntegersOriginal1.splice(0, 2);
console.log("🚀 ~ file: App.jsx:10 ~ App ~ arrayIntegers1:", arrayIntegers1);
// returns [1, 2]; original array: [3, 4, 5]

let arrayIntegers2 = arrayIntegersOriginal2.splice(3);
console.log("🚀 ~ file: App.jsx:15 ~ App ~ arrayIntegers2:", arrayIntegers2);
// returns [4, 5]; original array: [1, 2, 3]

let arrayIntegers3 = arrayIntegersOriginal3.splice(3, 1, "a", "b", "c");
console.log("🚀 ~ file: App.jsx:20 ~ App ~ arrayIntegers3:", arrayIntegers3);
//returns [4]; original array: [1, 2, 3, "a", "b", "c", 5]

console.log(
  "🚀 ~ file: App.jsx:3 ~ App ~ arrayIntegersOriginal1:",
  arrayIntegersOriginal1
); //  now [3, 4, 5]
console.log(
  "🚀 ~ file: App.jsx:5 ~ App ~ arrayIntegersOriginal2:",
  arrayIntegersOriginal2
); // now [1, 2, 3]
console.log(
  "🚀 ~ file: App.jsx:7 ~ App ~ arrayIntegersOriginal3:",
  arrayIntegersOriginal3
); // now [1, 2, 3, "a", "b", "c", 5]
```

## [7. What is the difference between slice and splice](https://github.com/sudheerj/javascript-interview-questions#what-is-the-difference-between-slice-and-splice)

- `slice()`

  - Doesn't modify the original array(immutable)
  - Returns the subset of original array as NEW array
  - Used to pick the elements from array

- `splice()`
  - Modifies the original array(mutable)
  - Returns the deleted elements as array
  - Used to insert or delete elements to/from array

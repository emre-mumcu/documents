# Different ways of writing functions in JavaScript

There are 3 ways of writing a function in JavaScript:

* Function Declaration
* Function Expression
* Arrow Function

Function Declaration: Function Declaration is the traditional way to define a function. It is somehow similar to the way we define a function in other programming languages. We start declaring using the keyword “function”. Then we write the function name and the parameters.

Function declarations are hoisted.
These are executed before any other code. The function in function declaration can be accessed before and after the function definition.

```js
// Function declaration 
    function add(a, b) {         
        console.log(a + b);
    }
      
    // Calling a function
    add(2, 3);
```

Function Expression: Function Expression is another way to define a function in JavaScript. Here we define a function using a variable and store the returned value in that variable.

Function expressions are not hoisted.
A function Expression is similar to a function declaration without the function name. Function expressions load and execute only when the program interpreter reaches the line of code. The function in function expression can be accessed only after the function definition.

```js
// Function Expression
    const add = function(a, b) {
        console.log(a+b);
    } 
      
    // Calling function
    add(2, 3);
```

 Here, the whole function is an expression and the returned value is stored in the variable. We use the variable name to call the function.

 Arrow Functions: Arrow functions are been introduced in the ES6 version of JavaScript. It is used to shorten the code. Here we do not use the “function” keyword and use the arrow symbol.

 ```js
// Single line of code
    let add = (a, b) => a + b; 
      
    console.log(add(3, 2));
```

Note: When there is a need to include multiple lines of code we use brackets. Also, when there are multiple lines of code in the bracket we should write return explicitly to return the value from the function.

 ```js
// Multiple line of code
    const great = (a, b) => {
        if (a > b) 
            return "a is greater";
        else
            return "b is greater";
    }
      
    console.log(great(3,5));
```

# Defining functions


Broadly speaking, JavaScript has four kinds of functions:

* Regular function: can return anything; always runs to completion after invocation
* Generator function: returns a Generator object; can be paused and resumed with the yield operator
* Async function: returns a Promise; can be paused and resumed with the await operator
* Async generator function: returns an AsyncGenerator object; both the await and yield operators can be used

For every kind of function, there are three ways to define it:

* Declaration: function, function*, async function, async function*
* Expression: function, function*, async function, async function*
* Constructor: Function(), GeneratorFunction(), AsyncFunction(), AsyncGeneratorFunction()

Classes are conceptually not functions (because they throw an error when called without new), but they also inherit from Function.prototype and have typeof MyClass === "function".

```js
// Constructor
const multiply = new Function("x", "y", "return x * y");

// Declaration
function multiply(x, y) {
  return x * y;
} // No need for semicolon here

// Expression; the function is anonymous but assigned to a variable
const multiply = function (x, y) {
  return x * y;
};
// Expression; the function has its own name
const multiply = function funcName(x, y) {
  return x * y;
};

// Arrow function
const multiply = (x, y) => x * y;

// Method
const obj = {
  multiply(x, y) {
    return x * y;
  },
};
```

# NOTES:

* The Function() constructor, function expression, and function declaration syntaxes create full-fledged function objects, which can be constructed with new. However, arrow functions and methods cannot be constructed.
* The function declaration creates functions that are hoisted. Other syntaxes do not hoist the function and the function value is only visible after the definition.
* The arrow function and Function() constructor always create anonymous functions, which means they can't easily call themselves recursively. One way to call an arrow function recursively is by assigning it to a variable.
* The arrow function syntax does not have access to arguments or this.
* The Function() constructor cannot access any local variables — it only has access to the global scope.
* The Function() constructor causes runtime compilation and is often slower than other syntaxes.

For function expressions, there is a distinction between the function name and the variable the function is assigned to. The function name cannot be changed, while the variable the function is assigned to can be reassigned. The function name can be different from the variable the function is assigned to — they have no relation to each other. The function name can be used only within the function's body. Attempting to use it outside the function's body results in an error (or gets another value, if the same name is declared elsewhere). For example:

```js
const y = function x() {};
console.log(x); // ReferenceError: x is not defined
```

## Arrow function expressions

An arrow function expression is a compact alternative to a traditional function expression, with some semantic differences and deliberate limitations in usage:

* Arrow functions don't have their own bindings to this, arguments, or super, and should not be used as methods.
* Arrow functions cannot be used as constructors. Calling them with new throws a TypeError. They also don't have access to the new.target keyword.
* Arrow functions cannot use yield within their body and cannot be created as generator functions.

```js
// SYNTAX

param => expression

(param) => expression

(param1, paramN) => expression

param => {
  statements
}

(param1, paramN) => {
  statements
}


```

Rest parameters, default parameters, and destructuring within params are supported, and always require parentheses:

```js
(a, b, ...r) => expression
(a = 400, b = 20, c) => expression
([a, b] = [10, 20]) => expression
({ a, b } = { a: 10, b: 20 }) => expression

```

Arrow functions can be async by prefixing the expression with the async keyword.

```js
async param => expression
async (param1, param2, ...paramN) => {
  statements
}
```

### Description
Let's decompose a traditional anonymous function down to the simplest arrow function step-by-step. Each step along the way is a valid arrow function.

```js
// Traditional anonymous function
(function (a) {
  return a + 100;
});

// 1. Remove the word "function" and place arrow between the argument and opening body bracket
(a) => {
  return a + 100;
};

// 2. Remove the body braces and word "return" — the return is implied.
(a) => a + 100;

// 3. Remove the parameter parentheses
a => a + 100;
```

```js
// Traditional anonymous function
(function (a, b) {
  return a + b + 100;
});

// Arrow function
(a, b) => a + b + 100;

const a = 4;
const b = 2;

// Traditional anonymous function (no parameters)
(function () {
  return a + b + 100;
});

// Arrow function (no arguments)
() => a + b + 100;

```
The braces can only be omitted if the function directly returns an expression. If the body has additional lines of processing, the braces are required — and so is the return keyword. Arrow functions cannot guess what or when you want to return.

```js
// Traditional anonymous function
(function (a, b) {
  const chuck = 42;
  return a + b + chuck;
});

// Arrow function
(a, b) => {
  const chuck = 42;
  return a + b + chuck;
};
```

Arrow functions are always unnamed. If the arrow function needs to call itself, use a named function expression instead. You can also assign the arrow function to a variable so it has a name.

```js
// Traditional Function
function bob(a) {
  return a + 100;
}

// Arrow Function
const bob2 = (a) => a + 100;
```

### Function body
Arrow functions can have either a concise body or the usual block body.

In a concise body, only a single expression is specified, which becomes the implicit return value. In a block body, you must use an explicit return statement.


```js
const func = (x) => x * x;
// concise body syntax, implied "return"

const func2 = (x, y) => {
  return x + y;
};
// with block body, explicit "return" needed
```

Returning object literals using the concise body syntax (params) => { object: literal } does not work as expected. 

This is because JavaScript only sees the arrow function as having a concise body if the token following the arrow is not a left brace, so the code inside braces ({}) is parsed as a sequence of statements, where foo is a label, not a key in an object literal.

To fix this, wrap the object literal in parentheses:

```js
const func = () => { foo: 1 };
// Calling func() returns undefined!

const func2 = () => { foo: function () {} };
// SyntaxError: function statement requires a name

const func3 = () => { foo() {} };
// SyntaxError: Unexpected token '{'


// OK!!
const func = () => ({ foo: 1 });
```

# What do parentheses surrounding an object/function/class declaration mean?

It is a self-executing anonymous function. The first set of parentheses contain the expressions to be executed, and the second set of parentheses executes those expressions.
The parenthesis are only required in this context because function in JavaScript can be both a statement or an expression, depending upon context. The parenthesis force it to be an expression.

```js
(function() {
    //...
})();

```
The following is also valid: var x = function () { return 1 }() even though there are no surrounding parenthesis. Just a function-expression and the application (...) of said expression. In a case I this, I would recommend putting a ; before the opening parenthesis: I write semi-colon free code and lines starting with a ( may be parsed as expression continued from the previous line.


Specifically, it makes JavaScript interpret the 'function() {...}' construct as an inline anonymous function expression. If you omitted the brackets:

function() {
    alert('hello');
}();
You'd get a syntax error, because the JS parser would see the 'function' keyword and assume you're starting a function statement of the form:

function doSomething() {
}
...and you can't have a function statement without a function name.














https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions

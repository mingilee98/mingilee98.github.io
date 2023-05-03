---
title: "[JavaScript] Basics"
categories:
  - Web Development
tags:
  - JavaScript
---

# Table of Contents
1. [Variables](#variables)
2. [Numbers](#numbers)
3. [Strings](#strings)
4. [Other Data Types](#other-data-types)
5. [Strict vs Loose Equality](#strict-vs-loose-equality)
6. [Conditional](#conditional)

## Learning JavaScript
In this post, we are going to learn about the brief basics of JavaScript.

## Variables
There are three ways to declare variables in JavaScript. 
- **var**
- **let**
- **const**

**var** is a relatively old way of declaring a variable, so we will stick to **let** and **const** in this post.

Just like other languages, variables let us store information.

```javascript
let userName = 'hello';
let user = 'John', age = 25, userName = 'Hello';
```

We commonly use camelCase for the names of variables.

We can use **const** to declare constant variables.

```javascript
const COLOR_RED = "#F00";
const COLOR_GREEN = "#0F0";
const COLOR_BLUE = "#00F";
```

Notice that we cannot reassign variables declared with **const**.

It is a convention to use all uppercase for constants calculated before runtime and use normal camelCase for constants calculated after runtime.

## Numbers

### Integer
JavaScript has only one type of number. Numbers can be written with or without decimals.

```javascript
let x = 3.14;    // A number with decimals
let y = 3;       // A number without decimals 
let x = 123e5;    // 12300000
let y = 123e-5;   // 0.00123 
```

Unlike other programming languages like C, JavaScript does not define different types of numbers like int or float.

JavaScript numbers are always stored as double-precision floating point numbers, following the international IEEE 754 standard. 

This format stores numbers in 64 bits, where the number (the fraction) is stored in bits 0 to 51, the exponent in bits 52 to 62, and the sign in bit 63.

The range of safe integers is $-2^{53}-1$ ~ $2^{53}-1$

### Floats
Like other languages, Floating point arithmetic is not always 100% accurate.

```javascript
let x = 0.2 + 0.1; // yields 0.30000000000000004 
```

### Numeric Strings

JavaScript will try to convert strings to numbers in all numeric operations.

```javascript
let x = "100";
let y = "10";
let z = x / y; // yields 10
```

### NaN
**NaN** is a JavaScript reserved word indicating that a number is not a legal number.

You can use the global JavaScript function **isNaN()** to find out if a value is a not a number:

```javascript
let x = 100 / "Apple"; // yields NaN
isNaN(x); // yields true
```

### Infinity
Infinity (or -Infinity) is the value JavaScript will return if you calculate a number outside the largest possible number.

Division by 0 (zero) also generates Infinity.

```javascript
// yields infinity
let myNumber = 2;
while (myNumber != Infinity) {
  myNumber = myNumber * myNumber;
} 

// yields infinity
let x =  2 / 0;
```

Notice that Infinity is also a number in JavaScript
```javascript
typeof Infinity; // returns number
```

### Unary +
Unary plus converts a non-number into a number
```javascript
// Converts non-numbers
alert( +true ); // 1
alert( +"" );   // 0
let apples = "2";
alert( +apples ); // 2
```
## Strings
We can create Strings like the following example

```javascript
const string = "The revolution will not be televised.";
```

Notice that both double and single quotes will work as long as they are consistent.

### Concatenating strings

To join together strings in JavaScript you can use a different type of string, called a **template literal**.

we can create template literals with backtick characters (`).

With template literals, we can include variables in it wrapped with **${ }**

```javascript
const name = "Chris";
const greeting = `Hello, ${name}`;
console.log(greeting); // "Hello, Chris"

const one = "Hello, ";
const two = "how are you?";
const joined = `${one}${two}`;
console.log(joined); // "Hello, how are you?"

// we can also include expressions
const output = `I gave it a score of ${(score / highestScore) * 100}%.`; 
```

Template literals respect the line breaks in the source code, so you can write strings that span multiple lines like this:

```javascript
const output = `I like the song.
I gave it a score of 90%.`;
console.log(output);

/*
I like the song.
I gave it a score of 90%.
*/
```

You can also concatenate strings using the + operator:
```javascript
const greeting = "Hello";
const name = "Chris";
console.log(greeting + ", " + name); // "Hello, Chris"
```

## Other Data Types
There are eight basic data types in JavaScript.

- **String**
- **Number**
- **BigInt**
- **Boolean**
- **Undefined**
- **Null**
- **Symbol**
- **Object**

Let's briefly learn about data types other than **String** and **Number**

### BigInt
For numbers that is out of the Integer range ($-2^{53}-1$ ~ $2^{53}-1$), we can use BigInt.

```javascript
// the "n" at the end means it's a BigInt
const bigInt = 1234567890123456789012345678901234567890n;
```

### Boolean
The boolean type has only two values: true and false.

```javascript
let nameFieldChecked = true; 
```

### Null
The special null value does not belong to any of the types described above.

In JavaScript, null is **not** a “reference to a non-existing object” or a “null pointer” like in some other languages.

It’s just a special value which represents “nothing”, “empty” or “value unknown”.

It forms a separate type of its own which contains only the null value:

```javascript
let age = null;
```

### Undefined
The special value undefined also stands apart. It makes a type of its own, just like null.

The meaning of undefined is “value is not assigned”.

If a variable is declared, but not assigned, then its value is undefined:

```javascript
let age;
alert(age); // shows "undefined"
```

### Objects
Objects are used to store collections of data and more complex entities.

We will learn about Objects in detail later.

## Strict vs Loose Equality
### Double Equals (==) 
There are two types of equality in JavaScript.

Double equals (==) is often referred to as 'loose equality' because it performs type coercion before making any comparison.

Let's look at an example.

```javascript
const a = 100;
const b = '100';

console.log(a == b) // true
```

We can see that the types of the constant variables are different. 

However, **a == b** will still return true because the variable a is 
converted to a string before making the comparison.

### Triple equals (===)

Triple equals (===), also referred to as "strict equality", works similarly to how double equal works, with one important difference: it does not convert the types of the operands before comparing.

Let's look at an example
```javascript
const a = 100;
const b = '100';

console.log(a === b); // false
```

There is also a “strict non-equality” operator **!==** analogous to **!=**.

## Conditional

### The if and else Statements
The syntax for if and else statements in JavaScript is 

```javascript
if (condition1) {
  //  block of code to be executed if condition1 is true
} else if (condition2) {
  //  block of code to be executed if the condition1 is false and condition2 is true
} else {
  //  block of code to be executed if the condition1 is false and condition2 is false
}
```

### The switch Statement
The syntax for switch statement in JavaScript is

```javascript
switch (expression) {
	case x:
		// execute case x code block
		break;
	case y:
		// execute case y code block
		break;
	default:
		// execute default code block
}
```
### Boolean Conversion
The if statement evaluates the expression in its parentheses and converts the result to a boolean.

- **A number 0, an empty string "", null, undefined, and NaN all become false**
- **Other values become true.**
<br>

> ##### Reference
>https://www.theodinproject.com
>https://www.w3schools.com/js/js_numbers.asp
>https://javascript.info/variables
>https://javascript.info/operators
>https://www.freecodecamp.org/news/loose-vs-strict-equality-in-javascript/
>https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/Strings#concatenating_strings
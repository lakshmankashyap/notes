# TypeScript

TypeScript makes javascript which is a dynamically typed language into a language that is static typed.

## Dynamic typed language

- Variables are not bound to a particular type
- Type checking is done during runtime
- Easier to read
- Add a layer of complexity

## Static typed language

- We have to say what is the type of a variable
- We have to declare a variable before using it.
- Type checking is done during compile time
- We get documentation with types
- We get less bugs : if it fails at compile time, we catch them early instead of having them in production
- Slower development

Static/Dynamic Typing is about when type information is acquired (Either at compile time or at runtime)

Strong/Weak Typing is about how strictly types are distinguished (e.g. whether the language tries to do an implicit conversion from strings to numbers).

## TypeScript basics

### Useful commands

- `tsc --init`
- `tsc file.ts --watch`

### Types

```ts
// boolean
let isCool: boolean = false;

// number
let age: number = 56;

// string
let eyeColor: string = "brown";
let favoriteQuote: string = `I'm not old, I'm only ${age}`; // Works with string interpolation

// Array
let pets: string[] = ["cat", "mouse", "dragon"];
let pets2: Array<string> = ["pig", "lion", "dragon"];

// Tuple
let basket: [string, number];
basket = ["basketball", 10];

// Enum
enum Size {
  Small = 1,
  Medium,
  Large
}
let sizeName: string = Size[2];
alert(sizeName); // Displays 'Medium' as its value is 2 above

// any : use with caution!
let whatever: any = "aaaaghhhhhh noooooo!";

// void : nothing return
let sing = (): void => console.log("Lalalala");

// null and undefined
let meh: undefined = undefined;
let noo: null = null;

// never
// never type tests two things : a function never returns and always throws

/* Difference between void and never

A function that doesn't explicitly return a value implicitly returns the value undefined in JavaScript.
Although we typically say that such a function "doesn't return anything", it returns. 
We usually ignore the return value in these cases. 
Such a function is inferred to have a void return type in TypeScript.

A function that has a never return type never returns. 
It doesn't return undefined, either. 
The function doesn't have a normal completion.
It means that it throws an error or never finishes running at all.
*/
let error = (): never => {
  throw Error("blah!");
};

// Type Assertions : can be dangerous too sometimes!
// More about type assertions : https://basarat.gitbooks.io/typescript/docs/types/type-assertion.html
let ohhithere: any = "OH HI THERE";
let strLength: number = (ohhithere as string).length;

// interface
// Useful for React and props
// Difference between interface and type :
// https://medium.com/@martin_hotell/interface-vs-type-alias-in-typescript-2-7-2a8f1777af4c
interface RobotArmy {
  count: number;
  type: string;
  magic?: string; // Optional property : can or cannot be there
}

let fightRobotArmy = (robots: RobotArmy) => {
  console.log("FIGHT!");
};

// The function below is exactly the same as above
let fightRobotArmy2 = (robots: {
  count: number;
  type: string;
  magic?: string;
}) => {
  console.log("FIGHT!");
};

// Function
let fightRobotArmyF = (robots: RobotArmy): void => {
  console.log("FIGHT!");
};
let fightRobotArmy2F = (robots: {
  count: number;
  type: string;
  magic?: string;
}): void => {
  console.log("FIGHT!");
};

// Classes
class Animal {
  private sing: string;
  constructor(sound: string) {
    this.sing = sound;
  }
  greet() {
    return "Hello, " + this.sing;
  }
}

let lion = new Animal("Lion");
// lion.sing won't work because sing is a private property

//In TypeScript, there are several places where type inference
//is used to provide type information when there is no explicit
//type annotation. For example, in this code
let x = 3;
// automatically detects x is a number.

// Union Type : it can be "this" or "that" or ...
let confused: string | number = "hello";
```

### DefinitelyTyped

#### Declaration files

The concept of declaration files is analogous to the concept of header file found in C/C++.

Declaration files are a way to describe the shape of JavaScript values to the TypeScript compiler. In other words : it is a mechanism for supplying type information about JavaScript to TypeScript.

They end with the `d.ts` extension.

#### How to get these types ?

With [DefinitelyTyped](https://definitelytyped.org/) ! It is a simple repository on GitHub that hosts TypeScript declaration files for all your favorite packages.

So to work with 3rd party libraries, we would do for example :

```
npm install --save @types/jquery
npm install --save @types/express
```

#### How to use them ?

**Before TypeScript 2**

```
/// <reference path="path_to_declaration_file">
```

**After TypeScript 2**

- TypeScript automatically knows where to find the declaration files.
- Even though triple slash directive is not needed, there is also a form that could be used. It takes the form of `/// <reference types="name_of_library">`
- `typeRoots` and `types`ﬂ are the two properties of `tsconfig.json` that can be used to configure the behavior of the type declaration resolution.

Learn more [here](http://www.geekabyte.io/2017/10/understanding-declaration-files-in.html)

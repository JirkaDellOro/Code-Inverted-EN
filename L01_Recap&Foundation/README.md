# L01 Recap & Foundation
<a href="https://www.youtube.com/watch?v=BRTOlPdyPYU"><img src="Material/steve-jobs-learn-to-program.jpg">
<small>Source: <a href="https://www.challengecharterschool.net/wp-content/uploads/2017/07/Steve-Jobs.jpg">https://www.challengecharterschool.net/wp-content/uploads/2017/07/Steve-Jobs.jpg</a></small>

## Introduction
In this chapter, you will solidify some fundamental and familiar concepts and likely learn a few new ones as well. A deep understanding of these, and the ability to use them confidently, is a prerequisite for the subsequent chapters. You will also be introduced to the activity diagrams of the "Unified Modeling Language" (UML 2.5), which you will use later in the course to describe and design algorithms. Furthermore, you will learn to use trace tables and the browser's built-in debugger—important tools for troubleshooting during the implementation of the applications you have designed.

## Errors
> "Aargh! My program is no longer running, even though I didn't do anything!"  

We all know that feeling, you are not alone. Pay attention to the following rules:
1. don't panic!
1. the computer is rather uncreative and simply follows your instructions. There is likely nothing wrong with it or with other software, such as the browser, and it can help you locate the error. It is tireless in this regard and won't hold anything against you.
1.  Gather information about the problem—don't just randomly change things. Instead, examine the behavior. Make use of the numerous helpful features the computer can display for you. Apply the [solution strategies!](#l%C3%B6sungsstrategien)
2. If you haven't solved the problem after 15 minutes, call the lecturer or a tutor! Otherwise you will lose too much time… if there is no one in the room, write an issue. Post the gathered information and links to your program, both on "Pages" for trying things out and on the code. Important: You need to be clear about exactly what you were planning to do. Have your concept ready!

>**Note:** One of TypeScript’s major strengths is error prevention, by flagging many errors at development time. JavaScript has almost nothing to offer here; the errors only show up during operation—especially at the user's end!

### Error types
You have now encountered a wide variety of errors. Naming them correctly helps others help you. An important criterion for distinction is the point in time at which the error occurs.
#### Runtime Error
Occurs at runtime. This is a nasty type of error, as it may go undetected for a long time or be reproducible only under certain conditions. Runtime errors are displayed in the browser console and must be addressed, even if the program continues to run.
#### Compiletime Error
Occurs when the TypeScript program is compiled into a JavaScript program. You will largely be spared these errors, at least initially—provided you set up your project properly. This error type will be shown in the compilers console, usually in the VSCode-terminal. 
#### Designtime Error
These are errors that are flagged while you are writing the code—specifically, right within the code itself. In VSCode red wavy lines in the text and red boxes in the margin are displayed for this. the "problem"-tab in the console will get filled with those. This allows you to react immediately and experiment quickly to fix the error. In addition, error descriptions are displayed when hovering the mouse, and solutions are even suggested via QuickFix.
#### Logical Error
Logical errors are not attributable to incorrect programming, but rather to poor design. The computer usually cannot even point out these errors, because it doesn't know what you actually intended to do. As long as the program is executable and none of the other error types occur, it will execute it and assume that it is doing what you intended. The only thing that helps here is thinking it through, but there are some tools you can use!

### Solution Strategies
1. Rule out the "classics" we all get hung up on—such as... 
   - not compiled
   - Script not linked in the HTML file, or a TypeScript file linked instead of a JavaScript file.
   - Parentheses forgotten in a function call or included in a function reference
   - Tested the wrong file, or one in the wrong folder, or a copy
1. When dealing with design-time and compile-time errors, pay close attention to the hints TypeScript provides; they often contain an accurate description of the problem. Ask if you don't understand them.
1. Use the [Debugger](#debugger). Set a Breakpoint at the spot the error occurs in your program and reload it. Check the values ​​of the variables just before the error occurs. Helps primarily with runtime errors.
1. Commenting out and uncommenting: Convert individual lines into comments one after the other (Windows: Strg+#, Mac: ⌘+K ⌘+U) and observe how the program behavior changes. Or turn entire blocks of code into a comment and uncomment individual lines one by one to turn them back into code. See when the error appears or disappears.
1. Generate useful Output at various points using `console.log(...)`. Based on the sequence of outputs, you can identify the program execution flow and, if applicable, specific memory states.
1. Trace the program execution with the debugger, This allows you to track down even the most stubborn bugs. Use Step-In, Step-Over, and Step-Out.
1. Use the **Rubber Duck Method** for logic problems—also used by professionals. Not a Joke! Place a rubber duck nearby, a stuffed animal or a roommate will do, and look at your code together. Explain to them in detail how your code works. There is a good chance that you will find the logical error together. Rubber ducks are very smart!

### Preventive Measures
You can ensure while coding that errors don't occur in the first place, or that you can quickly find their causes.
- Adhere strictly to the coding guidelines, see the [Booklet](../X01_Appendix/)
- Test a lot! Make only small changes at a time and run the program again. That way, you know that the error was just recently introduced.
- Always have the browser console open for that. As with an illness, some errors do not appear serious, and the program initially continues to run. However, they must be treated!
- Before implementing a function, implement the call and a stub consisting of placeholder code—perhaps just a console output. Then you know the overall structure is good and can implement the details. 
- Make frequent commits in Git with meaningful messages and push them to your remote repository. You can actually go back and test previous versions later, and trace all the changes. This makes it possible to determine exactly when an error was introduced and quickly narrow down the cause.

> **Attention:** Never adapt your concept to your coding skills. Instead, expand your skills and implement your concept. If you have doubts: ask!

## Insight
To put it very simply, a computer cannot actually do much more than read information from memory, combine it with other information—for instance, by means of a mathematical operation—and store the result back in memory. The code
```typescript
v = v + 1;
```
looks up the value stored at the memory location designated by `v` , adds the literal value `1` to it, and stores the result back at location `v`.

>**Attention:** when using the assignment operator `=` the expression on the right-hand side is evaluated first, and the result is assigned to the left-hand side.  

## Variables
The memory location designated by `v` can therefore hold different values, its value is variable. To simplify matters, one usually speaks simply of the “variable `v`". For the program above to run, this variable must therefore exist in the first place.

- [x] Open a browser tab—or any webpage (e.g., hs-furtwangen.de)—and open the console within it. Tap in the line of code shown above and press the Enter key, you should be able to trigger a ‘ReferenceError’ this way.  

A variable is created using the keyword `let`. This process is called **Declaration**.
```typescript
let v;
```

- [x] Write into the browser console `let v;`. What happens now? Using the arrow keys, you can now recall the line you typed earlier and execute it again. What is the result?

NaN means "Not a Number", so the result is not a number. This is because `v` exists, but had the value `undefined`. The result of `undefined + 1` is certainly not a number but `NaN`.  
What is still missing is the **Definition** of `v` so that this variable has a value before it is used in a calculation. This is easily achieved using the assignment operator and a literal value.

>**Attention:** 
> Very important: `=` does not mean "is equal to" as you know it from math; instead, it is an instruction to evaluate the right-hand side and assign the result to the left-hand side. Hence the name "assignment operator".

- [x] Write `v = 1` into the browser console and let the program line run again. What happens now?  

The complete program, including a line for console output, therefore looks like this:
```typescript
let v = 1;
v = v + 1;
console.log(v);
```  

- [x] Create the program using TypeScript in VS Code and test it in the browser.

>**Attention:** 
> - You now know what declaration and definition mean, and you understand the difference between them.
> - Declaration and definition can be written on a single line.
> - In addition to `let` the keyword `var` is also used—and was used especially in the past—though it behaves somewhat differently. We consistently use `let`! 
> - There is also `const`, where the definition must take place alongside the declaration. This allows you to specify that the variable's value cannot be changed subsequently—making it a constant. Any attempt to modify it will trigger an error message.

- [x] Try the program temporarily using `const` instead of `let` and see what happens.

## Data types
Computers in use today store information as long sequences of bits—the smallest units of information, which can have only two states: on and off. Yet, these sequences are used to encode widely varying types of information: numbers, text, images, music, video, and so on. 

For the computer to process this information correctly, however, it must know what type of data a given bit sequence represents.

- [x] Hover your mouse cursor over `v` in the program you entered earlier. What appears in the tooltip?

TypeScript infers from the context that `v` should be of type `number`. This process is called “inference”.

- [x] Put the `1` in quotation marks in the initial definition of `v`. What type is displayed now? What is the output when the program runs?

TypeScript has recognized that `v` is now intended to represent a string, even though it contains only a single character—the digit 1. However, this has also changed the program logic, because the addition operator `+` is defined quite differently for strings than it is for numbers.

- [x] Define `v` using the keyword `true`. Which type is displayed now? What else is visible? And what happens when you run the program? (assuming it compiled at all… check the resulting JS file, it should be identical)

>**Attention:** You realize that data types are crucial and can alter the entire logic of a program! However, JavaScript handles them quite loosely; unless you are very careful, it can actually change data types while the program is running—which is why so many errors occur. TypeScript provides a solution…

As convenient as it is that TypeScript can infer types from the context and subsequently flag type errors: it is even better to consider the desired data types—and define them—right at the design stage!  
 Therefore, **explicit annotation** of the types is mandatory in this course. This means that, during declaration, the required type must be written after the variable identifier, separated by a colon. A complete declaration and definition with type annotation therefore follows this pattern:
`let/const name: type = value;`

## Primitive Data Types
Programming languages ​often feature a large number of fundamental data types that one needs to be familiar with. In TS/JS, this is greatly simplified, and the only ones relevant to us are the three primitive types
- `number`,
- `string` and 
- `boolean`  

- [x] Annotate `v` in the declaration as `number`. How does the code display change now?

Assigning anything other than a numerical value is no longer permitted at all; you are alerted to this while you are still writing the program. 

- [x] Now replace `true` with the number `1` again, but change the addend in line 2 to a string, so that the program now looks like the one below.  

```typescript
let v: number = 1;
v = v + "1";
console.log(v);
```
`v` is of the type `number` and it should be assigned the result of adding a string and a number (the current value of `v`).
- [x]  Why doesn't this work, and why did something similar work earlier when `v` was defined as `"1"`?

## Complex Data Types
In addition to simple data types such as `number`, `boolean` and `string` there are also complex data types. You can simply think of these as collections of information. This information can be of a primitive type, but can also itself be of a complex type. In the second case, you have a collection of collections. You can nest this to any depth.  
A distinction can be made between homogeneous data structures, in which the managed information units are all of the same type, and heterogeneous ones, in which different data types are managed. A further distinction is made based on the different ways in which information is addressed. This can be done via an index—that is, a number indicating the position of the information within a sequence—or via a key that is associated with the information and serves as the means of access. Typically, this key is of type `string`, though other types are also possible. Finally, a distinction can be made between predefined keys and those generated at runtime.
### Array
An array is a data structure in which the contained information—referred to as elements—is addressed using indices. In JavaScript, arrays are inherently heterogeneous. An array can be created using the `[ ]` or `new Array()`. A comma-separated list of elements can be specified within the brackets at the time of creation.

- [x] Write `let a = [7, true, "Hello"]` into the console. Display `a` and try to access individual elements using the syntax `a[index]` replacing `index` with a number. What happens if you use a number less than 0 or greater than 2?
- [x] Enter `a[4] = [101, 102]`. What do you see now when you output the array—specifically at a[3] and a[4]?

So, you can throw all sorts of things into the array at any position—even other arrays (resulting in the aforementioned collection of collections). With TypeScript, however, the range of element types can be restricted to ensure homogeneity, thereby reducing a significant source of errors.
```typescript
let a: number[] = [7, true, "Hello"];
```
In the example above, an error is displayed immediately because the values ​​at positions 1 and 2 are not of type `number`. Only the 7 at position 0 is acceptable.

### Associative Array
In an associative array, elements are linked to so-called keys. Such an associative array is typically created using curly braces, with key-value pairs able to be specified directly within the braces. The association is represented by the colon `:`.

- [x] Now write  `let s = {"zahl": 7, "wahr": true, text: "Hello"}` into the console. Output  `s` and try to access individual elements using bracket syntax `s["key"]` and dot syntax `s.key` , where you replace `key` with the string representing the key. What happens if you use an unknown key here?
- [x] Enter `s[4] = [101, 102]`. What do you see now when you display the array? What is the significance of the number 4 now, and how do you specifically access the information associated with it?

### Type definitions and aliases
In TypeScript, it is possible to define new data types. This allows you to declare variables that can hold multiple data types without having to completely abandon explicit typing and resort to the `any` type, which would drag you down to the level of JavaScript. `prompt(...)` for example, returns a result of type `string` or `null`if the dialog is cancelled. To assign the result to a variable, the variable should therefore be of a type that combines both. This is a so-called union type, and it is coded by simply combining the types using the operator | . In the example, it can look like this:
```typescript
let input: string | null = prompt("Enter a number between 1 and 10", "5");
```
> **Notice:** If you hover the mouse cursor over a function name in VSCode, the signature—and thus the return type—is displayed. You can simply copy and use it.
 
To prevent such complex types from disrupting the flow of reading, aliases should be used. For the example above, an alias could be defined as follows:
```typescript
type Input = string | null;
let input: Input = prompt("Enter a number between 1 and 10", "5");
```
The new `Input` type can now also be easily used elsewhere in the program, instead of repeating the combination.

Type definitions are particularly helpful when structuring complex data. This is because not only can the element types be heterogeneous, but—in the case of associative arrays, for example—the keys themselves can be chosen arbitrarily. This allows for great flexibility, but also for errors that are difficult to detect. Type definitions make it possible to predefine keys and restrict the data types for values ​​and keys. 

In the following example, the keys are predefined and the associated values ​​must be of specific types.
```typescript
type VectorWithMeaning = {x: number, y: number, meaning: string};
let vector: VectorWithMeaning = {x: 12.4, y: -7.2, meaning: "Position Vector"};
```    
In the next example, however, the keys can be chosen freely but are restricted to the type `string`, and the array is homogenized to boolean values.
```typescript
type MapStringToBoolean = {[key: string]: boolean};
let a: MapStringToBoolean = {"wert1": true, "wert2": false};
```  
With the help of this definition, many errors can be ruled out already at design time, and VS Code can, where applicable, already offer the correct keys for selection.

Both variants can also be combined. The following example shows a structure that could be used to manage the grade records of many students, along with an initial dataset for it.
```typescript
type Student = {name: string, matrikel: number,
    grades: {[module: string]: number}
};
let students: Student[] = [];

students.push({name: "Big Brain", matrikel: 123456, grades: {"Code1": 1.3, "Code2": 1.0}});
```    

>**Notice:** With the help of `type`, you can create your own complex data types.

- [x] Implement the examples and experiment with them.
- [x] Take a look at the compiled JavaScript code and what remains of your types. Consider what this has to do with the difference between JavaScript and TypeScript.

## Function
A function is a piece of code within a program that constitutes a small program in its own right; it has its own name and can process input data and generate output data. Such a function can then be used by other parts of the program by calling its name along with the data to be processed. If the function returns data, it can, for example, simply be assigned to a variable using the assignment operator `=` and is thus available for further processing.  
You define a function using the keyword `function`, followed by a name of your choice and a list of parameters in parentheses. Together, this constitutes the so-called signature. Then follows the function body, containing the actual code, enclosed in curly braces. If the function is to return a value, it is specified after the keyword `return`.

- [x] Write  
`function isDivisible(dividend, divisor) {return (dividend % divisor == 0)}`
  into the console.
- [x] Now write `isDivisible(4, 3)`, what result is displayed?
- [x] Now write `isDivisible(4, 2)`, what result do you get now?
- [x] Experiment with other numbers.
- [x] Explain how the function `isDivisible` works. To do this, use the “Operators” chapter in the [Booklet](../X01_Appendix)

This JavaScript function is very error-prone, for instance, it returns a result even if you call `isDivisible(false, true)` which makes no sense. Written properly, following our style guidelines and in somewhat greater detail using TypeScript, the function looks like this:

```typescript
  function isDivisible(_dividend: number, _divisor: number): boolean {
    let result: boolean = (_dividend % _divisor == 0);
    return result;
  }
```  
Here, each of the parameters has been assigned a type annotation, so TypeScript will complain if the function is called with values ​​of a type other than `number`. It is also specified that the function should return a Boolean value for this purpose, the signature has been extended with the annotation `boolean`.

> **Notice:** In other programming languages, it is common for the signature as a whole to identify a function. Thus, a program can contain two functions with the same name that include completely different code, provided they differ in their parameter lists. In JavaScript/TypeScript, that is not the case. Here, only the name matters, so it must be unique.

> **Attention:** The `return` statement causes the function to be exited at that point. Therefore, the `return` statement is usually placed at the end of a function. However, you can also take advantage of this mechanism to exit a function early—for example, using an `if` statement under certain conditions (“early out”). It can also be a function with no return values, in that case, the `return` stands alone.

- [x] Familiarize yourself with the booklet, paying particular attention to the "Coding Style" chapter. This style is mandatory when working on the assignments. Much of it may not make sense to you right now, but you should refer back to it frequently later on.

## Object
An object is an associative array with functions attached to it. These functions can modify the elements of the array without needing to be passed information about the object, since they are part of it and have access to it. To distinguish these functions from ordinary ones, they are called methods. An object therefore possesses methods with which it can alter itself or its environment. In the next few lessons, you will simply use and create predefined objects, later, you will learn how to define entirely new object structures.  
> **FunFact:** In fact, at its core, everything in JavaScript is of the object type. Even the primitive data types merely simulate their primitivity, allowing them to be used just as they are in ‘classic’ programming languages. 

## Values ​vs. References
An important difference between primitive and complex data types is the way variables are linked to their values. 

- [x] Declare two variables of a primitive type (e.g. `v1`and `v2`). Define the first one and then assign the value of the first to the second (`v2 = v1`). Then change the value of the first and output both. What do you notice?
- [x] Now declare two variables of a complex type. Define the first variable with multiple elements and then use the assignment operator just as shown above (`v2 = v1`). Change an element of the first variable. Output both variables. What do you notice? 

> - **Attention:** Failing to take into account that complex data types are addressed as references—meaning multiple variables can point to the same set of data—is a common source of errors. However, this referencing mechanism itself is neither a problem nor an error, rather, it is a crucial foundation for application development.

## Trace Table
A trace table allows you to determine and record the state of a program at any point during its execution. Creating such tables by hand is a great exercise for understanding how a program runs. We will therefore walk through this process together in the following video. You can find the corresponding code here: [Cows.ts](../X00_Code/L01_Recap&Foundation/Cows/Cows.ts)


<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_UXKQDJEI"><img src="../X01_Appendix/Img/L01_V1_Bild.JPG" width="25%"/></a>  
  <a href="Material/Cows_TraceTable.jpg"><img src="Material/Cows_TraceTable.jpg" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_UXKQDJEI"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L01 Foundation: Trace Table</a>
</div>

## Debugger
Of course, there are technical tools available to automatically generate something similar to a trace table. To do this, you use the debugger (the “bug remover”) 

>**FunFact:** The term "bug" for a programming error was coined by the famous computer pioneer Grace Hopper, who in the 1950s was actually able to attribute a malfunction in her mainframe computer to a moth that had died inside a relay.

You can use the debugger to hunt for errors. Since we are examining the program while it is running in the browser, it is very convenient that the browser has such a debugger built-in. Our little program doesn't currently have any errors, but we’ll take a look anyway to see if our trace table matches the debugger's results.

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_TVLCUNKX"><img src="../X01_Appendix/Img/L01_V2_Bild.JPG" width="25%"/></a>
  <br/>
<a href="http://hdl.handle.net/10900.3/OER_TVLCUNKX"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L01 Foundation: Debugger</a>
</div>

## Activity Diagram
Here, the Cows program is converted into an activity diagram, with the most important nodes and edges explained along the way.  

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_GWJDCZNL"><img src="../X01_Appendix/Img/L01_V3_Bild.JPG" width="25%"/></a>
  <a href="Material/Cows_ActivityDiagram.jpg"><img src="Material/Cows_ActivityDiagram.jpg" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_GWJDCZNL"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L01 Foundation: Activity diagram</a>
</div>

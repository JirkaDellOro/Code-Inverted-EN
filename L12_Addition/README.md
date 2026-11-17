# L12_Addition

<a href="https://www.youtube.com/watch?v=PK_yguLapgA"><img src="Material/Ariane5Failure.jpg"/></a>
<small>Source: https://docplayer.net/docs-images/66/55611304/images/11-0.jpg</small>

- [x] Click the image to see how a minor software bug caused 290 million euros to go up in smoke.

In this chapter, you will learn about some advanced structures that make interactive applications more flexible, maintainable, and secure. Even if you do not use them during the course, you should still be familiar with them and understand how they work. We will also introduce some constructs that are not necessarily recommended but are widely used and might otherwise confuse you when researching online.

## Default parameters
In addition to the question mark, there is another way to specify optional parameters in function or method declarations. You simply provide an assignment operator followed by a default value after the declaration of the formal parameter. If no parameter is passed during the call, the formal parameter automatically takes on the default value. This is useful, for example, for the constructor of the Vector class.
```typescript
constructor(_x: number = 0, _y: number = 0) {
    this.set(_x, _y);
}
```
A new `Vector` can now simply be created by calling `new Vector()`; both coordinates will automatically be set to 0. Consequently, there is no need to check within the method body whether parameters were passed in order to handle this scenario. However, if parameter values ​​are provided, they will be used.

## Property Parameters
Defining object properties using values ​​passed to the constructor is a very common pattern. TypeScript therefore offers a shorthand syntax for this, allowing properties to be declared directly within the constructor's formal parameter list.  

```typescript
class Test {
  constructor (public a: number, protected b: number, private c: number) {
  }
}
```  

An instance of the `Test` class now possesses the properties `a`, `b`, and `c`, even though they were not declared within the class body itself but rather in the constructor signature. When created using `let test: Test = new Test(1, 2, 3);`, these properties are initialized with the values ​​1, 2, and 3.

However, this syntax "hides" the properties within the signature, making it difficult to adhere to the coding style established in the lessons, as the elements serve as both parameters and properties. Consequently, this approach was not used, though it is worth mentioning here.

## Exception Handling
Error handling was largely omitted in the previous lessons. Code adjustments were made only when the compiler's and linter's strict settings flagged potential errors.

Runtime errors frequently occur—and are difficult to predict—especially when a system interacts with other systems or with users. Furthermore, attempting to catch every possible error using conditional logic within the code causes the program's complexity to escalate rapidly.

```typescript
interface Greet {
    greet: string;
}
let greets: Greet[] = [{ greet: "Hi" }, { greet: "Hallo" }, { greet: "Servus" }];

let input: string | null = prompt("Lass dich grüßen!", "Gib hier eine Zahl ein");
let greet: string = greets[Number(input)].greet;
alert(greet);
console.log("Done");
```

- [x] Implement the mini-program above within its own namespace and run it in the browser. What happens if you enter a number other than 0, 1, or 2, or input arbitrary characters?

If the program fails, it does so silently. The user has no idea what is happening—unless, of course, they are studying Media Conception in Furtwangen and therefore naturally have the developer console open. An error message appears there.

- [x] Explain the error!

However, the error might not be serious enough to require the program to terminate. Alternatively, the user should be informed that something went wrong so they can investigate the cause. In this example, one could simply check whether the input is valid. But there is another mechanism capable of handling even unknown sources of error: **exception handling!**

### Error Object
In the event of such an exception—one that causes the program to crash—JavaScript can automatically generate an Error object. Much like an event propagated through the DOM, this object can be passed through the program—or, more accurately, "thrown." It contains information about the problem that can be analyzed. Consequently, not every error needs to be prevented proactively; instead, they can be handled after the fact.

### `try`
To use this mechanism, you first isolate the code block that might trigger the error you want to handle. As usual, this is done by enclosing the code in curly braces, but this time preceded by the reserved word `try`. This instructs JavaScript to attempt to execute the code within the block and to throw an error object if something goes wrong.

### `catch`
This block must be immediately followed by a block designated with the reserved word `catch`. The "throwing" analogy becomes clear here, as the error object is caught and can be processed within the block. To achieve this, a formal parameter is declared after the `catch` statement, just as is common with functions or methods. However, a specific characteristic of TypeScript comes into play: as with `for..in` and `for..of` loops, no type annotation is permitted.

For example, using `try` and `catch`, the code above looks like this:

```typescript
namespace L12_Exception {
    interface Greet {
        greet: string;
    }
    let greets: Greet[] = [{ greet: "Hi" }, { greet: "Hallo" }, { greet: "Servus" }];

    try {
        let input: string | null = prompt("Lass dich grüßen!", "Gib hier eine Zahl ein");
        let greet: string = greets[Number(input)].greet;
        alert(greet);
    } catch (_error) {
        alert("Tschüss!");
        console.log(_error);
    }
    console.log("Done");
}
```

- [x] Expand your example code accordingly and then experiment with it.

### `finally`
A `finally` block can follow the `catch` block. The code inside it is executed regardless of whether an exception occurred or not. Even if the example code is defined within a function and both the `try` and `catch` blocks contain a `return` statement—meaning the function returns at that point—the `finally` block is processed first. However, the output "Done" will not appear in that case.

### `throw`
You can also create and throw an exception yourself. This is roughly analogous to creating a `CustomEvent` and dispatching it through the DOM. The error object propagates back up the function call chain until it is handled by a `catch` block in one of the calling functions. If this does not happen, it appears in the console as an "Unhandled Exception"...

This means that, alongside `return` and `await`, there is now a third way to terminate or interrupt functions. Depending on the use case, using `throw` can be extremely efficient.

- [x] Trace the "Exception" code provided in the attachment to this lesson using the debugger!

### Representation in the Activity Diagram
There are two perspectives for representing exception handling in an activity diagram. The "external view" simply shows that an activity can terminate with an exception and indicates how the flow proceeds from there. For this purpose—as an exception to the usual rules—the activity may have two outgoing flows; however, these do not signify parallel processing. This is because one of them is marked with a small triangle and, optionally—as with a conditional flow—includes information about the exception condition in square brackets.
![](Material/draw.io/Exceptions.svg)

The "internal view" shows the internal structure of an interruptible region and the signals that necessitate exception handling. The region's boundary is depicted with a dashed line, while the flow resulting from signal receipt is shown as a jagged line. In addition to the jagged line and the triangle on a line, there is also a representation using a lightning bolt symbol on the line.

## Function Object
In JavaScript, functions—like everything else—are objects. For instance, they can be referenced using variables. The following is therefore possible:

```typescript
function doSomething(_parameter: number): string {
   ...
}

let doToo: Function = doSomething;
doToo(42);
```

The result is the same as calling `doSomething(42);` because `doToo` and `doSomething` reference the same function object.

### Anonymous Functions
The function can also be assigned to a variable immediately upon creation; in this case, there is no need to provide a function name. The function itself is then "anonymous".

```typescript
let doSomething: Function = function(_parameter: number): string {
   ...
}
```

The function creation is equivalent to the one in the example above. However, `doSomething(...)` can now only be called after the definition, which is somewhat less than ideal.

Anonymous functions are frequently used where they are passed as parameters and do not need to remain accessible as reusable objects. Example:

```typescript
setTimeout(function (): void {
        console.log("Timeout");
    },         2000);
```

The anonymous function object is passed immediately to the timeout function and invoked after two seconds. Afterward, the function object becomes garbage and is eventually disposed of by the garbage collector.

While such anonymity is acceptable for small functions, these kinds of constructs generally tend to reduce the program's readability and maintainability.

### Arrow Functions
Since 2015, another way of writing functions has become common in JavaScript; it looks somewhat more cryptic but is essentially equivalent to the `function` syntax you are already familiar with—which is also somewhat more intuitive.

```typescript
let doSomething: Function = (_parameter: number): string => {
   ...
};
```
The reserved word `function` preceding the parameter list has been replaced by a double arrow placed between the return type and the function body.

### `this` binding
One difference, however, lies in their behavior within classes. If an arrow function is declared as a method, it appears as a property in the console. More interesting, though, is the difference when used as an event handler:
- In traditional methods, `this` refers to the event target rather than the class instance. This frequently leads to unexpected errors.
- With arrow functions, on the other hand, `this` continues to refer to the instance, as expected. Therefore, this syntax is preferable in such cases.
- It is also possible to modify a traditional method so that it, too, references the instance via `this` when used as an event handler; the function object's `bind` method is used for this purpose.

- [x] To understand this better, try experimenting with the "FunctionObject" code provided in the attachment.

## Drag & Drop
One important standard feature of the DOM has not yet been covered in this course. Particularly in desktop applications, it is very common to manipulate objects directly—not via form elements, but rather through actions like drag-and-drop. Using this feature is quite straightforward:

- `draggable` is a property defined in the `HTMLElement` class. If set to `true`, the mouse behavior on the corresponding element changes.
- When the mouse button is pressed while the cursor is over the element, a `dragstart` event is fired.
- The event object is of type `DragEvent` and includes a `dataTransfer` property, which references an object of type `DataTransfer`.
- The `setData(key, value)` method adds information to the `dataTransfer` object.
- An `HTMLElement` intended to serve as a drop target must explicitly allow the drop. To do this, it must listen for the `dragover` event and use `event.preventDefault()` to suppress the default behavior, which is to reject the drop.
- It can then respond to a `drop` event; the corresponding handler receives the event object that was created and populated with additional information during the drag operation.
- The `getData(key)` method allows the information to be retrieved and processed—for example, to modify the drop target.

- [x] Attached is a small example program called "DragDrop" featuring a rudimentary implementation of the DOM drag-and-drop mechanism, a system capable of much more. Take a look!

## Garbage Collection
When a variable goes out of scope, the memory it occupied is released and becomes available for other data. However, for variables that reference objects, only the reference itself is deleted; the referenced object remains in memory because other variables might still be pointing to it. If the object were removed, those other variables would point to nothing, causing errors. On the other hand, if objects are never deleted, memory becomes increasingly cluttered, eventually leading to a program failure.

In a JavaScript program, memory is monitored by an algorithm known as the "garbage collector." It scans memory at irregular intervals, identifies objects that are no longer needed, and deletes them. In most cases, this process occurs completely unnoticed; however, with animations, it can lead to visible glitches if a single frame lingers longer than the others. Reusing objects can help minimize this effect.

## Debugger in VSCode
### Node Programs
VSCode's built-in debugger was briefly introduced in the lesson on servers. Since the browser is not involved in Node programs, its debugger cannot be used to inspect program execution. There are a few additional details that are very useful when working with the VSCode debugger.
Because VSCode can manage multiple projects simultaneously, the debugger needs to know which program to execute and monitor when it starts. In the simplest scenario—where the debugger displays "No Configurations"—it launches the program currently visible in the active editor window. However, you can also define specific configurations via the menu system; these are stored in a `launch.json` file within the `.vscode` folder, which the debugger automatically creates.

- [x] Open the `X01_Appendix` folder in VS Code, navigate to the Debug view, and select `Open launch.json` (or `Open Configurations`). The `launch.json` file should now be displayed.
- [x] Examine the first entry in the `configurations` array. Note that `${workspaceFolder}` is a placeholder that is replaced by the path to the directory you opened in VS Code—in this case, the path to `X01_Appendix`.
- [x] Locate the name of this first configuration elsewhere in the Debug view. Then, ensure that this configuration is used when you start the debugger.
- [x] Start the debugger (e.g., by pressing F5 or clicking the green arrow).
- [x] Observe the output in the debug terminal and the editor window.

Line 3 contains the familiar `debugger` statement; consequently, program execution has paused here, and the control panel is displayed, showing options you already recognize from the browser debugger. Instead of writing this statement into the code, you can, of course, simply set breakpoints by clicking next to the line number, as is standard practice.

- [x] Perform a "Step over" twice to see the next console output.
- [x] Explain the source of the greeting that now appears.

### Browser-based Programs
You don't have to forgo the convenience of the VS Code debugger when working with browser-based programs, either. VS Code can communicate with the browser via a local server and control the running program. This requires a bit of setup. The following example describes how to do this using Chrome.

- [x] Install the "Debugger for Chrome" extension in VS Code.
- [x] Look at the second entry under `configurations` in `launch.json`. Similar to the `program` entry, there is a `url` entry here. This specifies the path to the HTML file to be executed, relative to the root directory of a local file server—specifically, the directory you opened in VS Code.
- [x] Start the file server in the root directory using `npx serve` or, if you have already installed "serve" globally, simply by running `serve`.
- [x] Activate the "Launch DebugBrowser with Chrome" debug configuration from `launch.json`.
- [x] Start the debugger. Note that Chrome will now launch with the web app.
- [x] Explain where the greeting message is coming from now.

## Teamwork
**Git** offers additional features beyond those used so far; "branches" and "forks" are particularly important for teamwork.

### Branches
Branches allow multiple versions of the same repository to be stored simultaneously. Up to this point, you have worked only with the `master` branch. Especially when multiple people are working on or with the code, it is highly desirable to maintain a branch representing a stable, tested state (often `master`), while using other branches for ongoing development, experiments, and so on. Once a branch contains changes intended for the `master` branch, **Git** can handle the process of combining them—known as a "merge." Human intervention is required only if conflicts arise—that is, if the same file has been modified in both branches being merged—to decide which version of the code should be kept.  

### Forks
This system enables community-based open-source development without requiring team members to work within a fixed structure—or even know one another. Anyone can create a clone (**fork**) of a public repository in their own GitHub space and work on it. If something useful for the original project emerges, a "pull request" containing the changes can be submitted. The original project's developers can then review these changes and "merge" them into their repository if appropriate.

## Platforms
The technical foundation used for this "Development of Interactive Applications" module series is based on web technologies. However, this does not mean your concepts must be limited to websites. With what you have learned so far, you can design and develop full-fledged native apps and desktop programs. To do this, you employ techniques that elevate your software to an extended platform; while a browser and/or Node continues to operate in the background, the user remains unaware of this.

### Electron
Developed by GitHub itself, it uses Node.js and Chromium—the developer version of the Chrome browser. Its capabilities are extended to include system functions such as file system access, allowing your web app to become a full-fledged native desktop application for Windows, Mac, and Linux.

Incidentally, you have likely been using such an application for some time, as Visual Studio Code is an Electron app!

### Cordova/PhoneGap
Cordova and PhoneGap are essentially identical products that share the same origins and are developed by Apache and Adobe, respectively. They are based on the same principle as Electron, but are designed for mobile Android and iOS devices. This allows your web app to be distributed via the relevant app stores and to access features such as GPS, accelerometers, and the camera during operation.

The Wikipedia mobile app is a PhoneGap application!

### Progressive Web Apps
Since 2015, another model has been poised to shake up the app market: Progressive Web Apps (PWAs). Initially, they appear just like ordinary web apps, but upon the first visit, they install a "service worker" on the smartphone. This software selectively downloads and stores content on the device, ensuring the app remains functional even without an internet connection. Users can also create a home screen shortcut; the app can launch in full-screen mode and display server-sent push notifications, even when the app is not currently active. As a result, the PWA functions like a full-fledged native app, requiring minimal extra effort compared to standard web development. The huge advantage: once a potential user arrives at the site via a search engine, they do not need to leave the page and head to an app store to install and launch the app—usage has effectively already begun!

The mobile Twitter client is a Progressive Web App.

# Conclusion
<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_GZLPLYRR"><img src="../X01_Appendix/Img/L12_V1_Bild.jpg" width="25%"/></a>
  <br>  
  <a href="http://hdl.handle.net/10900.3/OER_GZLPLYRR"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L12 Addition: Extro</a>
</div>


## eiaSteroids
The example project has now also been given this unique and distinctive name. Take a close look at it, especially the final documentation. It serves as a model for your final project, and you should use it as a guide! So, here is the link to [**eiaSteroids**](../X01_Appendix/eiaSteroids)

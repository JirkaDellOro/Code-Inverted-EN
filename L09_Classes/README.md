## L09 Classes
# L09.1 Basics
<img src="Material/Alan-kay.jpg">  

<small>Source: https://www.thepositiveencourager.global/alan-kay-on-creativity-3</small>

As the son of a scientist and an artist, Alan Kay never distinguished between art and science. While earning a living as a jazz guitarist and guitar teacher, he obtained a bachelor's degree in mathematics and molecular biology, a master's degree in electrical engineering, and a Ph.D. in computer science. He played a pivotal role in coining the term "object-oriented programming."

## Object-orientation
This software design paradigm enables the intuitive modeling and conceptualization of even complex software applications.  

You know from experience that the reality you perceive is composed of a vast number of material elements (cars, houses, living beings, furniture, etc.) and immaterial ones (language, feelings, accounts, identities, etc.). While fully describing this environment is likely impossible, you can certainly grasp its individual components at various levels of abstraction. In doing so, you simplify, classify, and generalize, constructing a mental model of reality that allows you to make predictions even without knowing every detail of your immediate surroundings. You know what a dog is, even though "the dog" does not actually exist; there are only ever specific individual dogs. Nevertheless, you assume that a dog completely unknown to you is capable of barking. You know that a dog typically has four legs, a snout, and a tail, and that—in addition to barking—it has the ability to growl, bite, and sometimes smell terrible.

## Abstraction
You take a similar approach with object-oriented design. When planning a system, you consider the parts that make it up, the types of similar parts involved, how to describe them, and how these parts interact with their environment. You use abstraction and generalization to identify properties for various classes of parts—properties that further describe the individual, actual part. For the "Dog" class, for instance, these might be size or coat color. These properties can vary within certain limits without the animal in question ceasing to be a dog. It remains a dog—even if that is sometimes hard to believe. 

<img src="Material/Hunde.jpeg"/>  
<small><a href="https://zaypa.com/wie-wahle-ich-deinen-hund-fur-dich-und-deine-familie.html">https://zaypa.com/wie-wahle-ich-deinen-hund-fur-dich-und-deine-familie.html</a></small>

> **Abstraction** is the first principle of object-orientation!

## Modeling
Up to this point, we have defined more complex data structures using interfaces somewhere in the code. In doing so, we have already specified object properties; for instance, all vectors from the previous chapter possess x and y properties, which are defined with specific values ​​for each individual vector actually created. However, the logic governing what happens to these vectors is implemented elsewhere in the code. Consequently, whenever vectors need to be added or scaled, the corresponding code has to be written from scratch each time.

In object-oriented modeling, you define not only the properties but also the activities that an object of the described class should be able to perform. Just as a dog can bite, bark, growl, and smell, a vector might be able to change its length or combine with another vector via addition to form a resultant vector, and so on. In the diagram, you simply add a third field—below the fields for the type name and the properties—where you list these activities.

<img src="Material/draw.io/Vector_ClassDiagram.svg">

Up to now, you have only included interfaces as a side note in activity diagrams. Now you will be introduced to a new type of diagram: **the class diagram**. It illustrates how classes are structured and how they relate to one another.  

- [x] Where on earth have you seen something like that several times?

## Implementation  
The vector class shown in the diagram can be implemented as follows:

```typescript
class Vector {
    x: number;
    y: number;

    scale(_factor: number): void {
        this.x *= _factor;
        this.y *= _factor;
    }

    add(_addend: Vector): void {
        this.x += _addend.x;
        this.y += _addend.y;
    }
}
```

Vector is no longer just an object structure that can be described by an interface; Vector is now a class. Consequently, the reserved word `class` is used instead of `interface`. The activities are implemented as functions as usual, but the `function` keyword is no longer specified.

> **Note:** Functions within classes are called "methods." Objects of the class now possess methods that allow them to influence themselves and their environment.


## Encapsulation
A program using such a vector no longer needs to know how to scale the vector or add a second one to it. It suffices to know that these operations are possible and which parameters the vector requires for them; the vector handles the rest itself. This turns the vector into a "black box"—its internal workings need not be known in order to use it. The vector is "encapsulated."

> **Encapsulation** is the second principle of object-orientation!

## Instantiation
The code above describes the `Vector` class, but no instance of an object of this class exists yet. Unlike when working with interfaces, such an object is created using the `new` statement. This process is also known as instantiation, as an instance of the class is created.

- [x] In what contexts are you already familiar with `new`?
- [x] Implement the vector class in a new `namespace` and append the lines below.

```typescript
let v1: Vector = new Vector();
v1.scale(2);
console.log(v1);
```  
- [x] Test the program in the browser or with Node. What console output do you get?
- [x] Trace the program using the debugger into the class's `scale` method. What is the significance of the `this` keyword?
- [x] At the beginning of the class, the properties x and y are declared. Modify the code at this point so that they are also initialized with the value 0. What changes during the next test and debugging session?
- [x] Add a new method `set(_x: number, _y: number)` to the class that assigns the values ​​of `_x` and `_y` to the properties of an object of that class. Before calling `scale`, have v1 execute this method using values ​​of your own choosing. What happens now?

## Class vs. Object
The terms "class" and "object" are frequently confused or used imprecisely, leading to confusion and errors. Although the distinction has already been highlighted several times in this text, it is worth addressing it explicitly once more here.
A class describes the structure that an object of that class is to possess, as well as the methods associated with such an object. It is comparable to the dog genome or an architect's blueprint for a house. Once the class is defined, any number of objects of that class can be created—much like an entire litter of puppies, a housing development, or an array full of vectors. Each of these objects possesses the properties specified in the class, potentially with individual values ​​such as coat color, roof tile type, or coordinates.

<img src="Material/Instanzierung.svg">

Furthermore, an object of a class can utilize the methods defined within that class without requiring a reference to the object itself to be passed as a parameter. Instead, the object is automatically referenced within the method using the reserved word `this`. The method is invoked using dot notation—`object.method(...)`—and since the object is an instance of a specific class, the method is uniquely identified, even if other classes possess a method with the same name.

## Constructor
The house-building metaphor leads you straight to another interesting aspect. The architect—that is, you—draws up the plans, but the house is actually built by a building contractor.

A constructor is also used when instantiating objects. It is a special method that is automatically called when the `new` statement is executed, ensuring that the object is properly constructed. JavaScript always includes a default constructor that operates behind the scenes, but you can define your own constructor for any class.   

A constructor is significant when additional information needs to be incorporated into the object's creation or when other mandatory actions must be performed during the process. In the case of a house, the constructor might need to know exactly which bricks to lay, as this is a variable property of the house. When a dog is created, the local municipality might need to be notified immediately so that it can collect the dog tax.

For the vector class, it makes sense to supply the components when constructing a vector, so that the vector's x and y properties immediately take on the desired values.

```typescript
class Vector {
    x: number;
    y: number;

    constructor(_x: number, _y: number) {
        this.set(_x, _y);
    }

    set(_x: number, _y: number): void {
        this.x = _x;
        this.y = _y;
    }

    ...
}
```
The constructor now strictly requires two values ​​to be passed in order to instantiate a vector. TypeScript will report an error if you try to instantiate a vector using only `new Vector();` as before. `new Vector(10, -3)`, on the other hand, is accepted, and the vector is created with the provided values.

> **Note:** Incidentally, the `set` method is the one you should have implemented earlier. If it didn't work out, you can take a peek here...

## Conventions
When working with classes and objects, you should definitely adhere to a few conventions:

- Each class is implemented in its own file.
- The filename corresponds to the class name (e.g., `Vector` is found in Vector.ts).
- The method names do not repeat the class name (`scale` instead of `scaleVector`).
- The class name describes the nature of exactly one instance of the class, not multiple instances (`Vector` instead of `Vectors`).

> **Note:** By default, JavaScript scopes namespaces to the file level. Use the `export` keyword to make information—such as classes or variables—publicly available throughout your namespace without restriction.

# L09.2 Asteroids
With these basic rules of object-oriented design, one can begin to conceptualize and implement a complex interactive application. The original arcade version of the video game "Asteroids" (Atari, 1979) is used as an example for this and the following lessons.

> **FunFact:** Asteroids is one of the greatest successes in the history of video games and Atari’s best-selling arcade machine. However, there was a dominant strategy that allowed skilled players to play for hours after inserting just a single coin, accumulating so many bonus ships that the machine—once abandoned by the player—would take a long time to destroy them all...

- [x] If you aren't familiar with Asteroids, take a look at the corresponding Wikipedia article and this YouTube video: <https://www.youtube.com/watch?v=WYSupJ5r2zo>

## Use case diagram
![](Material/draw.io/Asteroids_UseCaseDiagram.svg)

- [x] If the use case diagram doesn't make sense to you right away, take a close look at the Wikipedia article and the video, and try to match the individual use cases to the events taking place.
- [x] Note that the diagram lists many use cases that the system must handle autonomously. Anything controlled by the player is also connected to it.

## UI-Scribble
![](Material/draw.io/Asteroids_Scribble.svg)  

To make the game more interesting—particularly for the purposes of this course—a slightly different defense mechanism is being designed for the spaceship. Instead of the small, individual projectiles fired by the UFOs, laser beams are used; these create a destructive force field at the point where they intersect. This allows for better use of the mouse as a control mechanism and introduces the added challenge of energy management for the player.

## Class Diagram
In object-oriented modeling, there is an additional type of diagram you should use before starting on detailed activity diagrams. Here, you first identify the various object types that play a role in your application. If these are objects with a visible representation, you can identify them in the UI sketch.

### Five Questions
For each object type, you then ask yourself the following questions, where "it" refers to an example object of that type.

1. what does it have?
1. what can it do?    
1. what does it know?
1. what does it hold?
1. what is it?

The first two questions lead you directly to the representation of the individual classes in the diagram. Let's start with the asteroids...

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_KBWGATWN"><img src="../X01_Appendix/Img/L09_V1_Bild.JPG" width="25%"/></a> 
  <a href="Material/Asteroids_ClassDiagram.jpg"><img src="Material/Asteroids_ClassDiagram.jpg" width="25%"></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_KBWGATWN"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L09 Classes: Class diagram</a>
</div>

## Activity Diagram
You can now design the individual methods using activity diagrams.

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_PNFVRAMC"><img src="../X01_Appendix/Img/L09_V2_Bild.JPG" width="25%"/></a>
  <a href="Material/Asteroids_ActivityDiagram-Asteroid.jpg"><img src="Material/Asteroids_ActivityDiagram-Asteroid.jpg" width="25%"></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_PNFVRAMC"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L09 Classes: Activity diagram Asteroid</a>
</div>

## Implementation
<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_XFJJRZUC"><img src="../X01_Appendix/Img/L09_V3_Bild.JPG" width="25%"/></a>
  <br/>   
  <a href="http://hdl.handle.net/10900.3/OER_XFJJRZUC"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L09 Classes: Implementation Asteroid</a>
</div>

## Timing Signals
Implementing the animation using a simple loop isn't feasible, as the browser would become entirely occupied with the loop and wouldn't have time to actually render the image. Instead, the browser needs to display the image, wait for a specific interval to pass, then draw and display the next image, and so on.
You already know that event listeners persist and remain active even after the main program has finished executing. The same applies to timing signals! In activity diagrams, a dedicated symbol—resembling an hourglass—is defined as the start node for these signals. This allows you to easily define a "heartbeat" for your application.

![](Material/Heartbeat.svg)

In JavaScript/TypeScript, the following options are available to you for implementing such time signals. 

### window.setTimeout (handler, time, args...)
The statement causes the `handler` function to be called after the time specified by `time` (in milliseconds) has elapsed, measured from the moment the statement is invoked. Additional parameters (`args...`) can be passed to the function.

### window.setInterval (handler, time, args...)
The instruction causes the `handler` function to be called periodically at the time interval specified by `time`. Additional parameters (`args...`) can be passed to the function.

### window.requestAnimationFrame(handler)
The instruction causes the `handler` function to be called periodically at a time interval deemed appropriate for graphical rendering. The browser determines this interval, Chrome attempts to achieve a refresh rate of 60 frames per second (fps).

## Main Program  

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_MWEVCUUT"><img src="../X01_Appendix/Img/L09_V4_Bild.JPG" width="25%"/></a>
  <a href="Material/Asteroids_ActivityDiagram-Main.jpg"><img src="Material/Asteroids_ActivityDiagram-Main.jpg" width="25%"></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_MWEVCUUT"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L09 Classes: Activity diagram Main Program</a>
</div>

> **Note:** Not everything can be determined from the outset; you have to experiment with some aspects to get a feel for how to achieve a good user experience. During the conceptual phase, you need to assess which tests are required and how to facilitate them. You also need to be capable of developing prototypes yourself so that you can implement and test minor elements independently, without needing to employ a whole team of people!

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_XHUTJHVV"><img src="../X01_Appendix/Img/L09_V5_Bild.JPG" width="25%"/></a>
  <br/>  
  <a href="http://hdl.handle.net/10900.3/OER_XHUTJHVV"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L09 Classes: Implementation Main Program</a>
</div> 


> **Note**: MouseEvent provides the offsetX and offsetY information—currently (as of May 2021) as an experimental technology. 

A problem arises when passing the position to the optional `_position` parameter in the `Asteroid` class constructor. Since `Vector` is a complex type, a variable of this type holds only a reference to a vector object (see [Values ​​vs. References](https://JirkaDellOro.github.io/Code-Inverted/L01_Recap%26Foundation#werte-vs-referenzen)). Consequently, assigning the parameter's content to the object attribute does not create a new vector object; instead, it simply reuses the existing reference. As a result, the fragments reference the same vector object and therefore share the same position.

- [x] Solve the problem by instantiating a new position object using the information from the parameter instead of assigning the reference and assigning that object.
- [x] Extend the vector class with a `copy()` method that returns a new vector object having the same attribute values ​​as the vector executing it.
- [x] Use this method to solve the problem above even more elegantly.
- [x] Scale the line thickness in the draw method of the Asteroid class inversely proportional to the size attribute.

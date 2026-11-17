## L11 Advanced
# L11.1 Modifiers & Accessors

![](Material/Sims.png)

<hr>  

**Side note**: A solution for the final exercise of Lesson 10 using CustomEvents, which were already briefly touched upon in [Lesson 03](../L02_Events/README.md#customevents).
<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_FMOQLXZU"><img src="../X01_Appendix/Img/L11_V1_Bild.jpg" width="25%"/></a>
  <a href="Material/draw.io/Asteroids_ActivityDiagram-Ufo.svg"><img src="Material/draw.io/Asteroids_ActivityDiagram-Ufo.svg" width="13%"></a>
  <a href="Material/draw.io/Asteroids_ActivityDiagram-Main_2.svg"><img src="Material/draw.io/Asteroids_ActivityDiagram-Main_2.svg" width="25%"></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_FMOQLXZU"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L11 Advanced: UFOs</a>
</div>

<hr>  

Object-oriented modeling offers several additional capabilities that simplify and improve the development and maintenance of software systems. Complex software is typically developed by teams, with various parts, modules, and classes designed, implemented, and utilized by different individuals. To avoid misunderstandings among team members or to rule out programming errors from the outset, it is extremely helpful and practical to be familiar with and utilize the following advanced mechanisms.  

- [x] For this lesson, design a small program consisting of a superclass, a subclass of that superclass, a main program, and an HTML file that loads your scripts. The specific functionality is irrelevant at this stage; the focus is solely on the structure.

## `Abstract`
### Abstract class
No objects are instantiated from the `Moveable` class in Asteroids—only from its subclasses. `Moveable` is thus a purely abstract class; like "dog," it represents merely a concept or an idea (compare Plato's theory of forms). A class can be designated as such using the reserved keyword `abstract`. Any attempt to instantiate an object of this class will now be flagged by the TypeScript compiler.  

- [x] Mark your superclass as abstract by simply writing `abstract` before `class`. Try to create an object of the class in the main program and observe the error messages.

Objects of the subclasses can nevertheless continue to use the methods of the abstract superclass.

- [x] Verify this statement by implementing a method in your superclass and calling it from an object of the subclass.

### Abstract Method
However, there are methods that cannot be meaningfully implemented in the abstract superclass itself, but only in the subclasses. `draw()` in `Moveable` is a good example of this. Nevertheless, the superclass `Moveable` must include the `draw` method so that the main program can easily manage all `Moveable` objects and call their `draw` methods.

Such methods in abstract classes should also be marked as `abstract`. This eliminates the need to implement a method body and enforces the requirement that the method be defined in the subclass.

- [x] Mark a new method in your abstract superclass as abstract. Do you receive an error message if you do not define this method in the subclass?

> **Note**: You can indicate abstract classes or methods during the design phase in the class diagram; simply write the corresponding class and method names in italics.

## Static
The principle of static class methods and properties works in the opposite direction. They can be used without the need to create an instance of the class; in this way, the class—or a part of it—becomes very concrete. To achieve this, one simply needs to prefix the method or property declaration with the reserved word `static`.

In the case of *Asteroids*, static methods within the Vector class could prove useful. The following line from `isHit`, for example, is rather inelegant

```typescript 
let difference: Vector = new Vector(_hotspot.x - this.position.x, _hotspot.y - this.position.y);
```
It would be more readable if the Vector class could calculate the difference between two vectors and return the resulting vector. This is easy to implement in the Vector class

```typescript
static getDifference(_v0: Vector, _v1: Vector): Vector
  let vector: Vector = new Vector(_v0.x - _v1.x, _v0.y - _v1.y);
  return vector
}

```
The complexity of creating a new vector and performing component-wise subtraction is now encapsulated within the Vector class, and the new class method `getDifference` can be used simply and intuitively.
```typescript
let difference: Vector = Vector.getDifference(_hotspot, this.position);
```

- [x] Define a static property and a static method in your superclass. Have the main program output the property to the console and call the method. Instead of using an object reference, simply use the class name for this purpose.

> **Note**: In class diagrams, static methods or properties are indicated by underlining!

## Validity
Variables declared with `let` within a code block delimited by curly braces are valid only within that specific block. In doing so, they can "shadow" other variables of the same name that are valid outside the block. This is illustrated in the following simple example.

```typescript
let x: string = "I'm valid outside";
{
  let x: string = "I'm valid inside";
  console.log(x);
}
console.log(x);
```
The output is generated
```plaintext
I'm valid inside
I'm valid outside
```
It is evident that two variables named `x` existed while the block was executing, as the content of the "outer" variable remained unchanged. In contrast, the "inner" variable ceased to be valid once the block finished executing; it left its "scope," and the memory it occupied was released. The "outer" variable, however, continued to exist, and its content was output.

Such blocks can define the boundaries of loop bodies, `if` blocks, function or method bodies, classes, objects, and namespaces. As shown in the example above, it is also possible—albeit rare—to define a block solely to establish a scope.

- [x] Examine the "Scopes" code provided in the appendix to this lesson and run it in either the console or the browser. In this code, the variable `x` is declared as a `string` ten times and assigned different string values. These values ​​indicate the scope.
- [x] Explain the output and the two deviations.

## Visibility
Multiple scopes exist side-by-side and nested within one another. Confusion can easily arise particularly when access is required from one scope to another, such as when the main Asteroid program accesses the properties or methods of "Moveable" objects. This is because classes and objects expose some information as an interface for external access, while other information is required solely for internal functionality; attempting to access the latter can lead to errors.

In object-oriented modeling, visibility modifiers can be used to protect properties and methods from unauthorized access. This not only enhances security but also makes it much easier for team members to identify and select the appropriate access points during development. These modifiers are simply reserved words placed before the declaration of methods and properties.

### `public`
Access is completely public. This is also the default visibility if no modifier is specified. That is why Asteroids was able to work as it did. An analogy would be a front yard that anyone can walk into and where anyone can handle the items found there.

> **Note**: In a class diagram, `public` is marked with the `+` symbol

### `private`
Access is permitted only for objects of the same class. In most cases, therefore, the object accesses its own properties and methods. An analogy might be the parents' bedroom, where only the adults in the house have access to the interior.

> **Note**: In the class diagram, `private` is marked with the `-` symbol

### `protected`
Access is permitted only for objects of the same class and their subclasses. An analogy is the rest of the house, which parents and children can use but which remains closed off to those outside the family.

> **Note**: In the class diagram, `protected` is marked with the `#` symbol

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_WYOSLZMG"><img src="../X01_Appendix/Img/L11_V2_Bild.jpg" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_WYOSLZMG"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L11 Advanced: Updating the classes</a>
</div>

### `readonly`
Different programming languages ​​may offer additional modifiers. Worth mentioning in the context of TypeScript is `readonly`, which can be combined with the previously mentioned modifiers. A property marked in this way can only be defined at the time of declaration or within the constructor; its value cannot be changed thereafter. It thus corresponds to the `const` keyword used for standard variables.

## Access functions 
### `set`
Sometimes it is desirable for an object's property to be modifiable—as if it were `public`—while ensuring the object itself is aware of the intended change and can intervene or trigger necessary processes. For instance, the object could verify whether the value to be assigned falls within specific limits and reject the update if it does not. To achieve this, it could provide a method for the change while declaring the actual attribute `private`, thereby restricting direct access to itself.

```typescript
private value: number;

setValue(_newValue: number): void {
  if (_newValue < 100)
    this.value = _newValue;
}
```
If the `value` property of the `instance` object is to be modified, the statement `instance.setValue(...)` is used. Since such use cases occur frequently—and the syntax `instance.value = ...` for modifying a property simply appears easier and more intuitive—there is a special type of method: the "setter"

```typescript
private valuePrivate: number;

set value(_newValue: number): void {
  if (_newValue < 100)
    this.valuePrivate = _newValue;
}
```

Now, for instance, when the statement `instance.value = 59;` is executed, this setter method is automatically called, and the value on the right-hand side of the assignment operator—in this case, `59`—is passed as a parameter. The same applies to combined assignment operators (see appendix). Note that the class no longer contains a property named `value`, as this would cause a naming conflict with the setter. Therefore, in this example, the internal attribute is named `valuePrivate`. However, viewed from the "outside," `instance` possesses a property named `value` that now incorporates special functionality and cannot be modified in an uncontrolled manner.

- [x] The Vector class already has a `set(...)` method. Is this a "setter"?

### `get`
The counterpart to `set` is, of course, `get`. This allows, for example, the reading of a property that isn't stored but only calculated when requested. A candidate for such a "getter" could be the `length` attribute of the Vector class. The length of a vector can be easily calculated using the Pythagorean theorem; JavaScript's `Math` object provides the `hypot` method for this purpose. Since the elements of the vector are constantly changing, it would be unnecessary overhead to calculate and store the length with every change. With

```typescript
get length(): number {
    return Math.hypot(this.x, this.y);
}
```

however, viewed from the outside, it appears as though this property is always present, since it can simply be accessed via, for example, `let speed: number = velocity.length;`. This statement obscures the fact that it is actually a method call.

> **Note:** By default, accessor functions are not explicitly marked in the class diagram but are simply represented as properties.

# L11.2 Asteroids reloaded

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_RDULYUHE"><img src="../X01_Appendix/Img/L11_V3_Bild.jpg" width="25%"/></a> 
  <a href="Material/draw.io/Asteroids_ClassDiagram.svg"><img src="Material/draw.io/Asteroids_ClassDiagram.svg" width="25%"></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_RDULYUHE"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L11 Advanced: Class diagram</a>
</div>

## Enumeration types
It is often necessary to describe a piece of information using a data type that allows for only a limited, discrete set of values. The simplest example of such a data type is `boolean`. Only two values ​​are permitted here: `true` and `false`. An attempt to assign a value such as `maybe` to a variable of this type, for instance, will fail.

You can create such enumeration types yourself using the reserved word `enum`.

```typescript
enum TASK {
    WATCH,
    PATROL,
    CHASE,
    SLEEP
}
```
The type in the example could be used to represent the tasks of a guard dog. An object of the guard dog type could then have a `task` property that can only take on these four values.

```typescript
class Watchdog extends Dog {
    private task: TASK = TASK.WATCH;

    update(): void {
        switch (this.task) {
            case TASK.SLEEP:
                ...
                break;
            case TASK.WATCH:
                ...
                break;
            ...
        }
    }
}
```

If the watchdog's `update` method is now executed cyclically, different activities or behavioral methods can be invoked depending on the current `task`. The `switch` statement already provides the necessary construct for this.

- [x] What might the corresponding activities look like? Create rough activity diagrams for them.

Without the specific enumeration type, `task` might simply be of type `number`. In that case, you would have tasks 0, 1, 2, and 3, and would need to track elsewhere which number corresponds to which task. Furthermore, numbers for which no task is defined would also be valid a scenario practically inviting errors. Alternatively, `task` could be of type `string`, assigned values ​​like `"sleep"`, `"watch"`, and so on. While this makes the program readable again, it is highly error-prone, as typos such as `"petrol"` can easily creep in.

By using an enumeration type, however, TypeScript can offer the correct suggestions while you write the code, present the valid values ​​for selection, and detect errors immediately.

> **Note**: According to the UML standard, an enumeration is marked in a class diagram with the tag `<<enumeration>>`. The same applies to interfaces, which are marked with `<<interface>>`.

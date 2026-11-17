## L10 Inheritance
# L10.1 Inheritance

![](Material/Evolution.jpg)

## Generalization vs. Specialization
<img src="Material/barabara-liskov.jpg" width="100" align="right"/>

In 1987, Barbara Liskov presented a core concept of object-oriented modeling at a conference. According to this concept, an object of a class should be replaceable by an object of a subclass of that class without requiring changes to the program using the object. In 1993, together with Jeannette Wing, she formalized what is now known as the **Liskov Substitution Principle**.

With regard to dogs, the Liskov Substitution Principle can be illustrated as follows:
If dogs can generally bark, then subclasses of `Dog`—such as `Great Dane`, `Chihuahua`, and `Poodle`—can also bark. So, if you ring the doorbell of a house displaying a "Beware of Dog" sign, you can assume that a dog will bark. The sign does not need to say "Beware of Poodle," even if the dog inside happens to be a poodle.  

`Poodle` is a specialization of `Dog`; `Dog` is the generalization of `Poodle`.  

Poodles are considered particularly quick to learn, which is why they are often used for special tricks and in circus performances. It makes no difference whether they are Toy, Miniature, or Standard Poodles; these are simply size variations of the breed.

The `dog` can be further generalized, as it belongs to the `caniform` family—a group that also includes bears, seals, and skunks, for example. Together with the `feliforms`, the `caniforms` constitute the `carnivores`, which in turn belong to the class of `mammals`, a group that also includes `humans` as a specialized form. 

## Representation in the class diagram
In biology, classification involves classes, races, species, genera, families, orders, and so on. In object-oriented modeling, one merely needs to distinguish between superclasses and subclasses.

The superclass represents the generalization of a subclass, and the subclass a specialization of the superclass.

In a class diagram, arrows with an open triangular arrowhead point from the subclass to its superclass, thereby representing this relationship. The digression into biology mentioned above, which, incidentally, does not stand up to scientific scrutiny, could be represented in a class diagram as follows. 

![](Material/draw.io/Dogs_ClassDiagram.svg)

- [x] Study the diagram and clarify the relationship between the subclass and the superclass.
- [x] Describe the characteristics and abilities of a miniature poodle based solely on the diagram.
- [x] Should the various poodles form their own subclasses? What would be an alternative?
- [x] How does such a diagram and considering generalization and specialization, help you when dealing with living things? Describe this using the given example.

## Inheritance
In this context, the term "inheritance" is also used. A superclass passes on all its properties and methods to the subclass, just as shown in the species family tree above.

> **Inheritance** is the third principle of object-orientation!

Due to the inheritance relationship, superclasses are sometimes referred to as parent classes and subclasses as child classes. However, this leads to confusion, as certain runtime relationships between objects—such as those in the DOM—are already designated as parent-child relationships. Other terms include upper and lower class, base and derived class, among others. In this course, the terms "superclass" and "subclass" will continue to be used exclusively.

## Modeling rules
A circus animal trainer must know not only the characteristics of each dog breed in his troupe but even those of every individual animal. The clerk at the spaceport of a modern planet, on the other hand, needed only to know whether a life form was a mammal in order to assign it to the right room. ([see Ijon Tichy, Episode 2, 4:23 – 5:33](https://www.youtube.com/watch?v=_DSCXsIR03Q))

When modeling a system, you should observe these two rules:
- each part should be only as smart as necessary
- each part must be as dumb as possible

Each part should therefore know only what is absolutely necessary for its own function regarding its environment and other parts!

# L10.2 UFOs
Asteroids is being further developed with the above-mentioned principles in mind.

## Extension of the class diagram
<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_QHHUCSHA"><img src="../X01_Appendix/Img/L10_V1_Bild.jpg" width="25%"/></a>
  <a href="Material/Asteroids_ClassDiagram.jpg"><img src="Material/Asteroids_ClassDiagram.jpg" width="25%"></a>
  <a href="Material/Asteroids_ActivityDiagram-Moveable.jpg"><img src="Material/Asteroids_ActivityDiagram-Moveable.jpg" width="25%"></a>
  <a href="Material/Asteroids_ActivityDiagram-Asteroid.jpg"><img src="Material/Asteroids_ActivityDiagram-Asteroid.jpg" width="25%"></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_QHHUCSHA"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L10 Inheritance: Class diagram</a>
</div>

## Implementation Moveable, refactoring Asteroid  
<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_PJXZSGZX"><img src="../X01_Appendix/Img/L10_V2_Bild.jpg" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_PJXZSGZX"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L10 Inheritance: Implementation 1</a>
</div>

> **`extends`** extends a superclass with a subclass!
> **`super(...)`** calls the superclass constructor from the subclass!  
> **`super.method(...)`** calls the superclass's `method` from the subclass!

## Polymorphism
Greek *polýs* means "much" or "many," and *morphé* means "form" or "shape." Dogs come in a wide variety of forms—a topic already covered extensively in these lessons. However, individual dog breeds not only look different but may also behave differently. Upon seeing a flock of sheep, a hunting dog might be inclined to rush in and bring one down, whereas a herding dog would be more likely to circle the flock and keep it together. For someone walking these dogs—such as the estimated 200,000 professional dog walkers in the USA—the primary concern is having enough leashes and collars to keep the dogs under control. The breed is irrelevant.

> **Polymorphism** is the fourth principle of object orientation!

Two classes that inherit from Moveable are currently implemented in the Asteroids program—in other words, two subclasses of the Moveable superclass. Put differently, Moveable currently takes three forms: Moveable, Asteroid, and Projectile. They differ in appearance and behavior.

| Class | Appearance | Movement |
|---|---|---|
| Moveable | None | Straight-line with wrapping at the edges |
| Asteroid | Rock fragment | Same as Moveable |
| Projectile | Small dot | Same as Moveable, but with limited range |

To enable the class instances to move and render, the main program calls their `move` and `draw` methods. The main program is completely indifferent as to whether these are asteroids or projectiles, provided they are `Moveable` objects meaning they have either inherited these methods or implemented (overridden) them. The main program can take them all for a walk...

According to the Liskov Substitution Principle, the following must be possible, even if Moveable is polymorphic:

```typescript
let m: Moveable = new Moveable();
let a: Moveable = new Asteroid(1);
let p: Moveable = new Projectile(new Vector(0, 0), new Vector(0, 0));
```

Note that the variables m, a, and p were all declared with the type Moveable, but they reference different object types.

- [x] Implement these three lines and output the variables to the console or view them in the debugger.

If the preceding step is possible, then the following must also be possible:

```typescript
let moveables: Moveable[] = [];
moveables.push(new Asteroid(1));
moveables.push(new Projectile(new Vector(0, 0), new Vector(0, 0)));
```

- [x] Verify this as well with a corresponding implementation.

## Activity diagram main program  

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_TAIUWVMJ"><img src="../X01_Appendix/Img/L10_V3_Bild.jpg" width="25%"/></a>
  <a href="Material/Asteroids_ActivityDiagram-Main.jpg"><img src="Material/Asteroids_ActivityDiagram-Main.jpg" width="25%"></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_TAIUWVMJ"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L10 Inheritance: Activity diagram main program</a>
</div>

## Implementation using polymorphism

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_IVAAHJUM"><img src="../X01_Appendix/Img/L10_V4_Bild.jpg" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_IVAAHJUM"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L10 Inheritance: Implementation 2</a>
</div>

> **`a instanceof B`** checks whether the object `a` is an instance of the class `B`.

- [x] Design the UFOs! In doing so, you must make the following design decision, among others:
  - Who is supposed to generate the projectiles? Will this task be handled by the UFOs or by the main program?
    - In the case of the UFO: how do the projectiles get into the Moveable array to be managed?
    - In the case of the main program: how does the main program detect that the UFO is firing a projectile? 

>**Note:** The path for drawing the UFOs is already generated by the `createUfoPath()` function. 

## L02 Events
# L02.1 Basics
[<img src="Material/EventInfo_Child2Parent.png">](https://www.youtube.com/watch?v=IHbQdC5VHfw)
<small>Quelle: <a href="https://www.youtube.com/watch?v=IHbQdC5VHfw">https://www.youtube.com/watch?v=IHbQdC5VHfw</a></small>

<!-- 
## Rückblick DOM
Lädt der Browser eine Datei und versucht diese als HTML-Datei zu interpretieren, baut er anhand der Daten im Speicher ein Document-Object-Modell (DOM) auf. Was schließlich im Browserfenster angezeigt wird ist also nicht ein direktes Abbild der Datei, sondern ein Abbild dieses internen Speichermodells.
- [x] Erzeuge eine einfache Textdatei mit der Endung ".txt" im Dateinamen und schreibe einige Worte hinein, auch mit mehreren Leerzeichenfolgen, Umlauten und Tabulatoren. Lade diese Datei im Browser und schaue dir in den Entwicklertools die Seitenstruktur an (Tab links neben Console)   

Es wird deutlich, dass ein `html`-Element enstanden ist und darin ein `head`-Element sowie ein `body`-Element. In letzterem ist irgendwo, wahrscheinlich in einem `pre`-Element, unser eigentlicher Text vergraben.
- [x] Ändere die Endung in ".html" und lade die Datei erneut. Was hat sich verändert?

### DOM-Manipulation
Ein Skript kann das DOM manipulieren, darin Elemente verändern, hinzufügen oder löschen, der Browser kümmert sich automatisch um die Darstellung für den User. Das wurde im ersten Modul dieser Reihe bereits umfassend geübt und auch in der vorangegangenen Lektion wiederholt.
>**Achtung:** Die Begriffe Objekt, Element und Knoten können teilweise synonym verwendet werden, es ist aber Vorsicht geboten. 'Alles' in JavaScript/TypeScript ist Objekt, auch etwas vom Typ `number` oder `string`, trotz der scheinbaren Primitivität. Ein Knoten (Node) ist ein Objekt mit speziellen Eigenschaften und Fähigkeiten, mit dem sich ein Graph aufbauen lässt. Ein Element wiederum ist ein spezieller Knoten, der Eigenschaften eines HTML-Elementes aufweist. Mit Element werden aber auch allgemein Einträge in Arrays bezeichnet. Der Kontext ist also entscheidend.
- [x] Wiederhole hierzu die Inhalte des Schaubildes zur [DOM-Klassenhierarchie](https://jirkadelloro.github.io/Code-Inverted/X01_Appendix/DO-Hierarchy/DOM-Classhierachy.svg)

### Baumstruktur
Das DOM lässt sich, wie auch z.B. die Aktivitätsdiagramme, als Graph mit Knoten, die mit Kanten verbunden sind, darstellen.
- [x] Suche die Klasse `Node` im Schaubild zur DOM-Hierarchie. Welche verwandtschaftlichen Beziehungen werden innerhalb der Klasse genutzt?  

Diese Knoten enthalten die Kernfunktionalität zur Bildung des Graphen und damit des DOMs. Jeder Knoten kann auf einen anderen Knoten als `parentNode` verweisen und auf eine Liste von `childNodes`. Im DOM ist `document` der Wurzelknoten, der lediglich eine Referenz auf `html` in seiner Kinderliste hat. `html` referenziert über die Eigenschaft `parentNode` das `document` und hat in seiner Kinderliste Referenzen auf `head` und `body`. `body` wiederum referenziert `html` als Mama bzw Papa und hat wieder verschiedene Kindreferenzen, je nach Inhalt der darzustellenden Seite. Damit ergibt sich eine Baumstruktur, die sich in der Tiefe immer weiter verästeln kann und mit Hilfe der Entwicklertools, wie oben bereits getan, leicht einsehen lässt.
- [x] Wähle aus deinen eigenen vorangegangenen Arbeiten eine kleine Webseite aus und stelle deren DOM grafisch dar.
-->

## Events
The DOM provides a system for interacting with the user: the event system. It very conveniently supplies information about events within the application, without requiring any knowledge of the hardware. The operating system and the browser already evaluate these events and present the information in a general, standardized form.

### Event Object
Events are special objects that carry information about an occurrence. Such an occurrence can be a mouse click, a key press, a touch on the screen, the loading of a file, the completion of a data transfer, and much more.
- [x] In the DOM class diagram, several event classes are listed. Find them and determine what information they carry.

### Target
Usually, an event refers to a specific object—for example, the button that was clicked, the link that was touched, the window that finished loading, or the text field that was changed. The `target` property of the event object provides a reference to this target object.
- [x] Of what type is `target`? Check the class diagram…
- [x] Objects of which classes can therefore be `target`s? 

### Type
`type` is a simple string that indicates what kind of event is being described. For example, predefined values include `click`, `load`, `change`, `dragstart` and many others. It is also possible to define your own, new event types.
- [x] Do some research. Find out which kinds of events the browser makes available!

## Event-Handler
Handlers are functions that evaluate an event. Working with them is very straightforward.

### Handler-Implementation
To evaluate an event, you simply implement a function whose signature follows this pattern:
```typescript
function handlerName(_event: Event): void {
    ...
}
```
The function thus accepts a parameter of type `Event`, in this example, the parameter is named `_event`. The name of the function can be chosen freely, but it is recommended to use the prefix handle or the abbreviation hnd, for example handleClick, because a function that processes an event is called a handler.

### Listener-Installation
For the system to know which handler should be called for which event, the handler must be registered. This is done with the statement `addEventListener(...)`, for example:
```typescript
document.addEventListener("click", handleClick);
```
The first parameter is the string that describes the event type, the second is a reference to the handler. When the document object receives an event object of type "click", that event object is passed on to the handler function `handleClick`. The `document`-Object now listens to the system; an “ear” called a listener has been installed on it.
>**Attention:** A common mistake in JavaScript is to pass a function call instead of a function reference, for example `addEventListener("click", handleClick())`. The extra parentheses cause the function to be called immediately during installation, and whatever it returns is then installed as the handler reference.

### Example
The following is probably the simplest example we can show using the event system:
```typescript
namespace L02_Load {
    window.addEventListener("load", handleLoad);

    function handleLoad(_event: Event): void {
        console.log(_event);
    }
}
```
Here, the `window`-Object, which corresponds to the browser tab in which the application runs, is instructed to call the function `handleLoad` when a load event occurs, and to pass the corresponding event object to it. handleLoad then simply outputs the object to the console.
- [x] Examine the `event`-Object that is logged.
- [x] Research the `load`-Event, wwhen exactly is it fired? What is the difference compared to `DOMContentLoaded`?
- [x] Install the listener on the `document`object instead of the `window`-object. What happens now?
- [x] Experiment in the same way with `DOMContentLoaded`. How does the system behave now?

> **Note:** To delay the execution of a script, the HTML attribute `defer` is now also available. However, it is good practice and helps avoid side effects to continue working with the load-related events.

## Event Phases
Not all events are reported to all objects in the system. It only makes sense to install listeners where they can actually take effect. Things become particularly interesting for interactions performed on DOM objects, such as clicking a button. These events are passed through the DOM graph in three phases.
 
![](./Material/Event.svg) 

### Phase 1: Capture
First, the event object is passed to `window`. From there, it travels to `document`, then to `html`, then to `body` and further down the tree in the direction of the `target`.
### Phase 2: Target
When it is passed from the parent object to the `target`, for example to the clicked button element, the event object is said to be in the target phase.
### Phase 3: Bubble
Finally, the event object travels back up the tree until it reaches `window` again. It rises like an air bubble in water up to the surface.

### Listener-Options
When installing a listener, a third parameter can be used to provide information about how it should behave. If a simple `true` is passed here, the listener reacts during the capture phase. Otherwise—which is more common—it reacts during the bubble phase. In either case, it also reacts during the target phase.

### CurrentTarget
In addition to `target`, the event object also carries a reference to the object whose listener most recently heard the event. With `currentTarget`, you can therefore determine where in the DOM the event is currently located and being processed.

### Path
The complete path that the event takes through the DOM can be inspected via the `path` attribute or determined in code using the method `composedPath()`.

### Example
- [x] Examine the page [Phases](../X00_Code/L02_Events/Phases) and run the code.
- [x] What happens when you click on the button, when you click to the right of it, and when you click below it? Why?

## Custom Events
In addition to the events that are automatically created and dispatched by the system, it is also possible to explicitly create and dispatch events in your own code. You can define new event types and attach arbitrary information in the `detail` property.
```typescript
// define a custom event that bubbles and carries some information
let event: CustomEvent = new CustomEvent("someSpecialType", {bubbles: true, detail: {someKey: someData}});
// send the event from some dispatcher
someEventTarget.dispatchEvent(event);
```
All identifiers in this example that start with "some" can and should of course be replaced with meaningful names.  

# L02.2 Event-Driven Application
<img src="Material/erpresserbrief.jpg" width="50%">
<figcaption><small>Quelle: https://www.paketda.de/juniorpost/erpresserbrief-basteln.php</small></figcaption>

Creating ransom notes by hand is tedious. So it becomes an interesting business idea to develop a web app that allows blackmailers to handle their correspondence in no time. How do we approach this?

## Use Case Diagram
With the help of a use case diagram, you first create a rough overview of the requirements for your application. This is very quick and greatly helps with the conceptual design. 

<div align="center">
   <a href="http://hdl.handle.net/10900.3/OER_CXSBWOSW"><img src="../X01_Appendix/Img/L02_V1_Bild.JPG" width="25%"/></a>
  <a href="Material/BlackmailerCompanion_UseCaseDiagram.jpg"><img src="Material/BlackmailerCompanion_UseCaseDiagram.jpg" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_CXSBWOSW"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L02 Events: Anwendungsfalldiagram</a>
</div>

## Sketch: User Interface
Next, you create a sketch of the appearance of the application. This will already reveal quite a bit about the required underlying structure. You annotate the sketch with the HTML tags and attributes that seem appropriate. Distinguish between static and dynamic elements and properties. For the dynamic parts, you already note where listeners should be installed and which events should be linked to which activities. Check whether all interaction possibilities required to realize the use cases are covered.

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_BOIFLJAE"><img src="../X01_Appendix/Img/L02_V2_Bild.JPG" width="25%"/></a>  
  <a href="Material/BlackmailerCompanion_UI-Scribble.jpg"><img src="Material/BlackmailerCompanion_UI-Scribble.jpg" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_BOIFLJAE"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L02 Events: UI-Scribble</a>
</div>

## Activity Diagrams
At this point, you have already captured the key activities, the involved elements, and the events to be evaluated from the user’s perspective. Now you switch to the system’s perspective and define how it should operate. For this, you now use activity diagrams. Each event serves as a signal reception that starts an activity.
Begin again with an overview of the activities. Then, take each activity in turn and refine it. Repeat this process until you arrive at atomic actions that can be implemented directly as program statements. At first, you will probably need to research these statements.  
<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_HFILWFPA"><img src="../X01_Appendix/Img/L02_V3_Bild.JPG" width="25%"/></a>
  <a href="Material/BlackmailerCompanion_ActivityDiagram.jpg"><img src="Material/BlackmailerCompanion_ActivityDiagram.jpg" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_HFILWFPA"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L02 Events: Aktivitätsdiagramm1</a>
</div>

## Implementation
Once you feel that your conceptual design covers everything needed for a first implementation of the application, you can start coding.

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_UTUDZVWF"><img src="../X01_Appendix/Img/L02_V4_Bild.JPG" width="25%"/></a>
  <a href="Material/BlackmailerCompanion_ActivityDiagram2.jpg"><img src="Material/BlackmailerCompanion_ActivityDiagram2.jpg" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_UTUDZVWF"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L02 Events: Aktivitätsdiagramm2</a>
</div>

## Testing
During implementation, the program is tested repeatedly. It is important to implement in such a way that errors are not only discovered in a final test, but that you always have runnable intermediate versions which are stored in the code repository.  

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_GUVDTCKB"><img src="../X01_Appendix/Img/L02_V5_Bild.JPG" width="25%"/></a> 
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_GUVDTCKB"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L02 Events: Testing</a>
</div>

## Iterative Workflow
Keep in mind that you can and should improve your conceptual design at any time if you notice that something is missing or does not work. The same applies to all previous steps. For example, considerations during the UI sketch may influence the use case diagram, or insights gained during implementation may lead to changes in the sketch, which in turn affect the activity diagrams. It is unlikely that the first conceptual approach can be carried out unchanged and lead directly to a perfect application.
> **Attention:** What you must not do is make major changes at the last stage—implementation—without first revising the conceptual design. This approach will almost certainly lead to **failure**!

## Exercises
- [x] Solve the problem that occurs when placeLetter is called again after deleteLetter
  - First experiment with solution options 1 and 2
  - Then plan solution 3 and implement it
- [x] Extend the ransom note app so that it can also be used on smartphones
  - In an additional area, all permitted letters should be available for selection so that they can be activated by touch
  - The currently selected letter should be highlighted in this selection so that the user knows what they have chosen before changing the note
- Note: Research the Pointer Event

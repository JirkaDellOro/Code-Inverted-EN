# L03 FormElements
<img src="Material/PassierscheinA38.png">
<small>Sources: <a href="https://mueli77.com/2016/12/11/asterix-review-special-45-12-pruefungen-fuer-asterix/">https://mueli77.com/2016/12/11/asterix-review-special-45-12-pruefungen-fuer-asterix/</a></small>

# L03.1 Overview
Interaction has been a core aspect of HTML from the very beginning. The World Wide Web is based on the idea of linking scientific documents with interactive references, the links. The focus was only much later shifted to cat videos.

> Fun fact: Tim Berners Lee ([GitHub profile](https://github.com/timbl)) developed HTML, HTTP, URLs, the first web server and the first browser in the early 1990s. You can get an impression of what it felt like back then in an interactive way here: [WWW](http://line-mode.cern.ch/www/hypertext/WWW/TheProject.html)

Very early on, HTML elements of different types were also developed that allow a user to enter information. These are still used today to build forms. The elements automatically react to user interaction and provide visual feedback without any additional programming. For basic functionality, plain HTML is sufficient.

In this chapter you will get to know the most important elements, learn how to use them and what information they provide. Similar elements can also be found in other development environments for designing user interfaces. 

> - [x] Get the code [L03_FormElements/Overview](https://github.com/JirkaDellOro/Code-Inverted/tree/master/X00_Code/L03_FormElements/Overview) from the appendix running locally. In this lesson you will gradually enable and examine its functionality. At first only the heading "Form-Elements" appears.

## Text Input
The most important input option is text. Using keyboard event, you could intercept every character as you type and build up a string, but there is a much simpler way. The generic `input` element already provides this basic functionality in its default form as a text field, and it can do much more.
> Example: <input type="text" placeholder="Type something here" required/>

### Placeholder
The `placeholder` attribute can be assigned a value that can serve, for example, as a prompt for the user. As soon as the user begins typing, the placeholder automatically disappears and reappears when the field is cleared.

### Password Input
The value "password" for the `type` attribute ensures that the input is not shown and cannot be copied. 

### Validation
For example, specifying "email" or "url" as the value for the `type` attribute already performs a simple validation of the entered string to detect obvious input errors and show that the value is invalid. If the ‘valueless’ attribute `required` is specified, it additionally indicates that input is mandatory. The visual appearance can be adjusted using CSS and the pseudo-selectors  `:invalid`, `:valid` and `:required`. 
Much more complex validation can be performed using regular expressions, which however requires some expertise. For this, the `pattern` attribute is used to define the regular expression to be applied. In the example, exactly 3 digits are required.

### Textarea
A larger variant of the `input` element is `textarea`. Here, longer texts can be entered over multiple lines; when the content overflows, a scrollbar appears, and the user can, if necessary, resize the field.

### Exercises
> - [x] Now activate in the HTML file the fieldset with the `id` "fsText". To do this, remove the surrounding comment markers`<!--` and `-->`. The easiest way is to select the whole block and then press `Ctrl+#` (Mac: ⌘+K ⌘+U). Save the file.
> - [x] The fieldset labeled "Text-Input" should now appear when you reload the file in the browser. Check how the elements behave and trace how this behavior comes about. Keep in mind that no script is running yet. 
> - [x] Now activate the script "FormElements.js"  in the HTML file. Continue interacting with the elements and observe the output in the console. When is the `input` event dispatched, and when is `change` dispatched?
> - [x] Examine the code in the file FormElements.ts and clarify how it works.

## Selektionen
Very often, the user should be able to choose between different options without entering text. For this, the options must be presented and a clear way of interacting must be offered. HTML provides various elements and element types for this.

### Datalist
With datalist, a text field can be assigned a list of predefined entries. The user can select such an entry via pointer interaction or restrict the selection by typing on the keyboard. However, they can still enter arbitrary free text and ignore the selection. The connection between `input` and `datalist` is made via the attributes `list`and `id`.  

### Radiobutton
Example: <input type="radio" name="radio"><input type="radio" name="radio" checked><input type="radio" name="radio">  
This type is specifically designed for single‑choice selections. All `input` elements of type `radio`that share the same value in the `name` attribute are treated as a group. Only one element of the group can be selected.

### Checkbox
Example: <input type="checkbox"><input type="checkbox" checked><input type="checkbox" id="c3">  
`input` elements of type `checkbox`, on the other hand, are typically used for multiple‑choice selections.

> **Attention:** Radio buttons and checkboxes usually refer to a text next to them. For the user, it is very annoying if this relationship is not used to support interaction—i.e. if they cannot click the text to interact. It is extremely easy to take this into account: the text just has to be wrapped in a `label` tag. The connection between `label` and `input` is then made via the attributes `for` and `id`. This should **always** be implemented!

### Select
Example: <select name="Select" id="select">
<option value="option1">1.</option>
<option value="option2" selected>2.</option>
<option value="option2" selected>3.</option>
</select>  

The `select` element, is the most powerful one, also known as a `dropdown menu` or `combo box`. In a small amount of space, it can offer a predefined set of options as a single‑choice or multiple‑choice selection. It can even be used to create sub‑groups of options.

### Exercises
- [x] Now activate the second fieldset; it has the `id` "fsSelection". Experiment in the browser with the selection elements.  
- [x] Observe in the console exactly how element names and values are matched. What do you notice? Where do the values come from? Compare with the text inputs.
- [x] Now activate in the script the code marked with "// Handling checkbox". Why is this code here—what is its purpose? Observe the output in the browser console.
- [x] Add the ‘valueless’ attribute `multiple` to the `select` tag. In the browser, select several options there while holding down the Ctrl or Shift key. Again observe the output and record your observations. 

## Further Input Elements
There are also some input elements that behave completely differently. Their basic functionality is, however, very easy to understand.

- [x] Activate the fieldset with the `id` "fsSpecial". Experiment with it: change attribute values and interact with the elements in the browser. Observe the console output.  
- [x] Answer the question about the effect of the `value` attribute.  

> **Note**: It now becomes very clear how dramatically the Input element changes its appearance by default depending on its type attribute. This makes it an "all‑purpose weapon". In addition to the attributes `name` and `value`, input elements also have the attributes `readonly` and `disabled`. In that case, users can no longer change the content, with `disabled` they cannot even focus the element or copy any content.  

## Output Elements
There are also a few standardized elements that provide feedback to the user. They serve the interaction from the system back to the user.

- [x] Activate the last fieldset, it has the `id` "fsOutput". 
- [x] Also activate the last code fragments. Explain how the output elements are controlled/addressed. 

# L03.2 Cocktail Bar 

<img src="Material/CocktailBar.png"/>

How often do you sit at home in the evening and wish you had a really delicious cocktail? But the ingredients are missing, it is too much effort to get started, and you do not want to go out anymore? The next brilliant business idea practically suggests itself: the **online cocktail bar!**  

Customers can spontaneously order a cocktail here via a web form, which will be delivered to their homes by mail a few days later.

## Use-Case-Diagram
Quickly draw a small use case diagram again. Try it yourself before you watch the video. 

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_ROTBOJQE"><img src="../X01_Appendix/Img/L03_V1_Bild.JPG" width="25%"/></a>
  <a href="Material/CocktailBar_UseCaseDiagram.jpg"><img src="Material/CocktailBar_UseCaseDiagram.jpg" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_ROTBOJQE"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L03 FormLements: Use Case Diagram</a>
</div> 

## Sketch: User Interface 
The user interface is now becoming a bit more complex. Create a sketch and think about which tags, attributes and listeners you need. Invest a little time in designing an initial draft yourself before you watch the video.

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_ZVWFIXVL"><img src="../X01_Appendix/Img/L03_V2_Bild.JPG" width="25%"/></a>
  <a href="Material/CocktailBar_UI-Scribble.jpg"><img src="Material/CocktailBar_UI-Scribble.jpg" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_ZVWFIXVL"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L03 FormLements: UI-Scribble</a>
</div>

## Activity Diagrams
At this point, you have already roughly designed what should happen and how it should be presented. Now develop an activity diagram again to define how everything works. Start again from the events and move from the coarse to the fine. At the beginning, do not worry too much about details; instead, break large problems down into smaller ones. Try it yourself before you watch the video.

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_VUTEAUWS"><img src="../X01_Appendix/Img/L03_V3_Bild.JPG" width="25%"/></a> 
  <a href="Material/CocktailBar_ActivityDiagram.jpg"><img src="Material/CocktailBar_ActivityDiagram.jpg" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_VUTEAUWS"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L03 FormLements: Activity Diagram</a>
</div>

## Implementation I
Now try your hand at the implementation. Watch the video at the latest when you start working on the order summary. There are a few more tips there.

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_PSXQYBNK"><img src="../X01_Appendix/Img/L03_V4_Bild.JPG" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_PSXQYBNK"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L03 FormLements: Implementation I</a>
</div>

# FormData
JavaScript provides a simple mechanism to automatically evaluate form elements and access the results. For this, the form elements must be subordinate to a `form` element in the DOM runtime hierarchy. If, when creating an object of type `FormData`, a reference to this form element is passed, the values of the `name` and `value` attributes are made available as key–value pairs.
```typescript
let formData: FormData = new FormData(document.forms[0]);
```
The `document`object conveniently provides a list of all subordinate `form` elements. In the example above, the first form in the document is evaluated.

## `name`-Attribute
Up to now, the `name` attribute has only played a role in the grouping of radio buttons. In fact, for the automatic evaluation of forms it is the decisive criterion for mapping.
> **Attention:** it is not the `id` attribute that is relevant for form elements, but `name`. 

It is also important to note that, unlike `id`, the `name` attribute does not have to be unique. When evaluating, the same name can therefore appear multiple times as a key, each time with different values. 

## Reading Out
### `get(...)`
For unique and known names, the values of the `value` attributes can be read using the `get(...)` method of the object, similar to an associative array, for example:
```typescript
console.log(formData.get("Drink"));
```
### `entries()`
However, since the names are not necessarily unique and should not necessarily be reproduced in the code, it is often useful to iterate over all entries in the FormData object.
The `entries()` method of a FormData object returns all found key–value pairs. With a `for..of` loop these can conveniently be processed one after another:

```typescript
for (let entry of formData.entries()) {
    console.log(entry);
    console.log("name: " + entry[0]);
    console.log("value: " + entry[1]);
}
```
Each `entry` object is thus a small array with two entries: at position 0 a string containing the value of the `name` attribute of the original form element, and at position 1 the corresponding value of the `value` attribute. The latter is of the type `FormDataEntryValue`, a somewhat special string, and may need to be converted.

## Implementation II

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_UDZAJFTH"><img src="../X01_Appendix/Img/L03_V4_2_Bild.JPG" width="25%"/></a>
  <a href="Material/CocktailBar_UI-Scribble2.jpg"><img src="Material/CocktailBar_UI-Scribble2.jpg" width="25%"/></a>
  <br/>
  <a href="http://hdl.handle.net/10900.3/OER_UDZAJFTH"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L03 FormLements: Implementation 2</a>
</div>   

> Attention:   
> - In the video, the term FormData element is used several times. It is better to say FormData object, because it is not an HTML element.
> - In the video, the shorter form `for (let entry of formData)` is also used, which is allowed. However, the version given above, `for (let entry of formData.entries())` is cleaner.

- [x] The output of the order overview does not yet work correctly. Find the error and fix it.
- [x] Implement the special handling of the drinks.
- [x] Implement the calculation of the total sum.
- [x] Review the design and, if necessary, correct the diagrams based on your experiences. 

Note: Inaccuracies in the calculation of floating-point numbers can very quickly lead to ugly number representations. However, the `number` type has several methods for converting a number into a formatted string. Research these methods and use them.

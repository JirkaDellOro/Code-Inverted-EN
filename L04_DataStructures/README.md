# L04_DataStructures
<img src="Material/OrdnungChaos.jpg">
<small>Source: <a href="https://gutezitate.com/zitate-bilder/zitat-ordnung-ist-etwas-fur-primitive-das-genie-beherrscht-das-chaos-albert-einstein-181618.jpg">https://gutezitate.com/zitate-bilder/zitat-ordnung-ist-etwas-fur-primitive-das-genie-beherrscht-das-chaos-albert-einstein-181618.jpg</a></small>

Albert Einstein was undoubtedly one of the greatest geniuses humanity has ever produced. It is unlikely to offend anyone if, in this lesson, we assume that none of the people involved enjoys a similar status. This probably applies to most of those involved in designing interactive applications, and perhaps even more to the users of these applications. So anyone who does not claim Einstein’s genius for themselves might still benefit from order.

- **FunFact**: Another quote attributed to Einstein, which should not go unmentioned in this context, is: “Genius is 1% talent and 99% hard work ..."  

## Separation Of Concerns
Just as undisputed as Einstein’s genius is the principle of Separation of Concerns (SoC) in software development. This principle is about ensuring that each part of the overall software has a clear responsibility that is separated from the responsibilities of the other parts. This responsibility should be describable in a short sentence. If the word “and” appears in that sentence, something may be wrong, and that part has been assigned too many responsibilities. For the cocktail bar, we have already done many things right, especially if we look at it from a very high-level perspective and consider the basic separation required for developing web applications.
- [x] Work through this short [article in SelfHTML.org](https://wiki.selfhtml.org/wiki/HTML/Tutorials/Trennung_von_Inhalt,_Pr%C3%A4sentation_und_Verhalten). Compare it with your own work and check whether you have taken everything into account.

The CSS part of the cocktail bar affects only the visual design and does not add any content or interaction possibilities. The script deals exclusively with evaluating the interaction. The HTML file provides the structure of the page and the interaction elements and serves as the data source... oops, there are quite a few **AND**s in there. And that is where things will soon start to grind.

## Separation of Structure and Content
SelfHTML states that the HTML document should provide structure and content. This fits quite well for static pages whose content will never change significantly. However, the Blackmailer Companion was already a good counterexample. There, the essential content only emerges through interaction with the user and neither can nor should be statically defined in the HTML file. The HTML file only defines the rough structure. It is still true that what is created with the help of the HTML file—namely the DOM—continues to provide structure and content, but is itself dynamic and manipulated by the script.

Combining structure and content in the cocktail bar is problematic, because the bar’s menu and prices can change. And assuming that the bartender is not one of the developers of the online cocktail bar, the question arises of how the changed menu is supposed to find its way into the app. Is the operator supposed to tinker around in the HTML code themselves? And risk the whole app no longer working because of a typo in an attribute? This option must be ruled out.

This means that the existing separation of responsibilities must be extended by separating the content from structure, behavior and presentation, so that the content can be changed without having to change the other responsibilities or accidentally changing them.

## Extension of the Use Case Diagram
Two things follow from the above:
- there is another actor, the bartender
- they must be given a way to adjust the menu without interfering with the actual application  

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_GFOXPPRS"><img src="../X01_Appendix/Img/L04_V1_Bild.JPG" width="25%"/></a>
<a href="Material\CocktailBar_UseCaseDiagram.jpg"><img src="Material\CocktailBar_UseCaseDiagram.jpg" width="25%"/></a><br/>
<a href="http://hdl.handle.net/10900.3/OER_GFOXPPRS"><img src="../X01_Appendix/Img/video.jpg" width="3%"/>L04 Use Case Diagram</a><br/>
</div>

## Algorithms and Data Structures
Besides the principle of “Divide et impera”, it is generally a very good approach in application design to distinguish between algorithms and data structures. Taken together, the two form the application. On the one hand, identify and examine the information to be processed, and consider how this information should be structured so that it can be processed well. From this, the data structures arise. On the other hand, think about how this information should be processed; by doing so, you design the algorithms.  

<img src="Material/AdaLovelace.jpg" align="right"/>

- **FunFact**: many sources attribute the first algorithm ever designed for a computer to Lady Ada Lovelace (1815–1852). The associated mechanical computer, the “Analytical Engine”, could not yet be built in a functional way at that time. But Ada’s algorithm worked! In her honor, more than a century later, a popular programming language was named “Ada”.

The design of algorithms and data structures thus forms the basis for the subsequent programming. Starting the programming without this foundation is an erratic approach and, for more complex applications, usually leads to increased effort and inferior solutions.

Considering them separately leads to good data structures that can also be processed by algorithms that have not yet been planned for other purposes, and to algorithms that in turn can process data that is not yet known.

# Cocktail Bar: Data Structure
The cocktail bar works quite well, but it is completely inflexible and leaves the bartender out of the loop. It is therefore desirable that the page essentially stays the same, but is not derived from a static HTML file; instead, it should be generated dynamically based on data that the bartender can maintain themselves. A simple option is to provide them with a special, simply structured text file into which they can enter their data following a clear pattern. So they should not be confronted with chaos that demands genius, but with order.

<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_MRBDUWIR"><img src="../X01_Appendix/Img/L04_V2_Bild.JPG" width="25%"/></a>
<a href="Material\CocktailBar_DataStructures.jpg"><img src="Material\CocktailBar_DataStructures.jpg" width="25%"/></a>
  <br/>
<a href="http://hdl.handle.net/10900.3/OER_MRBDUWIR"><img src="../X01_Appendix/Img/video.jpg" width="3%"/>L04 Datenstruktur</a>
</div>

## Generation of the Form
Now the data structure is defined, and we also know how the finished form should look and function, and it has already been tested. So now an algorithm must be developed that generates the form using the data. To do this, we must first decide which parts of the DOM are to be generated automatically when the page loads by interpreting the HTML file, and which parts are then added dynamically by the script.

In theory, it is of course possible to omit a description of the page structure in the HTML file entirely, implement only a reference to the script, and leave DOM creation completely to the script that is called in this way. On the one hand, however, this would violate the SoC principle, because too much responsibility would be assigned to the script; on the other hand, an HTML file that describes the structure already exists.

It therefore seems more sensible to remove only the descriptions of the dynamic structures from the existing HTML file and reduce it to the static ones. This is a classic design decision and, for this example, is made in the sense described above.


<div align="center">
<a href="http://hdl.handle.net/10900.3/OER_RMJGXPFC"><img src="../X01_Appendix/Img/L04_V3_Bild.JPG" width="25%"/></a>
<a href="http://hdl.handle.net/10900.3/OER_RMJGXPFC"><br/><img src="../X01_Appendix/Img/video.jpg" width="3%"/>L04 Generierung des Formulars</a> <br/> 
<a href="Material\CocktailBar_UI-Scribble.jpg"><img src="Material\CocktailBar_UI-Scribble.jpg" width="25%"/></a>
<a href="Material\CocktailBar_ActivityDiagram.jpg"><img src="Material\CocktailBar_ActivityDiagram.jpg" width="25%"/></a>
<a href="Material\CocktailBar_ActivityDiagram_generateContent.jpg"><img src="Material\CocktailBar_ActivityDiagram_generateContent.jpg" width="25%"/></a>
</div>

> - [x] Design the two other activities for creating the interaction groups as well.  

## Implementation I

<div align="center"> 
  <a href="http://hdl.handle.net/10900.3/OER_DOUCTCEZ"><img src="../X01_Appendix/Img/L04_V4_Bild.JPG" width="25%"/></a><br/>
<a href="http://hdl.handle.net/10900.3/OER_DOUCTCEZ"><img src="../X01_Appendix/Img/video.jpg" width="3%"/>L04 Implementation 1</a>  
</div>

## Implementation II
<div align="center">
  <a href="http://hdl.handle.net/10900.3/OER_TPHTKHOI"><img src="../X01_Appendix/Img/L04_V5_Bild.JPG" width="25%"/></a><br/>
<a href="http://hdl.handle.net/10900.3/OER_TPHTKHOI"><img src="../X01_Appendix/Img/video.jpg" width="3%"/>L05 Implementation 2</a>  
</div>

## Refactoring
### Incorporating experiences  
> - [x] Reflect in the concept the corrections that resulted from the first implementation. Design error messages as well, so that the bartender gets a hint when something does not work because of incorrect input.

### Completing  
> - [x] Revise and complete the implementation according to the improved concept. Also adjust the element types to be used and, if necessary, the stylesheet.
> - [x] Outsource the interfaces into a separate file so that the bartender does not accidentally tamper with them.  

### Separation of Concerns
If we now look at the code in the HTML file, the script, and the data file, we notice that the categories of the cocktail bar’s menu appear everywhere and are used not only for labels but also for controlling the program. This contradicts the SoC principle!
> - [x] Revise the concept of the cocktail bar so that the names of the menu categories do not appear in the HTML file, the script, and the data file. They should only be contained in the data file. This is also where the information about the desired form of interaction for each category must be stored. To achieve this, it will be useful to create a dedicated data structure for the categories that contains more information.


## JSON (JavaScript Object Notation)
Of course, this solution is not necessarily the ultimate wisdom when it comes to keeping the menu maintainable for the bartender. Because of the specification of the `namespace` and the declaration as well as the assignment operator `let data: Data = `  is still a script file, and if the bartender handles it incorrectly, they can easily render the program unusable. Without these specifications, however, it would already be a valid JSON file, and in the next lessons it will be explained how to work with it and how to use pure JSON as a data and data transfer format.
Interestingly, JSON is now also used extensively outside the JavaScript world in completely different kinds of applications. In the meantime it has established itself as a competitor to **XML (Extensible Markup Language)**, whose structure also underlies HTML, and in terms of public interest—at least as far as can be determined using Google Trends—has overtaken XML since spring 2016.

<img src="Material/XMLvsJSON.png">  

XML is still a widely used standard and can be employed more efficiently than JSON in some applications.

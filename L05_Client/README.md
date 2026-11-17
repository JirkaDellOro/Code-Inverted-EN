# L05_Client
<img src="Material/ZitsFaceToFace.jpg">  
<small>Source: <a href="https://arnoldzwicky.org/category/these-modern-times/page/3/">https://arnoldzwicky.org/category/these-modern-times/page/3/</a></small>

The cocktail bar already works quite well as it is, but there is still one major drawback: the order is not yet being sent. As long as the bartender is not sitting next to the customer while they are putting together their cocktail, the business will be lost. However, this situation does not match the use cases we are considering and makes the system obsolete anyway.
Therefore, the web page that is displayed by the customer’s browser on their computer must communicate with another computer that receives the order, processes it, and finally communicates it to the bartender. If necessary, it should also provide feedback about the processing, which in turn is received by the customer’s browser and displayed to them.

- [x] Read this short [Wikipedia-article](https://de.wikipedia.org/wiki/Client-Server-Modell) to clarify some basic terms of the client–server model.

## Use Case Diagram
In fact, the cocktail bar already uses a client–server system as soon as it is published on GitHub Pages. Pages acts as a file server and delivers the HTML, CSS, and JS files that a browser can interpret. The browser acts as the client, and it does not matter on which computer it runs, as long as it can communicate with the server via an internet connection.
- [x] Compare this with the [EIA2-Landschaft](../X01_Appendix/Landscape/EIA2Landscape2022.svg). Which systems and processes are you already familiar with from EIA1 and EIA2? Draw a closed polyline that includes all of these and excludes all unknown processes and systems.

If we split the use case diagram so that it shows two systems, client and server, it becomes clear that
- the customer interacts with a different system than the bartender
- the order has to be transferred from one system, the client, to the other system, the server.  

![](Material/draw.io/UseCase.svg)  
> The file serving mentioned above is not shown here, since the browser and server already take care of this automatically.

## Communication
Communication is the process of exchanging or transferring information. There are two basic forms of communication that are distinguished. 
### Synchronous
![](Material/Synchron.svg)  

Synchronous means “at the same time”. In communication, however, this does not mean “simultaneous”, but “coordinated in time”. This means that the communication partners wait for each other until the other has transferred their information and then begin transferring in turn. Thus, the systems involved inevitably spend half of the time available waiting. In the diagram that shows the communication algorithm of one of the partners, this is indicated by the small loop.

- [x] Collect examples of synchronous communication in your own everyday life.

As small as this loop may seem, it can be a big problem. The systems could make sensible use of the waiting time instead of just staying in it. It becomes particularly problematic when a system needs a long time to respond. For example, if a website no longer reacts because the browser is waiting for an answer from the server, the user will soon abort the process.


### Asynchronous 
![](Material/Asynchron.svg)  
For this reason, asynchronous communication has become established. The communication partners send one or more requests; with that their part is initially done and they can take care of other tasks. When an answer to one of the requests is available, the requesting system receives a signal. This serves as the starting point for a new process in which the answer is first processed and, if necessary, further requests are sent.  

- [x] Collect examples of asynchronous communication in your own everyday life.
- [x] What are the particular challenges of asynchronous communication?

This makes it possible for websites, for example, to build up the display and provide interaction while further data are being loaded. Or to send data during interaction with the user.

## Forms
HTML forms already offer the possibility of sending data synchronously completely without additional programming. For this, it is only necessary to add an `action` and a `method` attribute to the form tag and to integrate a submit button in the form. The `action` value specifies the URL to which the data should be sent. The data themselves, i.e. the values of the form elements, are automatically prepared and sent when the submit button is pressed. How they are sent depends on the chosen `method`.
### Get
With `method="get"`, the data are simply appended to the URL in plain text as strings, separated by a `?`. The key–value pairs are separated from each other by `&` (et, ampersand), the keys and values are connected by `=`. The browser then calls this URL including the appendix, the complete string appears in the address bar.

- [x] Surf the web and look for examples of data transfer in this form by observing the address bar.
- [x] Extend the form element of your current cocktail bar implementation with the attributes `method="get"` and `action=""`. The empty value for `action` means that the current URL is to be used again. Additionally, insert a submit button in the form with
```html
<button type="submit">Submit</button>
```
, examine what happens when you now call the page and press the button.
> The type `"submit"` is the default for buttons and does not have to be specified explicitly. So every button inside a form for which no other type is explicitly specified will trigger sending the data. And this frequently leads to confusion among developers...

### Post
With `method="post"` the data is transmitted as separate data packets and is not visible to the user. However, the post method is primarily used when transmitting larger amounts of data, as there is a limit to the permissible length of a URL. On the server side, the data packets must first be reassembled into a single, coherent request.
- [x] Now use the `"post"` method for the form and click the submit button. Try to locate the data using the developer tools. To do this, click the "Network" or "Network Analysis" tab and then select the POST request from the list of requests.  

## Implementation of asynchronous communication
#### XmlHttpRequest-Events
Asynchronous communication is an ideal scenario for employing the now-familiar event-driven approach. JavaScript provides the standard `XMLHttpRequest` object for this purpose; such an object can be instantiated using the `new` keyword. The object offers methods that make it straightforward to establish a connection to the server and send a request. As the object manages the connection, it triggers specific events whenever the status changes; these events are captured using the standard listener pattern and passed to the corresponding handler functions. The complete transmission of the server response constitutes one such status change, thereby enabling the implementation of the aforementioned asynchronous communication pattern on the client side.
> The XML in the object's name reveals that it's a somewhat older technology, released by Microsoft in 1999 during the HTML4 standardization. However, it's widespread and still frequently used. [More information is available here.](https://wiki.selfhtml.org/wiki/JavaScript/XMLHttpRequest)

#### Fetch & Promises
The global JavaScript function `fetch` was introduced in 2017 as an interface for asynchronous communication. It allows a request to be sent and a response received using a single statement following the pattern `fetch(url [, data])`. However, due to its asynchronous nature, `fetch` does not immediately return the server's response; instead, it returns an object of the type `Promise`. This can be viewed metaphorically: the promise made by `fetch` entails an obligation to execute the actual function—in this case, to communicate—and to invoke specific subsequent functions depending on whether the operation succeeds or fails. It is, therefore, another form of event-driven programming, albeit one that appears in a slightly different guise.  

In the simplest case, the entire construct looks like this:
```typescript
    function communicate(_url: RequestInfo): void {
        // try to communicate
        let promise: Promise<Response> = fetch(_url);
        // establish the functions to call when communication 1. succeeds, 2. fails
        promise.then(handleSuccess, handleFailure);
    }

    function handleFailure(_response: Response): void {
        console.log("Failure", _response);
    }

    function handleSuccess(_response: Response): void {
        console.log("Success", _response);
    }
```
Here, too, functionality is distributed across multiple functions, making the overall structure difficult to follow. This situation is further compounded by the fact that the response object passed to the handlers by the Promise does not yet represent the raw data from the communication partner's response. Processing the response requires additional functions—which in turn generate Promises that invoke further handlers, and so on.
> **FunFact:** This creates a tangle of functions that developers refer to as "callback hell."

#### Async/Await
That is why the new keywords `async` and `await` were implemented in JavaScript in 2017. This enables something truly remarkable. Instead of modeling asynchronous processes using the synchronous programming paradigm—where one code statement executes strictly after the completion of the previous one—and events (which leads to "callback hell"), these two keywords make the programming itself asynchronous. As a result, the `fetch` example suddenly becomes extremely clear and readable:

```typescript
    async function communicate(_url: RequestInfo): Promise<void> {
        let response: Response = await fetch(_url);
        console.log("Response", response);
    }
```
The `async` keyword declares a function as asynchronous, meaning its execution can be paused and resumed at a later time. This is precisely what the `await` keyword does; consequently, it only makes sense to use it within asynchronous functions. Once `fetch` is initiated, the `communicate` function pauses, and program execution continues with the statement immediately following the function call. However, once `fetch` completes, execution of `communicate` resumes at the statement following the `fetch` call. This means that the main program—provided it has not yet finished running—is interrupted to handle outputting the response to the console. Afterward, `communicate` reaches its conclusion, and the main program resumes. However, the function could contain further `await` statements that trigger similar behavior. In this way, processing the response further does not result in code bloat caused by callbacks. 
- [x] Copy the asynchronous code above and embed it in a new program. Have the main program call the `communicate` function with the parameter "https://jirkadelloro.github.io/Code-Inverted/L05_Client/Material/Test.txt". Have the strings "Start" and "End" printed to the console before and after the call to `communicate`.
- [x] Now run the program after implementing it so that it is executable. Describe your observations. In what order does the output appear in the console?
- [x] The returned response object is complex, and the actual content of the server response is not yet visible. Extend the `communicate` function by adding a line that calls the text() method on the response object. Attention: text() returns a Promise; however, if you use `await` here as well, the final result will be a string that you can assign to a variable of type string. Then, output the content of the server response.

## Uniform Resource Locator (URL)
You now have a few tools at your disposal to initiate synchronous or asynchronous communication from the client side—that is, to send a request and receive a response from the server. So far, we have only considered the GET method, where the payload is embedded in the URL string. This step still needs to be implemented for asynchronous communication. To do this, we should first examine the structure of the URL in more detail.
- [x] Study the structure of the URL using the following graphic.
<img src="Material/URL.png">  
<small>Quelle: <a href="https://nodejs.org/dist/latest-v12.x/docs/api/url.html#url_url_strings_and_url_objects">https://nodejs.org/dist/latest-v12.x/docs/api/url.html#url_url_strings_and_url_objects</a></small>  

### Part 1
Protocol, authentication, and host are required to establish a connection to a server. However, simply specifying the hostname, such as www.hs-furtwangen.de, is often sufficient to enable communication between the browser and the server. 
> Presenting hostnames in this fully written-out format is intended to assist human users. In reality, the browser first establishes a connection to a name server and queries it for the address associated with that name.  
- [x] Enter a "real" address in the browser's address bar: 141.28.2.12

### Part 2
The second part of the URL is then interpreted arbitrarily by the server that receives the request. A pure file server attempts to identify a folder structure within the path name in order to locate a file in its storage system and return its contents as the response. github.io is one such pure file server; in the example above, it simply returned the contents of the file Test.txt.

#### Query-String
The query string now represents the actual payload data that was automatically formatted and transmitted during synchronous transfer using the form's GET method. An application server—offering functionality that extends beyond the capabilities of a simple file server—can interpret this string and use the extracted data to perform calculations or control further processes.

## Send data asynchronously
To send data via a GET request using fetch, you simply need to append a query string to the URL. How that string is constructed is irrelevant; you can be as creative as you like. However, if the data is already available from a form, it is easy to generate the string from a FormData object. Another standard JavaScript object helps with this: URLSearchParams.
```typescript
let url: string = "https://whatever.server/path/file";
let query: URLSearchParams = new URLSearchParams(<any>formData);
url += url + "?" + query.toString();
await fetch(url);
```
> At present (2019), a small hack is unfortunately still required, as TypeScript does not yet accept an object of type `FormData` as a parameter and therefore has to be tricked using `<any>`.

### Custom Data
A `URLSearchParams` object can be used in much the same way as a `FormData` object. In particular, additional key-value pairs can be added to both objects using `append`. This makes it very easy to include information that does not originate from a form.

> **Additional information**: In addition to the URL, `fetch` accepts a second parameter: a `RequestInit` object. This allows you to specify a wide range of additional control information for the communication and easily use the POST method for data transmission. You can also use the `FormData` object directly.

# Cocktail bar: Send order

<div align="center">
<a href="http://hdl.handle.net/10900.3/OER_DLGSHALR"><img src="../X01_Appendix/Img/L05_V1_Bild.JPG" width="25%"/></a>
<a href="Material/CocktailBar_UI-Scribble.jpg"><img src="Material/CocktailBar_UI-Scribble.jpg" width="25%"/></a>
  <br/>
<a href="http://hdl.handle.net/10900.3/OER_DLGSHALR"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L05 Client: Activity Diagram</a>
</div>


## Implementation I
<div align="center">
<a href="http://hdl.handle.net/10900.3/OER_ZMJSQDTS"><img src="../X01_Appendix/Img/L05_V2_Bild.JPG" width="25%"/></a>
  <br/>
<a href="http://hdl.handle.net/10900.3/OER_ZMJSQDTS"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L05 Client: Implementation I</a>
</div>

- [x] Update the implementation so that the order overview is cleared upon reset!

## Further information

<details>
<summary>Local file server</summary>
You are likely already familiar with and using the VSCode plugin LiveServer, which effectively bypasses the CORS issue. However, there are other ways to run a local server.
An error now occurs during testing on the local machine. The URL is flagged as invalid because the HyperText Transfer Protocol is not being used. Browser security mechanisms come into play, preventing scripts on websites from accessing the client computer's local storage. These mechanisms are part of the Cross-Origin Resource Sharing (CORS) policy, which governs which resources from a given source are permitted to access resources from another source.
The problem does not occur when the cocktail bar is accessed via GitHub Pages. All resources are located on the same domain—using the HTTP protocol—and access via `fetch` is permitted. Of course, it would be very tedious to have to copy the current project to a remote file server for every test during development. Instead, it is easy to turn the local development machine into a file server and then test the project under "Internet conditions." The following example uses a lightweight file server for this purpose, available at <https://www.npmjs.com/package/serve>.
<div align="center">
<a href="http://hdl.handle.net/10900.3/OER_ECPNTLWE"><img src="../X01_Appendix/Img/L05_V3_Bild.JPG" width="25%"/></a>
  <br/>
<a href="http://hdl.handle.net/10900.3/OER_ECPNTLWE"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L05 Client: Local file server</a>
</div>
</details>

<details>
<summary>Submit-Button</summary>  

There are various ways to solve the submit button problem.
<ul>
  <li>prevent the default behavior in the script using `_event.preventDefault()`</li>
  <li>define the button type as "button" in the HTML file</li>
  <li>place the button outside the form</li>  
</ul>
</details>

## Load additional data
`fetch` and the insights gained in this chapter can also be used to solve the problem left over from the previous chapter: completely separating the bartender's menu data from the program code. Just as was done earlier with the file Test.txt, a file server can provide the contents of a file as a string, and the client can then process that response. In the last chapter, the `data` variable was defined directly within the code—meaning the data existed as a literal within the source code itself. This made it easy to access the data using dot or bracket notation and to leverage its hierarchical structure. However, when the information is received merely as a string within the response, it is desirable to convert it into an associative array as well.

### JSON-Object
For this purpose, JavaScript provides the JSON object, which makes it easy to create an associative array based on information contained in a string. Conversely, it can also be used to generate a string representation of such an array. The JSON object offers two methods for this: `parse` and `stringify`.

<div align="center">
<a href="http://hdl.handle.net/10900.3/OER_CRBMCJPT"><img src="../X01_Appendix/Img/L05_V4_Bild.JPG" width="25%"/></a>
<a href="Material/CocktailBar_ActivityDiagram.jpg"><img src="Material/CocktailBar_ActivityDiagram.jpg" width="25%"/></a>
<a href="Material/CocktailBar_ActivityDiagram_Client.jpg"><img src="Material/CocktailBar_ActivityDiagram_Client.jpg" width="25%"/></a>
  <br/>
<a href="http://hdl.handle.net/10900.3/OER_CRBMCJPT"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L05 Client: Diagrams</a>
</div>

> **Notice:** The video uses the term "marker," but the correct term according to the UML standard is "connector."

## Implementation II

<div align="center">
<a href="http://hdl.handle.net/10900.3/OER_IBOVUHND"><img src="../X01_Appendix/Img/L05_V5_Bild.JPG" width="25%"/></a>
<br/>
<a href="http://hdl.handle.net/10900.3/OER_IBOVUHND"><img src="../X01_Appendix/Img/video.jpg" width="3%"/> L05 Client: Implementation II</a>
</div>

> **Notice:** There is already a wealth of free JSON editors that could be provided to the bartender to edit the menu with minimal errors.

## L08 Canvas
# L08.1 Grundlagen
<img src="Material/bobross.jpg"/>
<small>Source: <a href="http://pix4.qmde.de/www.quotenmeter.de/pics/sonstiges/2019/medien/bobross_ox__W1000xh0.jpg">http://pix4.qmde.de/www.quotenmeter.de/pics/sonstiges/2019/medien/bobross_ox__W1000xh0.jpg</a></small>  

Standardized HTML elements greatly simplify the design of web applications, and form elements even support complex interactions in a format already familiar to most users. However, this imposes significant limitations on design possibilities. Furthermore, common methods of presenting information—such as charts—cannot be generated at runtime using these elements. That is where the `HTMLCanvasElement` comes in!
> **FunFact:** "Canvas" can mean a tarpaulin, a sail, or an awning... but also an artist's canvas.

The `HTMLCanvasElement` provides a rectangular area on a browser page where individual pixels can be colored as desired using drawing commands. Within this area, shapes and colors can be dynamically generated.
> **FunFact:** "Pixel" is a portmanteau derived from **pic**ture and **el**ement (or **cell**). Raster graphics—such as those displayed on modern monitors, captured by cameras, or used to build web pages—are composed of a multitude of these colored pixels.

## CanvasRenderingContext
However, the `HTMLCanvasElement` does not provide the commands for this image manipulation directly; instead, it offers specialized programming interfaces (APIs)—known in this context as the `CanvasRenderingContext`—for this purpose. These interfaces can be quite extensive and are available for both 2D and 3D graphics rendering (e.g., using WebGL). The following code requests such a `CanvasRenderingContext`—designed for image manipulation via two-dimensional drawing commands—from a canvas element present in the DOM and stores a reference to it.
```typescript
let canvas: HTMLCanvasElement = document.querySelector("canvas");
let crc2: CanvasRenderingContext2D = canvas.getContext("2d");
```
- [x] Create an HTML file and set up a canvas element using `<canvas>`.
- [x] Create a TypeScript program that requests the rendering context of the canvas element and stores a reference in a variable named `crc2`, as specified above.
- [x] Type `crc2.` on another line and get an idea of ​​the range of drawing commands now available to you.  

When the DOM is constructed, the pixels of the canvas element are initially set to the color `#FFFFFF`—that is, white. Consequently, the canvas does not stand out against a white background. The simplest command you can use to change this is `fillRect()`, though you must first define the fill color using the `fillStyle` property of the rendering context.
```typescript
crc2.fillStyle = "#FF0000";
crc2.fillRect(0, 0, crc2.canvas.width, crc2.canvas.height);
```
- [x] Find out what the parameters of the `fillRect` method mean. Experiment with them.

## Paths
`fillRect(...)`, `clearRect(...)` and `strokeRect(...)` are the only drawing commands that immediately produce a visible result. You define more complex shapes first using a path object.
### Vector graphic
Even though the resulting image is ultimately a raster graphic, the paths are defined according to vector graphics principles. This means you do not manipulate individual pixels directly; instead, you formulate mathematical descriptions of the shapes to be drawn and their coordinates. The rendering context handles the rasterization when the `stroke()` and `fill()` drawing methods are called. Only then are the pixels located inside or along the edge of the path colored according to the previously defined fill or stroke specifications. 
However, there are also methods for direct pixel manipulation, though these will not be covered in this lesson.

### Arc  

```typescript
crc2.beginPath();
crc2.arc(100, 100, 20, 0, 1.5 * Math.PI);
crc2.closePath();
crc2.stroke();
```  

- [x] Explore what `arc` does and what the parameters mean.
- [x] Find out how to change the line color.
- [x] What happens if the `closePath()` instruction is not executed? Why?


### Ellipse
For a long time, it was necessary to represent ellipses as distorted circles or to approximate them using curves. However, there is now an `ellipse` command as well.

- [x] Find out how the ellipse works...

### Line segments
You can extend a path with line segments using the `moveTo(...)` and `lineTo(...)` commands. A line is defined solely by its end position, while the start position is the end position of the preceding command.

- [x] Have a triangle drawn.

### Curves
You can create any kind of angular shape using `moveTo(...)` and `lineTo(...)`. If you want smoother lines, use quadratic or Bézier curves.

- [x] Experiment with [this application](https://jirkadelloro.github.io/Code-Inverted/X01_Appendix/Canvas/Curves/Curves/start.html). Note that for quadratic curves, in addition to the previous end point of the current path, two further points must be specified; for Bézier curves, three further points are required. 

### Text
As an alternative to overlaying text using an additional HTML element, text can also be rendered onto the canvas via the rendering context. The methods `fillText(...)` and `strokeText(...)`, along with other methods for controlling text output, are used for this purpose.

### Further information
The CanvasRenderingContext offers several additional drawing and styling features; consult this [cheat sheet](../X01_Appendix/Canvas/HTML5_Canvas_Cheat_Sheet.pdf) for details. This gives you a quick overview and allows you to look up the syntax. However, even this is not exhaustive; you can find numerous other sources online if needed.

### Path objects
When using path methods directly on the `RenderingContext`, a global path object is manipulated. Calling `beginPath()` clears the existing path and creates a new one.
However, it is also possible to create individual path objects using `new Path2D()` and execute path commands on them. This allows the path to be stored and reused later in the program without having to re-run the path-creation algorithm. Finally, to draw such a path object, it is simply passed as a parameter to the drawing method. 

```typescript
let path: Path2D = new Path2D();
path.arc(60, 60, 50, 0, 2 * Math.PI);
crc2.stroke(path);
```

## Rendering
"Render" is a relatively non-specific term that simply means "to execute," "to make," or "to do." In the context of digital media, it usually refers to the generation of an image based on given data. There are various approaches to this.

> **FunFact:** As a noun, "Render" also means "plaster." Consequently, one could mistakenly translate the "render pipe"—frequently mentioned in 3D graphics—as a "plaster pipe."

### Retained Mode
First, a model of the entire scene to be displayed is created, which can be continuously manipulated. The rendering API then handles rendering the scene as a whole. A familiar example of such a model is the DOM. It is rendered automatically, thereby generating the visual representation that the user sees.

### Immediate Mode
However, CanvasRenderingContext operates in immediate mode. There is no management of the image elements to be displayed. When, where, and how each path is rendered is entirely up to the program using the context. As soon as a drawing command is executed, the affected pixels in the target image change color.

> **Attention:** Since the canvas is itself a DOM element, the browser must be given the opportunity to render the finished image on the page. An infinite loop, for example, would prevent it from doing so.

### Painter's algorithm
This means that subsequent drawing commands can overwrite the effects of preceding ones. Consequently, the image must be constructed "from back to front": elements intended for the background are drawn first, followed by those in the foreground. This is known as the "painter's algorithm," as this is also the most efficient way for a painter to work.
- [x] Take a look at some videos online—perhaps featuring Bob Ross. Pay attention to the order in which he paints the various elements of the picture.

## Coordinate system
The standard coordinate system has its origin in the top-left corner of the canvas; positive coordinate values ​​increase horizontally to the right and vertically downwards. Values ​​are specified directly in pixels, with the coordinate (0.5, 0.5) referencing the first visible pixel at the top-left, and the coordinate (canvas.width - 0.5, canvas.height - 0.5) referencing the last visible pixel at the bottom-right.

> **Attention:** As is always the case with digital media, we start counting at 0; therefore, on a canvas measuring 300x200, the pixel at coordinate (300, 200) lies outside the visible area.  

Coordinate values ​​can be either integers or floating-point numbers. Depending on the canvas settings, rendering will either fill the nearest pixel or distribute the color—with partial transparency—across the surrounding pixels. To target the exact center of a pixel, it may be necessary to use decimal values. The code below generates the image shown alongside (greatly enlarged).  

<img src="Material/Aliasing.png" style="float: right" >  

```typescript
crc2.beginPath();
crc2.moveTo(2.1, 1);
crc2.lineTo(2.1, 10);
crc2.moveTo(4.5, 1);
crc2.lineTo(4.5, 10);
crc2.moveTo(7.5, 1);
crc2.lineTo(10.5, 10);
crc2.stroke();
```

A diagonal line always passes through numerous intermediate positions. If only the pixels most heavily intersected by the line were drawn, a "staircase" effect (aliasing) would result. Color blending (anti-aliasing) masks this, but the use of transparency creates new colors in the image and causes the line to appear blurred.

## Transformation
A problem arises when similar image elements—such as trees, houses, snowflakes, etc.—are to appear multiple times within an image. Since the paths used to draw these elements are constructed using absolute coordinates relative to the standard coordinate system, this information is hard-coded into the path definition. Even if variables are used instead of literal values ​​as parameters—thereby allowing the construction code to be reused—the entire path must be reconstructed if the same image element is to appear at a different location. 
Transformations offer a solution here. Instead of respecifying coordinates and dimensions, transformations allow the coordinate system to which the values ​​refer to be changed.

<!--
- [x] Download the standalone [Adobe Flash Player Projector](https://www.adobe.com/support/flashplayer/debug_downloads.html). No installation is required.
- [x] Use this to open the following URL: <https://webuser.hs-furtwangen.de/~del/Apps/Transformation/Transformation.swf>
- [x] Experiment with the Transformer app. Create three different, active transformations and observe the effects.
- [x] Swap them by clicking the double-arrow icon. Explain why the "House of Santa Claus" changes its position or appearance in the process.
-->

The transformations are performed sequentially, and each subsequent transformation modifies the result of the preceding one. In fact, there is only a single transformation matrix representing the entire chain of transformations. This matrix is ​​modified each time `translate(...)`, `rotate()`, or `scale()` is called. When a path is created or drawn, the composite matrix is ​​applied to the coordinates using simple matrix multiplication.
This means that repeated transform calls accumulate in the matrix, which is generally undesirable. There are several approaches to solving this problem:
- Restore the overall matrix to its original state using `resetTransform()`
- Save the current state of the overall matrix using `save()`, and restore this state later using `restore()`. `save()` can be called multiple times; the saved states are restored in reverse order using `restore()`.
- Save the current state of the overall matrix using `getTransform()` and assign it to a variable of type `DOMMatrix`; this state is then restored using `setTransform(...)` with that variable as the parameter.

## Style
### Color
The CanvasRenderingContext can process color specifications as strings, just like those used in CSS3. This gives you a choice of color encodings, allowing you to select the one best suited to the task at hand. For a series of image elements that need to be rendered in the same color but with varying brightness, specifying the color in HSL format is significantly easier than using other formats. The following strings all produce the same <span style="background-color: salmon; width:130px; display:inline-block; text-align:center">salmon red</span>

| Format       | string                                   |
|--------------|------------------------------------------------|
| Hexadecimal  | #FA8072                                        |
| RGB          | RGB( 250, 128, 114 ) oder RGB( 98%, 50%, 45% ) |
| HSL          | HSL( 6, 93%, 71% )                             |
| Predefined | salmon                                         |

RGB and HSL also allow (when referred to as RGBA and HSLA) a fourth parameter opacity (alpha)—specified as a floating point number between 0 and 1. Additionally, the less precise notation using only three hexadecimal digits is also possible.

### Color gradient
Two types of gradients are possible: linear and radial. For a linear gradient, two points are specified on the canvas, and the color transitions from the first point to the second. A radial gradient is defined using two points as well as two radii, resulting in two circles between whose perimeters the color transitions. The specific color values ​​at various points along the gradient path are determined using stops. The position of each stop is defined by a single value relative to the gradient's total length or area, where a value of 0 corresponds to the start and 1 to the end of the gradient. The stop with the lowest value determines the color at the start and beyond, while the one with the highest value determines the color at the end and beyond; between these points, the colors blend into one another.

```typescript       
let gradient: CanvasGradient = crc2.createLinearGradient(0, 0, 0, 100);

gradient.addColorStop(0, "black");
gradient.addColorStop(.5, "red");
gradient.addColorStop(1, "gold");

crc2.fillStyle = gradient;
crc2.fillRect(0, 0, 200, 100);
```
- [x] What image does the code provided above produce?
- [x] Add more stop points to achieve sharper edges between the colors.

### Pattern
It is also possible to use fill patterns. Various sources for the pattern are supported, such as bitmaps, SVG images, or graphics generated on a canvas. The following code produces the image shown alongside.

<img src="Material/Pattern.png" style="float: right" >  

```typescript
let pattern: CanvasRenderingContext = document.createElement('canvas').getContext('2d');
pattern.canvas.width = 40;
pattern.canvas.height = 20;

pattern.fillStyle = '#fec';
pattern.fillRect(0, 0, pattern.canvas.width, pattern.canvas.height);
pattern.moveTo(0, 10);
pattern.lineTo(10, 10);
pattern.lineTo(20, 0);
pattern.lineTo(30, 0);
pattern.lineTo(40, 10);
pattern.lineTo(30, 20);
pattern.lineTo(20, 20);
pattern.lineTo(10, 10);
pattern.stroke();

crc2.fillStyle = crc2.createPattern(pattern.canvas, 'repeat');
crc2.fillRect(0, 0, canvas.width, canvas.height);
```

## Lines
Naturally, the appearance of lines drawn using the `stroke()` command can also be controlled in detail. Lines can be solid or have various dashed patterns, be assigned colors, gradients, and even patterns, and feature specific styling for line endings and the handling of corners.

- [x] Research options for line styling on the Internet.

## Save/Restore
The `save()` and `restore()` methods save not only the current transformation matrix but also style settings for fills and lines, along with other information that collectively defines the state of the rendering context. This makes it possible, for instance, to revert to a previous style after a different one has been used in the interim. `save()` stores this information on a stack; subsequent calls to `save()` push additional sets of information onto the top. `restore()` empties the stack from top to bottom, one item at a time, restoring the `CanvasRenderingContext` states in the reverse order of their saving.

# L08.2 Landscape

## Scribble
<div align="center">
<a href="http://hdl.handle.net/10900.3/OER_SHBUHNLQ"><img src="../X01_Appendix/Img/L08_V1_Bild.JPG" width="25%"/></a>
<a href="Material\Alley_Scribble.jpg"><img src="Material\Alley_Scribble.jpg" width="25%"/></a>
<br/>
<a href="http://hdl.handle.net/10900.3/OER_SHBUHNLQ"><img src="../X01_Appendix/Img/video.jpg" width="2%"/>L08 Canvas-Scribble</a>
</div>



## Activity diagram
<div align="center">
<a href="http://hdl.handle.net/10900.3/OER_BKILGHVL"><img src="../X01_Appendix/Img/L08_V2_Bild.JPG" width="25%"/></a>
<a href="Material\Alley_ActivityDiagram.jpg"><img src="Material\Alley_ActivityDiagram.jpg" width="25%"/></a>
<br/>
<a href="http://hdl.handle.net/10900.3/OER_BKILGHVL"><img src="../X01_Appendix/Img/video.jpg" width="2%"/>L08 Canvas: Activity diagram</a>
</div>



## Implementation of the background
<div align="center">
<a href="http://hdl.handle.net/10900.3/OER_XQZRKOBR"><img src="../X01_Appendix/Img/L08_V3_Bild.JPG" width="25%"/></a>
<br/>
<a href="http://hdl.handle.net/10900.3/OER_XQZRKOBR"><img src="../X01_Appendix/Img/video.jpg" width="2%"/>L08 Canvas: Implementation I</a>
</div>


## Implementation of the cloud
<div align="center">
<a href="http://hdl.handle.net/10900.3/OER_YFYTDWCV"><img src="../X01_Appendix/Img/L08_V4_Bild.JPG" width="25%"/></a>
<br/>
<a href="http://hdl.handle.net/10900.3/OER_YFYTDWCV"><img src="../X01_Appendix/Img/video.jpg" width="2%"/>L08 Canvas: Implementation II</a>
</div>


## Activity diagram for the mountains
<div align="center">
<a href="http://hdl.handle.net/10900.3/OER_CMDSHKDM"><img src="../X01_Appendix/Img/L08_V5_Bild.JPG" width="25%"/></a>
<a href="Material\Alley_ActivityDiagram-Mountains.jpg"><img src="Material\Alley_ActivityDiagram-Mountains.jpg" width="25%"/></a>
<br/>
<a href="http://hdl.handle.net/10900.3/OER_CMDSHKDM"><img src="../X01_Appendix/Img/video.jpg" width="2%"/>L08 Canvas: Activity diagram mountains</a>
</div>



## Implementation of the mountains
<div align="center">
<a href="http://hdl.handle.net/10900.3/OER_BARAOYLQ"><img src="../X01_Appendix/Img/L08_V6_Bild.JPG" width="25%"/></a>
<br/>
<a href="http://hdl.handle.net/10900.3/OER_BARAOYLQ"><img src="../X01_Appendix/Img/video.jpg" width="2%"/>L08 Canvas: Implementation Mountains</a>
</div>


## Exercises
- [x] Isolate the functionality for generating a random number between a minimum and a maximum value. Design it as a distinct operation and implement it as a function that accepts the two limit values ​​and returns a corresponding random number. Replace the points in the program where this function can and should be used with appropriate calls to it.
- [x] Study the [Scribble](Material/draw.io/Alley_Scribble.svg) and the [activity diagram](Material/draw.io/Trees_ActivityDiagram.svg) regarding the layout of the row of trees, as well as the implementation. You might find some valuable pointers there for your further work.
- [x] Study the code for the avenue and figure out why additional variables were introduced before the calls to the draw functions.
- [x] Experiment with the parameters of the various functions, as well as the hard-coded values ​​within them. Modify the image according to your aesthetic sensibilities.
- [x] Shift the horizontal position of the road on the horizon by a random amount between -200 and 200 pixels so that the vanishing point differs each time the program is run. Why do the rows of trees move along with it?

- [x] The CanvasRenderingContext offers additional features, such as shadows. Get an overview of them.

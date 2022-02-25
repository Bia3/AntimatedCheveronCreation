# Animated Chevron Creation
## Drawing the Chevrons
___
Inorder to create the chevrons quickly I used Inkscape though you could use 
Illustrator or a vector graphics app of your choice. Just make sure whichever app
you choose it is able to export a plain SVG.

First create a blank document and added in some guides along the x-axis at 70 mm, 75 mm,
100 mm, 120 mm and 125 mm. I then added guides along the y-axis at 100 mm, 110 mm, 115 mm,
120 mm, 130 mm, 140 mm, 142 mm, and 145 mm.

![Empty document with aforementioned guides](imgs/guides.png)

Use the Guideline dialog to make this easier to place the guides on exact points.\
![The Guideline dialog box](imgs/GuidesSetting.png)
If you are using Inkscape here is some documentation on guidelines
[Working with Guides in Inkscape](https://inkscapetutorials.wordpress.com/2014/04/25/working-with-guides-in-inkscape/). 

The center box created by the guides is where we will be drawing our chevrons.
Using the free hand tool draw lines that connect (70, 100) to (75, 100) and (75, 100)
to (100, 115). From (100, 115) to (120, 100) and (120, 100) to (125, 100). Then 
from (125, 100) to (100, 120) and (100, 120) finally back to (70, 100). 

You should end up with something like this.\
![Empty outline of the first chevron](imgs/Draw1.png)

Next we want it to be filled in with no outline. We set the stoke paint to none and 
set fill to black(#000000ff).\
![The first chevron now filled in black with no outline](imgs/Draw2.png)

We then want to copy the top chevron twice to get the other two shifting them down
by 10 mm. Draw a rectangle to fill in the last empty box left in the guides using 
the outer x-axis guides. Then just like with chevron make sure stroke paint is 
set to none and fill is set to black.

We should end up with this.
![The complete drawing with three chevrons over a solid box](imgs/DrawComplete.png)

Next we want to clean up the XML of this doc by a bit. First ope up the XML editor.\
![The XML Editor](imgs/XMLEdit1.png)\
Notice that there is a group for layer 1. We want to remove that. So lets drag the 
three paths as well as the rectangle up and out of that group.\
![XML Editor with the paths and rectangle moved out of the layer1 group](imgs/XMLEdit2.png)\
Then delete the layer1 group.\
![XML Editor with the paths and rectangle and the layer1 group is gone](imgs/XMLEditComplete.png)\
After that delete all the guides from the edit menu. Select the entire drawing and
again under edit resize the document to the selection. This will help with sizing 
the animation in HTML.

Save the file out as a Plain SVG file.\
![The save dialog box with Plain SVG selected in the file type drop down](imgs/Save.png)
## Cleaning up the SVG file
Open the saved doc in the editor of your choice I will be using IntelliJ though
you could do this in Atom or VSCode.

If we take a look at what the vector graphics app created for us we will notice a 
bunch of points have decimals in them. This paths are long and hard to use for animation.
We will want to simplify these. 
```svg
<path
    id="path1017"
    style="fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583px;stroke-linecap:butt;stroke-linejoin:miter;stroke-opacity:1"
    d="M 9.7305117e-5,9.5249999e-5 25.000121,19.999909 50.000147,9.5249999e-5 45.000025,0 25.000121,15.076178 4.9997936,9.5249999e-5 Z" />
<path
    id="path1017-1"
    style="fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583px;stroke-linecap:butt;stroke-linejoin:miter;stroke-opacity:1"
    d="M 0,10.00018 25.000026,30 l 25.00002,-19.99982 -5.00012,-9e-5 -19.9999,15.07617 -20.00033,-15.07608 z" />
<path
    id="path1017-1-1"
    style="fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583px;stroke-linecap:butt;stroke-linejoin:miter;stroke-opacity:1"
    d="M 6e-6,20.00018 25.000026,40 l 25.00002,-19.99982 -5.00011,-9e-5 -19.99991,15.07617 -20.00032,-15.07608 z" />
<rect
    style="fill:#000000;fill-opacity:1;stroke:none;stroke-width:1.4"
    id="rect2202"
    width="50"
    height="3"
    x="2.6e-05"
    y="42" />
```
Round all the points to the nearest 5 mm.
```svg
<path
    id="path1017"
    style="fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583px;stroke-linecap:butt;stroke-linejoin:miter;stroke-opacity:1"
    d="M 0,0 25,20 50,0 45,0 25,15 5,0 Z" />
<path
    id="path1017-1"
    style="fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583px;stroke-linecap:butt;stroke-linejoin:miter;stroke-opacity:1"
    d="M 0,10 25,30 l 25,-20 -5,0 -20,15 -20,-15 z" />
<path
    id="path1017-1-1"
    style="fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.264583px;stroke-linecap:butt;stroke-linejoin:miter;stroke-opacity:1"
    d="M 0,20 25,40 l 25,-20 -5,0 -20,15 -20,-15 z" />
<rect
    style="fill:#000000;fill-opacity:1;stroke:none;stroke-width:1.4"
    id="rect2202"
    width="50"
    height="3"
    x="0"
    y="42" />
```
Do the same for the viewBox and with/height of your SVG.
```svg
<svg
   width="50mm"
   height="50mm"
   viewBox="0 0 50 50"
```
Change the ids to something easy to remember I will be using TopChevron,
MiddleChevron, BottomChevron, and UnderLine. Change the root SVG id to 
something simpler as well. Create some classes for styling the objects. 
```svg
<svg
        id="Chevrons" >
        <defs>...</defs>
        <path
          class="chevron"
          id="TopChevron" />
        <path
          class="chevron"
          id="MiddleChevron" />
        <path
          class="chevron"
          id="BottomChevron" />
        <rect
          class="line"
          id="UnderLine" />
</svg>
```

Now that all the paths are easier to edit and modify we want some of our styling moved
out of the objects. Remove the defs tag create an empty style tag.
```svg
<svg
        width="50mm"
        height="50mm"
        viewBox="0 0 50 50"
        version="1.1"
        id="Chevrons"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:svg="http://www.w3.org/2000/svg">
        <style></style>
        ...
</svg>
```
Now we want to move out the fill styles and opacity styles and remove anything we don't need.
```svg
<svg
        width="50mm"
        height="50mm"
        viewBox="0 0 50 50"
        version="1.1"
        id="Chevrons"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:svg="http://www.w3.org/2000/svg">
        <style>
            .cheveron {
              fill: #000;
              fill-opacity: 1; 
            }
            
            .line {
              fill: #000;
              fill-opacity: 1;           
            }
        </style>
        <path
            class="chevron"
            id="TopChevron"
            style="stroke:none;stroke-width:0.264583px;stroke-linecap:butt;stroke-linejoin:miter"
            d="M 0,5 25,25 50,5 45,5 25,20 5,5 Z" />
        <path
            class="chevron"
            id="MiddleChevron"
            style="stroke:none;stroke-width:0.264583px;stroke-linecap:butt;stroke-linejoin:miter"
            d="M 0,15 25,35 l 25,-20 -5,0 -20,15 -20,-15 z" />
        <path
            class="chevron"
            id="BottomChevron"
            style="stroke:none;stroke-width:0.264583px;stroke-linecap:butt;stroke-linejoin:miter"
            d="M 0,25 25,45 l 25,-20 -5,0 -20,15 -20,-15 z" />
        <rect
            class="line"
            id="UnderLine"
            width="50"
            height="3"
            x="0"
            y="47" />
</svg>
```
## Animating the SVG
Keeping in our editor of choice
I want an animation to three seconds and loop it infinitely. It is a good Idea
to open the SVG in a browser and refresh after each change to test your animation.\
![The Completed SVG](chervrons.svg)

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="bGYjzEr" data-user="sean_r" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/sean_r/pen/bGYjzEr">
  Animated Chevrons</a> by Sean (<a href="https://codepen.io/sean_r">@sean_r</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

D3 Concepts
   1. Working with SVGs Concepts

   2. Data binding

        - [A beginner’s guide to data binding — SitePoint](https://www.sitepoint.com/a-beginners-guide-to-data-binding-in-d3-js/)
        - [Thinking with joins — Mike Bostock](https://bost.ocks.org/mike/join/)
        - [Let’s make a grid with D3.js — Chuck Grimmett](http://www.cagrimmett.com/til/2016/08/17/d3-lets-make-a-grid.html)

    3. Scale

            1. They need to be set with a **domain** and a **range**, which can be pretty easy to confuse. The **domain** represents the interval that our **input values** will run between, and the **range** represents the interval that our **output values** will run between.
            2. If you’re working with data that increases exponentially over time, then you might want to use a **logarithmic scale**.
            3. If you’re working with date values, then you’ll use a **time scale**.
            4. If you want to assign colors between different categories, you can use an **ordinal scale**.
            5. If you’re spacing out rectangles in a bar chart, then you’ll use a **band scale**.

        - [An introduction to linear scales in D3 — Ben Clikinbeard](https://benclinkinbeard.com/posts/introduction-to-d3-scales/)
        - [A walkthrough of the different types of scales — D3 in depth](http://d3indepth.com/scales/)
        - [The entry for scales in the D3 API reference](https://github.com/d3/d3-scale)

    4. Margins and Axes

            1. D3’s axis generators work by attaching an axis onto whichever element they’re called on. 

            2. The problem is that, if we try attaching axes straight onto our SVG canvas, then we’ll end up with something like this:

                ![img](https://cdn-images-1.medium.com/max/2486/1*Dij7WdNq0jjNXbooC-8o6Q.png)

        In D3, we need to get used to the “margin convention” that all of our projects should follow:

        ![img](https://cdn-images-1.medium.com/max/2000/1*1RmRQh986P-TlvLhnTd4NQ.png)

        - [An walkthrough of our margin convention code — Mike Bostock](https://bl.ocks.org/mbostock/3019563)
        - [A guide to axis generators in D3 — TutorialsTeacher](http://www.tutorialsteacher.com/d3js/axes-in-d3)
        - [The D3 API reference entry on axes](https://github.com/d3/d3-axis)

    5. The General Update Pattern

            1. The tricky thing about understanding the general update pattern is figuring out exactly what selectAll(), enter(), and exit() are doing. D3 works by using a set of “virtual selectors”, which we can use to keep track of which elements need to be updated.
                - [A walkthrough of the general update pattern — Quinton Louis Aiken](http://quintonlouisaiken.com/d3-general-update-pattern/)
                - [An interactive exploration of the general update pattern — Chris Given](https://bl.ocks.org/cmgiven/32d4c53f19aea6e528faf10bfe4f3da9)



Element Selector

```javascript
d3.select("#canvas")
	.append("SVGShape")		// Adding svgshape to svgconatiner
	.attr("cx",10)		// Adding property to svg shape element
```

Data binding 

```javascript
var circles = d3.select("#canvas").selectAll("circle")
    .data(data0);		// Adding data
    
    circles.enter().append("circle")
    .attr("cx", /* The first argument to these functions gives us access to the item in our data that we’re looking at, and the second argument gives us the item’s index in our array.*/function(d, i){ return 25 + (50 * i); })
    .attr("cy", function(d, i){ return 25 + (50 * i); })
    .attr("r", 5)
    .attr("fill", "grey");
```

Defining Scale

```javascript
var x = d3.scaleLinear()
    .domain([d3.min(data0, function(d){ return d.gpa; }) / 1.05, 
        d3.max(data0, function(d){ return d.gpa; }) * 1.05])
    .range([0, 800]);

var y = d3.scaleLinear()
    .domain([d3.min(data0, function(d){ return d.height; }) / 1.05,
        d3.max(data0, function(d){ return d.height; }) * 1.05])
    .range([500, 0]);
```

Setting Margin to SVG Canvas

```javascript
var svg = d3.select("#canvas");

var margin = {top: 10, right: 10, bottom: 50, left: 50};
var width = +svg.attr("width") - margin.left - margin.right;
var height = +svg.attr("height") - margin.top - margin.bottom;

var g = svg.append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");


// Adding Axis to svg
var xAxisCall = d3.axisBottom(x)
var xAxis = g.append("g")
    .attr("class", "x-axis")
    .attr("transform", "translate(" + 0 + "," + height + ")")
    .call(xAxisCall);

var yAxisCall = d3.axisLeft(y)
var yAxis = g.append("g")
    .attr("class", "y-axis")
    .call(yAxisCall)

// Labels
xAxis.append("text")
    .attr("class", "axis-title")
    .attr("transform", "translate(" + width + ", 0)")
    .attr("y", -6)
    .text("Grade Point Average")
yAxis.append("text")
    .attr("class", "axis-title")
    .attr("transform", "rotate(-90)")
    .attr("y", 16)
    .text("Height / Centimeters");

// Interval method in D3JS
var flag = true;

// Run this code every second...
d3.interval(function(){
    // Flick between our two data arrays
    data = flag ? data0 : data1;

    // Update our chart with new data
    update(data);

    // Update our flag variable
    flag = !flag;
}, 1000)
```

```javascript
// EXIT old elements not present in new data.
circles.exit().remove()
```

Transition 

```javascript
// Standard transition for our visualization
var t = d3.transition().duration(750);
```

-------------------------------------------------------------------------------

SVG Concepts

There’s a fairly unintuitive coordinate system that you’ll need to understand, since the (0, 0) point of an SVG grid is at the top-left, rather than the bottom-left.

[A guide to SVGs for absolute beginners — Rob Levi

[An SVG primer for D3 — Scott Murray](http://alignedleft.com/tutorials/d3/an-svg-primer)

SVG Shapes

 	1. Circle
 	2. Reactangle
 	3. path
 	4. 



--------------------------

# React Learning Steps

https://medium.freecodecamp.org/the-react-handbook-b71c27b0a795

**SECTION 1**: MODERN JAVASCRIPT CORE CONCEPTS YOU NEED TO KNOW TO USE REACT

- [Variables](https://medium.com/p/b71c27b0a795#1f25)
- [Arrow functions](https://medium.com/p/b71c27b0a795#625c)
- [Rest and spread](https://medium.com/p/b71c27b0a795#5d57)
- [Object and array destructuring](https://medium.com/p/b71c27b0a795#87c9)
- [Template literals](https://medium.com/p/b71c27b0a795#cf4f)
- [Classes](https://medium.com/p/b71c27b0a795#ef55)
- [Callbacks](https://medium.com/p/b71c27b0a795#6b71)
- [Promises](https://medium.com/p/b71c27b0a795#7899)
- [Async/Await](https://medium.com/p/b71c27b0a795#05d9)
- [ES Modules](https://medium.com/p/b71c27b0a795#634f)

**SECTION 2**: REACT CONCEPTS

- [Single Page Applications](https://medium.com/p/b71c27b0a795#2435)
- [Declarative](https://medium.com/p/b71c27b0a795#244f)
- [Immutability](https://medium.com/p/b71c27b0a795#c5a1)
- [Purity](https://medium.com/p/b71c27b0a795#b75c)
- [Composition](https://medium.com/p/b71c27b0a795#ab50)
- [The Virtual DOM](https://medium.com/p/b71c27b0a795#f042)
- [Unidirectional Data Flow](https://medium.com/p/b71c27b0a795#0708)

**SECTION 3**: IN-DEPTH REACT

- [JSX](https://medium.com/p/b71c27b0a795#2bb6)
- [Components](https://medium.com/p/b71c27b0a795#8606)
- [State](https://medium.com/p/b71c27b0a795#b70b)
- [Props](https://medium.com/p/b71c27b0a795#2801)
- [Presentational vs container components](https://medium.com/p/b71c27b0a795#c422)
- [State vs props](https://medium.com/p/b71c27b0a795#831f)
- [PropTypes](https://medium.com/p/b71c27b0a795#a8ba)
- [React Fragment](https://medium.com/p/b71c27b0a795#4c2f)
- [Events](https://medium.com/p/b71c27b0a795#6f82)
- [Lifecycle Events](https://medium.com/p/b71c27b0a795#e98e)
- [Forms in React](https://medium.com/p/b71c27b0a795#d8d9)
- [Reference a DOM element](https://medium.com/p/b71c27b0a795#d9b8)
- [Server side rendering](https://medium.com/p/b71c27b0a795#d28b)
- [The Context API](https://medium.com/p/b71c27b0a795#5731)
- [Higher order components](https://medium.com/p/b71c27b0a795#9008)
- [Render Props](https://medium.com/p/b71c27b0a795#5b9b)
- [Hooks](https://medium.com/p/b71c27b0a795#073c)
- [Code splitting](https://medium.com/p/b71c27b0a795#6982)

**SECTION 4**: PRACTICAL EXAMPLES

- [Build a simple counter](https://medium.com/p/b71c27b0a795#61f3)
- [Fetch and display GitHub users information via API](https://medium.com/p/b71c27b0a795#1f54)

**SECTION 5**: STYLING

- [CSS in React](https://medium.com/p/b71c27b0a795#c186)
- [SASS in React](https://medium.com/p/b71c27b0a795#a030)
- [Styled Components](https://medium.com/p/b71c27b0a795#9e55)

**SECTION 6**: TOOLING

- [Babel](https://medium.com/p/b71c27b0a795#eb1b)
- [Webpack](https://medium.com/p/b71c27b0a795#f0be)

**SECTION 7**: TESTING

- [Jest](https://medium.com/p/b71c27b0a795#d8af)
- [Testing React components](https://medium.com/p/b71c27b0a795#54c3)

**SECTION 8**: THE REACT ECOSYSTEM

- [React Router](https://medium.com/p/b71c27b0a795#37c5)
- [Redux](https://medium.com/p/b71c27b0a795#8e78)
- [Next.js](https://medium.com/p/b71c27b0a795#839c)
- [Gatsby](https://medium.com/p/b71c27b0a795#555e)

[Wrapping up](https://medium.com/p/b71c27b0a795#aff3)
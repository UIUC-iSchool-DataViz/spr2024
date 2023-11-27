---
title: Changing Data in Vega-Lite and d3
layout: lecture
include_vega: true
setup_script: setup_script.js
tags: 
  - vega-lite
  - starboard
  - d3
description: >-
  We will learn about Starboard, then update data in vega-lite and possibly learn a little d3.
---

# Changing Data in Vega-Lite and D3

Today we'll work on updating data in vega-lite, and maybe introduce ourselves to d3, but first we'll learn about [Starboard](https://starboard.gg).

---

## Document Object Model

The Document Object Model (DOM) is how we interact with the collection of HTML
objects in our document.

For instance, a page can be composed of `<div>` objects, `<p>` objects, etc,
and we can construct and interact with these.  This includes things like
modifying style sheets.

![](https://www.w3schools.com/js/pic_htmltree.gif)

One alternative is to have rendering tied to data and data
values, and to have those automatically update as needed -- `vega-lite`.


---

## Other Platforms: A Digression

In the past, I've taught this course and used several different platforms for interactive web-based visualization:

- Iodide (and pyodide)
- ObservableHQ
- Idyll

Each one has *slightly* different approaches to development, but they all share
the same fundamental usage patterns and development patterns.

Today we will be using Starboard, which is open source, can be self-hosted,
fully-private and supports a variety of languages and systems.


---

## Starboard

[Iodide](https://iodide.io) was a project originally from Mozilla for generating literate combinations of Javascript, Python (via [Pyodide](https://hacks.mozilla.org/2019/04/pyodide-bringing-the-scientific-python-stack-to-the-browser/)) and Markdown to a single document-based interface.

The documents you created appear to be similar to Notebooks, and they shared some of the same attributes and behaviors, but they will be different in how you create new cells, reorder cells, and version them.

Much of what *was* Iodide is now available in [starboard.gg](https://starboard.gg/).  You can utilize "Offline" notebooks to have them stored *only* in your browser, which is a notable improvement from the original Iodide.

---

## Starboard Cell Types

Starboard has a couple different primary cell-types: javascript, python, markdown and HTML.  There are others, but those are the four we will be working with.

To get started, create a new notebook -- you can do this in the "offline" format if you like! -- at [starboard.gg](https://starboard.gg/) and let's get started exploring what we can do.

---

## Importing Javascript Libraries

We can import Javascript libraries we want to use using the `import` command.  Let's start by importing `vega-lite`.

```javascript
await import('https://cdn.jsdelivr.net/npm/vega@latest');
await import('https://cdn.jsdelivr.net/npm/vega-lite@latest');
await import('https://cdn.jsdelivr.net/npm/vega-embed@latest');
```

In production, you'll want to avoid using `@latest` and instead pin to a specific version.

---

## Fetching Data with Starboard

To fetch data with Starboard, you can use the `fetch` command in a javascript cell:

```javascript
const data = await fetch("https://yoururl.com/data.csv");
```

You can also access it in Python with:

```python
pyodide.http.open_url("https://yoururl.com/data.csv")
```

This second command will return a `StringIO` object which you can read from.

---

## Pyodide API

Recall that we can run Python code in Python cell type.  In Python, we can obtain access to our Javascript objects by `import`ing them from `js`:

```
%% js
var a = {1: 2, 3: 4};
%% py
from js import a
a
```

This will create the `a` object in Javascript, and make it accessible in Python by importing it.  Note that we're not `print()`-ing `a`, but instead allowing it to be displayed as the final statement in the cell.


---


## Pyodide API

We can also dynamically access properties in the Javascript from Pyodide using the `js` module.  For instance, we can examine properties of our `document`:

```python
import js
js.document.title
```

---

## Passing Data Back

This ability to pass data also allows us to do things in pandas and immediately have them visible in Javascript.  For instance, we can do something like this:

```javascript
var my_schema = {'data': {'values': []}};
```

Then:

```python
from js import my_schema
my_schema['data']['values'] = [
  {'a': 1, 'b': 2},
  {'a': 3, 'b': 4},
  {'a': 5, 'b': 6}
]
```

And back in Javascript:

```javascript
my_schema['data']['values']
```

and we have modified, in place, our data values.

---

## Accessing Pyodide in Starboard

We can also access Python (Pyodide) values in Javascript.  For example:

```python
import numpy as np
a = [1, 2, 3, 4]
b = np.random.random(5)
```

```javascript
var a_js = pyodide.globals.get('a');
var b_js = pyodide.globals.get('b');
```

Note that Starboard and Pyodide know how to convert between Javascript [TypedArray](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray) and NumPy [`ndarray`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.html) objects.

---

## Starboard and Objects

It's also possible -- with a number of caveats! -- to pass more complex objects back and forth, and to rely on proxy calls.

```python
class ExampleClass:
    def __init__(self, something):
        self.something = something
    def get_name(self):
        print(self.something)
        return self.something
c = ExampleClass('hi')
```
```javascript
my_obj = pyodide.globals.get('c')
my_obj.get_name();
```

---

## Why update data?

- Reaction to new, incoming data
- Modify based on some set of inputs that can't be bound to vega-lite events
- Static visualization, changing view

---

## Updating Data in vega-lite

We can change the visible data in vega-lite.  We'll do this with some simple data, and then try our hand at a simple role playing game.


---


## Embedded vega-lite config

We are principally using a vega-lite "embed" mechanism:

```javascript
var embedded = vegaEmbed('#vis', yourVlSpec);
```

We are also able to specify a configuration variable to this at the config
option.  ([Details](https://github.com/vega/vega-embed))  You may find it
useful to update the `actions` option in `opt`, which controls which items are
available in the menu:

```javascript
var embedded = vegaEmbed('#vis', yourVlSpec, {'actions': false});
```

---

## Accessing embedded vega-lite

The object returned by `vegaEmbed` is a
[`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).
This means that when you access it, it may not *yet* be available -- so instead
of actually calling on it, we supply a function to be called *at some point*
when it is ready -- when the promise has been __resolved__.  This function will
be called with that object.

```javascript
somePromiseObject.then( function(resolvedObject) {
  resolvedObject.doSomething();
});
```

(This type of syntax, for deferring actions to the future, is very common in
Javascript.)

---

## Accessing embedded vega-lite with Starboard

In some cases, you can use what's known as an `await` command to wait for the
promise to return successfully.  We can use this in starboard:

```javascript
var embedded = await vegaEmbed("#vis", yourVlSpec);
```

We will attempt to use both forms, to provide information on using vega-lite in
various different contexts.


---

## The vega-lite View API

The [`View` API](https://vega.github.io/vega/docs/api/view/#view_insert) in vega-lite (rather, vega) is how we manipulate and change data.  This can be done by constructing a `changeset`, appending operations to that changeset, and then executing that changeset on the `view` of our embedded visualization.

```javascript
var cs = vega.changeset()
          .remove( function(t) {
              return t.CIRCULATION < 10000
          });
embedded.then( function(res) {
  res.view.change('table', cs).run();
});
```

This will update the data table named `'table'` with everything "queued up" in
the `changeset` object `cs`.

---

## vega-lite: insert example

```json
  {
    "$schema": "https://vega.github.io/schema/vega-lite/v3.json",
    "description": "A scatterplot",
    "data": {"name": "table",
             "values": [ {"x": 1.0, "y": 2.0},
                         {"x": 2.0, "y": 1.0},
                         {"x": 3.0, "y": 9.0},
                         {"x": 4.0, "y": 8.0},
                         {"x": 5.0, "y": 6.0} ] },
    "mark": "point",
      "encoding": {
        "x": {"field": "x","type": "quantitative"},
        "y": {"field": "y","type": "quantitative"}
      }
  }
```

<div id="vis2"></div>


---


## vega-lite: insert example

We can add elements to our dataset with the `insert` function.  This takes an
array of data tuples, like those already included, and adds them to the data
being visualized.

```javascript
var cs = vega.changeset()
          .insert( [
            {'x': 1.0, 'y': 10.0},
            {'x': 5.0, 'y': 1.3},
            {'x': 2.1, 'y': 0.7}
          ]);
embedded3.then( function(res) {
  res.view.change('table', cs).run();
```


---


## vega-lite: insert example

We do this by affixing an event handler, in this case to a button and the event
`click`.

```javascript
document.getElementById("button3")
  .addEventListener("click", 
    function onButtonClick(event) {
  });
```

<div id="vis3"></div>
<button id="button3">Click to insert</button>

---

## vega-lite: remove

Similarly, we can *remove* data points by supplying a function that is
evaluated on each of the data tuples.

```javascript
var cs = vega.changeset()
          .insert( [
            {'x': 1.0, 'y': 10.0},
            {'x': 5.0, 'y': 1.3},
            {'x': 2.1, 'y': 0.7}
            ])
          .remove( function(t) {
            return t.x < t.y;
          });
embedded4.then( function(res) {
  res.view.change('table', cs).run();
```


---


## vega-lite: remove

<div id="vis4"></div>
<button id="button4">Click to insert and remove</button>

(Wait, what happened ...?)


---

## Example 1

We're going to set up a contrived example.  In [iodide](https://starboard.gg/) we will:

1. Create a simple HTML report
1. Create a listener button for adding a new data point
1. Create a listener button for removing a random data point


---

## Example 2 (Advanced-ish)

We're going to create a "Character Widget" to show characteristics, like in an RPG, of a set of characters.

We'll add buttons to update their hit points.


---

## A Little Bit of D3

We're going to learn a very small bit of [d3.js](https://d3js.org/).

The easiest way to utilize D3 is through [observablehq.com](https://observablehq.com):

```
d3 = require("d3@6");
```

although it is straightforward to include the necessary code in an HTML page:

<code>
&lt;script src="https://d3js.org/d3.v6.min.js"&gt;&lt;/script&gt;
</code>


---

## Time to Vote!

Do we want to learn a new platform today?

Should we dive into Observable, or should we stick with Iodide?


---

## Concepts in d3: outline

The basic concepts here we will convey, focusing on *static* visualizations:

- `.select()` and `.selectAll()`
- `.enter()`
- `.attr()` and `.style()`
- `d3.scaleLinear()`


---

## Concepts in d3: data and elements

When we manipulate items in d3, we connect the concept of a data item to an
element in a document.

Typically, this will be an element in an SVG -- for instance, a `line`,
`circle`, `rect` or `text` element.

Our typical workflow:

1. Select all items of a specific criteria
1. Bind these items to a set of data
1. Update, append or remove items based on their corresponding item.


---

## Concepts in d3: accessors and setters

We will very frequently run into the case where we call something, and supply a
*function* to it, rather than supplying a value.  For instance, *both* of these
calls to `attr` are valid in a typical d3 workflow:

```javascript
.attr("cx", 1.0)

// or

.attr("cx", d => d.x)
```

In one, we are setting the value static; in the other, we base the value on the datapoint supplied.


---

## Concepts in d3: Let's make circles

For our first exploration, let's just make some circles.  I will demonstrate this in observable, but here is the key snippet of code:

```
var dataset = [ {'x': 100, 'y': 200, 'radius': 15},
                {'x': 150, 'y': 300, 'radius': 30} ];
svg.selectAll("circle").data(dataset).enter()
   .append("circle")
   .attr("cx", d => d.x)
   .attr("cy", d => d.y)
   .attr("r", d => d.radius)
   .style("fill", "black");
```

What does this do?


---

## Concepts in d3: Objects

We will mostly use the objects

- [`rect`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/rect) -- which has `x`, `y`, `width` and `height`
- [`circle`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/circle) -- which has `cx`, `cy`, and `r`
- [`line`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/line) -- which has `x1`, `x2`, `y1` and `y2`.

Each of these objects can be controlled in style, appearance, position, etc, according to standard SVG and CSS rules.

Let's experiment!


---

## Concepts in d3: Scaling

To map from a given range to a different range in a linear fashion, we can construct a linear scale:

```javascript
var xScale = d3.scaleLinear().range([0, 100]).domain([0.0, 1.0]);
```

This is now an object we can use to map the values 0 .. 1 to 0 .. 100.  This is useful for, among other things, computing the position of a given value:

```javascript
.attr("cx", d => xScale(d.x))
```


---

## Additional Basic Topics

`d3.axisBottom(xScale)` can be used to create ticks and axes; however, it
requires manual translation using the `transform` attribute, using something
like `.attr("transform", "translate(0, 30)")`.  This then brings up our concept
of margins, width, height, and the like!  How can we manage these?

Let's try it out!


---

## Creating an SVG canvas

We can create one programmatically:

```javascript
const svg = d3.create("svg")
  .attr("id", "my_svg")
  .attr("width", 300)
  .attr("height", 300);
```

or, in Iodide, we can create one through standard HTML:

```
<svg id="my_svg">
```


---

# Selecting and binding data

```javascript

var dataset = [
  {x1: 1.0, y1: 3.0, x2: 3.5, y2: 2.0, radius: 10},
  {x1: 2.5, y1: 2.0, x2: 0.75, y2: 3.5, radius: 5},
  {x1: 3.1, y1: 0.6, x2: 4.1, y2: 3.4, radius: 25}
];

svg.selectAll("circle")
   .data(dataset)
   .enter()
   .attr("cx", d => xScale(d.x1))
   .attr("cy", d => yScale(d.y1))
   .attr("r", d => d.radius)
   .style("fill", "black");
```


---

## Scaling

```javascript
var xScale = d3.scaleLinear().range([0, 100]).domain([0.0, 1.0]);
```

We will cover other scales later today.


---

## Events

There are many events that we can "listen" for, and respond to.  For instance, the `click` event is a common event to manage.

The function receives the data (if any), the index of the node, and the nodelist.

```javascript
d3.selectAll("circle")
  .on("click", (d, i, n) => {
    d3.select(n[i]).attr("r", 100)
  });
```


---

## Transitions

We can also update items through transitions, which can accept both a `delay` and a `duration`, both specified in milliseconds.

```javascript
d3.selectAll("circle")
  .data(dataset)
  .transition()
  .duration(2000)
  .attr("x", d => d.x + 2)
```

Updating attributes utilizes "tween"-ing functions for interpolation.  Many are
built in, but you can choose to build your own.


---

## D3 Notes: HTML

Anything that is in the DOM can be selected by, and modified by, D3.

```
`<p id="my_hi">hi there</p>`
```

How might we modify this paragraph?


---

## Transformations

SVG specifies a number of [possible transformations](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/transform).  These include rotation, translations, skews and scales.  We can set these through the `.attr` call, but note that this requires a bit of string manipulation.

```javascript
svg.selectAll("circle").attr("transform",
    (d, i) => "skewY(" + i*5 + ")");
```

Note that we're concatenating an integer to a string here, which casts the int
to a string.  This is a bit non-intuitive.

We can use this to reshape our objects in many different ways -- especially using the index of the data point!


---

## Scales

D3 provides a number of different [scaling types](https://github.com/d3/d3-scale).  We will be discussing, specifically, banded scales and color scales.

One thing to note is that d3 also provides handy functions for computing properties of [arrays](https://github.com/d3/d3-array).  For instance:

- `d3.min`, `d3.max`, `d3.minIndex` and `d3.maxIndex`, all of which accept both an iterable *and* an "accessor" function.
- `d3.extent`, which provides the format required by a scale.
- `d3.sum`, `d3.mean`, `d3.median`, `d3.quantil`, `d3.variance`


---

## Banded Scales

Banded scales provide discrete categorization of values, often used for categorical data.

```javascript
var band = d3.scaleBand(["low", "medium", "high"], [0.0, 100.0]);
```

This takes an array of input values for the domain.  Additional changes can be made to the bandwidth, padding, etc. The d3 wiki has a\
[diagram](https://raw.githubusercontent.com/d3/d3-scale/master/img/band.png) describing this.


---

## Color Scales

We can use `d3.scaleSequential` to generate colormaps for continuous values.  These accept an interpolator function.  d3 provides interpolator functions for many common colormaps.  For instance:

```javascript
var csc = d3.scaleSequential(d3.interpolateViridis).domain([1.0, 100.0])
```

We can also use log versions of these, and quantized versions.


---

## Brushing

Brushing in d3 requires some build-it-yourself effort, but building a brush object itself is straightforward.  You create a brush object with extents, and you call that on your object.  Here, `brushed` is a function to be called when the brush selection changes.

```

function brushed() {
  console.log(d3.event.selection);
}

var brush = d3.brush()
              .extent([[0.0, 0.0], [512, 512]])
              .on("brush", brushed);
svg.append("g").attr("class", "brush").call(brush);
```

What could we do with this?


---

## Paths

d3 can also generate `path` objects.  Typically these are generated using curve
generators.

```javascript
var line = d3.line().curve(
            d3.curveCatmullRom.alpha(0.5));
var myPath = svg.append("path")
              .attr("d", line(myPoints))
              .attr("stroke", "black");

```

Interpolation of paths can be tricky, but it is possible.


---

## End for now ...

Next time, we will bring this all together with our data visualization
techniques and export to a webpage.


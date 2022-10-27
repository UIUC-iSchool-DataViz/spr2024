---
title: Intro to Starboard
layout: lecture
tags:
  - javascript
  - starboard
  - dashboards
description: >-
  Today we'll start talking about the platform Starboard, and using it as a way to explore javascript and python
---

## Today

 * Comparison/Composition Visualizations
 * Javascript/HTML "Review"
 * Starboard

---

## Technology vs Technique

The last few weeks, we have focused strongly on the technology behind visualizations.

We're going to spend a bit of time at the start of each class working on the *techniques* behind the visualizations.

Today, we want to ask the question: how do we compare the elements that compose a whole?

---

## Composition

Don't use a pie chart.

<!-- .slide: data-background-image="images/piechart.png" data-background-size="auto 65%" data-background-position="right 50% bottom 50%" -->

notes:
pie charts force you to analyze things by area or angle, which are multidimensional attributes that are easy to confuse.

which is the most popular zoo animal in this pie chart? Elephants, otters, or lions?

---

## Composition

Don't use a pie chart.

<!-- .slide: data-background-image="images/piechartlabels.png" data-background-size="auto 65%" data-background-position="right 50% bottom 50%" -->

notes:
we can make a marginal improvement by labeling the values.

But we wouldn't be doing visualization if we were interested in just reading text.

---

## Composition

Don't use a pie chart.

<!-- .slide: data-background-image="images/3dpiechart.png" data-background-size="auto 65%" data-background-position="right 50% bottom 50%" -->

notes:
And if 2-dimensional area is difficult to understand, then 3-dimensional volume is even worse. 

3 dimensional values violate the principle of proportional ink, which states that:

 The sizes of shaded areas in a visualization need to be proportional to the data values they represent. 

---

## Alternatives

<!-- .slide: data-background-image="images/donutchart.png" data-background-size="auto 65%" data-background-position="right 50% bottom 50%" -->

notes:
Some people will try to sell you on a modified version of a pie chart called a donut chart that has a hole in the middle. This is a slight improvement because it helps you see the values in the circle as 1-dimensional arc length instead of area. 

But circles are still hard to decipher.

---

## Alternatives

<!-- .slide: data-background-image="images/treemap.png" data-background-size="auto 65%" data-background-position="right 50% bottom 50%" -->

notes:
We can reduce some of the confusion associated with using circles by creating proportional *rectangular* area. Now we can compare along the dimensions of height and width for certain values.

But area is still problematic because it asks us to compare two dimensions - width and height - simultaneously.

---

## Alternatives

<!-- .slide: data-background-image="images/barchart.png" data-background-size="auto 65%" data-background-position="right 50% bottom 50%" -->

notes:
you can show comparitive values more effectively with a bar chart though. These values are easily compared along just one dimension.

---

## Alternatives

<!-- .slide: data-background-image="images/waterfallchart.png" data-background-size="auto 65%" data-background-position="right 50% bottom 50%" -->

notes:
there are really quite a few alternatives. There are many ways to show data stacking up progressively. This waterfall chart shows how each value is part of a whole, which was sort of the idea of the pie chart.

---

## Comparison

<!-- .slide: data-background-image="images/columnchart.png" data-background-size="auto 65%" data-background-position="right 50% bottom 50%" -->

notes:
to compare values from multiple sources, you could use collected columns

---

## Comparison

<!-- .slide: data-background-image="images/stackedcolumnchart.png" data-background-size="auto 65%" data-background-position="right 50% bottom 50%" -->

notes:
Or to show they're part of a whole, use a stacked column chart

---

## Comparison

<!-- .slide: data-background-image="images/stackedareachart.png" data-background-size="auto 65%" data-background-position="right 50% bottom 50%" -->

notes:
or to show a time-series, use connected lines that stack on top of each other to show area across the whole canvass. This shows you trends and specific vertical values.

---

## Comparison

This is not a good idea.

![](images/comparepiecharts.png)

---

## Hierarchical data

<!-- .slide: data-background-image="images/hierarchical_zoos.png" data-background-size="auto 65%" data-background-position="right 50% bottom 50%" -->

notes:
Sometimes we want to show data in a proportion and show connections.
This often happens for hierarcical data.

Here in this example we want to show proportions of land based mammals that
are popular at the zoo and then as we move out we subdivide by the individual
animal species.

---


## Hierarchical data: example packages

 * Sunbursts (e.g., [snakeviz](https://jiffyclub.github.io/snakeviz/) )
 * Nested box area (e.g., [callgrind](https://kcachegrind.github.io/html/Home.html) ) - for
 showing flowchart like structures for things like code programs

<div class="multiCol" data-markdown=true>

<div class="col" data-markdown=true>

![](images/sunburst.png)

</div><div class="col" data-markdown=true>

![](images/callgrind.gif)

</div> </div>

---

## Javascript

A few weeks ago, in [lecture 10](../week05/lecture10.html) we went over some of the basics of HTML and Javascript.

Today, we'll review these, then put them into practice with [Starboard](https://starboard.gg/).

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

## ~~Iodide~~ Starboard

[Iodide](https://iodide.io) *was* a project originally from Mozilla for generating literate combinations of Javascript, Python (via [Pyodide](https://hacks.mozilla.org/2019/04/pyodide-bringing-the-scientific-python-stack-to-the-browser/)) and Markdown to a single document-based interface.

Iodide has been deprecated, and a new project has been built that takes lots of its best parts and puts them back to use!  It's called [Starboard](https://starboard.gg).

The documents you'll create will appear to be similar to Notebooks, and they share some of the same attributes and behaviors, but they will be different in how you create new cells, reorder cells, and version them.

---

## Starboard Cell Types

Starboard has a couple different primary cell-types: javascript, python, markdown and HTML.  There are others, but those are the four we will be working with.

To get started, create a new notebook -- you can do this in the "offline" format if you like! -- at [starboard.gg](https://starboard.gg/) and let's get started exploring what we can do.

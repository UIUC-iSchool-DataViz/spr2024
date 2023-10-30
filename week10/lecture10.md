---
title: Choosing Charts
layout: lecture
tags:
  - vega-lite
  - choosing-charts
description: >-
  We go over some types of visualization and finish our COVID lab discussion.
---

# Finish our lab and choosing viz

---

# Final Project

Your final project, to be completed in (assigned) groups, is to build a semi-realtime "observatory" of data.  You can find a few different possibilities for the data, but I want to encourage you to think of data related to things like:

- Earthquakes
- Space weather, rocks and satellites
- Weather
- Political events and news
- Nothing directly COVID related

---

# Final Project: Phases

Your final project will have three phases, each of which will have a component for you to turn in.  The second and third components will be turned in as a group, but the first component will be individually graded.

**Before beginning, clear your dataset with me, Esther and Tali.**

Some places to look for dataset ideas:

- [Awesome Public Datasets](https://github.com/awesomedata/awesome-public-datasets)
- [Public APIs](https://github.com/public-apis/public-apis)
- [Awesome JSON](https://github.com/jdorfman/awesome-json-datasets)


---

# Final Project: Part One

The first part of your project will be to *explore* the data; this is worth **30 points out of 100**.  You are to turn in a notebook -- whether in Observable, Python, Iodide, or a *combination* of them, that includes every single cell and step you took in exploring the data.  This is an example of the "visualization for self" process.  This means you might take shortcuts, you might make mistakes, and you might make really, really awful visualizations.

Showing that you explored the data individually (even if you did it while talking to a collaborator or classmate) is enough to satisfy this portion of the project.

Download the data.  Dig in to it.  Look at it.  Find interesting things. Experiment.  Make plots, make summary statistics, and make it familiar to yourself.

---

# Final Project: Part Two

The second component of your project will be to design your "observatory." Note that this is *pseudo* realtime.  It does not need to *automatically* update its data; a refresh button, or requiring a page reload, is sufficient.  This is worth **40 points out of 100**.

This could consist of drawings, prototypes of web pages or notebooks, and the code necessary to build the different components.  This component is *scaffolding*, not the final product.  It should be thought of as the "viz for peers" stage of the process.  You can submit drawings (including photos of drawings), jamboards, notebooks (which you *are* allowed to clean up), and stages of prototypes.

You should walk me through the process of how you built your observatory, and it does not need to be a linear, successes-only process.


---

# Final Project: Part Three

The final component of your project will be an in-class presentation of your work, and the deployment of that work online.  This will be worth **30 points out of 100**.

Your "observatory" should be located somewhere online, either as a notebook I can download and run (or that runs on [binder](https://mybinder.org/)) or view on observable, starboard, or Github Pages.

You will also have five minutes, as a *group*, to present your work in class. This presentation needs to include the final product, as well as a summary of any stumbling blocks you might have had, and a little bit of the process.

These presentations will be on the 4th of December.  The project will be due by the end of finals week, but you are **strongly encouraged** to submit earlier.

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


## Hierarchical data: example packages

- Sunbursts (e.g., [snakeviz](https://jiffyclub.github.io/snakeviz/) )
- Nested box area (e.g., [callgrind](https://kcachegrind.github.io/html/Home.html) ) - for
  showing flowchart like structures for things like code programs

<div class="multiCol" data-markdown=true>

<div class="col" data-markdown=true>

![](images/sunburst.png)

</div><div class="col" data-markdown=true>

![](images/callgrind.gif)

</div> </div>


---

## Putting this Together

We can now put this together to construct a COVID-19 dashboard.

It must have these characteristics:

- Linking states to county-level visualizations
- Supplemental information about states, including an image
- Time series for national cases

How can we utilize our transforms, filters and view composition to this end?


---

## Linking and Brushing

We will update our visualization to include:

1. Display time series aggregations based on selection
1. Display county-by-county visualizations for Illinois
1. State-by-state counties, based on state selection


---

## Step 1: Display a Map

1. Find a GeoJSON map of the United States
	1. By State
	2. By County
2. Utilize the `geoshape` mark.
3. Apply a tooltip.

---

## Step 2: Color Map by Reported Cases

1. Load the NYT COVID dataset.
2. Find the maximum value per state.
3. Assign this value to the `color` encoding.

---

## Step 3: Display Selected State

1. Add on a new parameter that maps to the selected state.
2. Set up a `text` mark that simply displays the state name
3. Add on the maximum number of cases to the `text` mark

---

## Step 4: Time Series for State

1. Create a new time series for a specific state
2. Map the selected parameter to the time series
3. Expand to use multiple selections

---

## Step 5: Time Series Selection

1. Enable selection of time periods in the state time series
2. Map this to the input to the aggregation of maximum values
3. Utilize this new selection for the color encoding

---

## Images

Let's get some Flags up there!

https://uiuc-ischool-dataviz.github.io/data/state_abbreviations.json
https://github.com/OpenGovDataMirror/F_CivilServiceUSA_us-states/tree/master/images/flags

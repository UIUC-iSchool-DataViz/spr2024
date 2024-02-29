---
title: Beginning Geospatial Visualization
layout: lecture
tags:
  - geo
  - bqplot
  - cartopy
  - maps
description: >-
  We start out learning about the basics of geospatial visualization, including applying this in bqplot and cartopy.
---

# Beginning Geographic Visualization

---

## Buildings and Dates

Let's revisit our building dataset.  When we first load the data:

```python
df = pd.read_csv("building_inventory.csv", na_values={
    "Year Acquired": 0,
    "Year Constructed": 0,
    "Square Footage": 0
})
```

Our columns, from `df.dtypes`, look like:

```
...
Year Acquired              float64
Year Constructed           float64
...
```

But maybe we want these to be date time.

---

## Converting to datetime

We can convert our dataframe columns using casting and assignment:

```python
df["Year Acquired"] = ...
```

Note that if default parsing worked, we could supply a `parse_dates` argument.  But ours requires some modification.

We can use `pd.to_datetime` to convert our dates.  What happens if we use it without any arguments?  Why?

It accepts a `format` argument, which for us will be `"%Y"`.

---

## Before We Begin

Today we're going to talk about data from the COVID-19 pandemic.

This is a serious topic, and one that touches all of us, and if you are uncomfortable with this topic, I invite you to sign off and connect with me directly.

---

## Maps

The Earth is a sphere.

(Fun question: to what degree is it a sphere?)

Have you ever wrapped a piece of paper around a ball?

---

## Projections

To map from one system to another, we must "project" from the original sphere to the flat object we are observing.

What are some things we could preserve during such a projection?

---

## Projections: Common Preservations

Typically, one or more of these will be preserved, or at least, the distortion will be minimized:

- Area
- Shape (Conformal)
- Distance

There are other properties that can be preserved, as well.  Typically, maps will be a "compromise" between preserving different properties.

What happens when we preserve one property over another?

Mercator is a "conformal" projection.  What is wrong with this?

---

<!-- .slide: data-background-image="images/mercator.png" data-background-size="auto 80%" -->

---

## Discussion

What happens when we make a map that minimizes one region and maximizes another?


---

## Projections: Distortions

We can characterize distortions in a projection by examining how a known shape appears on them.  The Tissot Ellipse of Distortion is a method of showing this by drawing circles of a fixed radius and examining their elliptical distortion.

What do you notice?

---

<!-- .slide: data-background-image="images/mercator.png" data-background-size="auto 80%" -->

---

<!-- .slide: data-background-image="images/mercator_tissot.png" data-background-size="auto 80%" -->

---

<!-- .slide: data-background-image="images/transversemercator.png" data-background-size="auto 95%" -->

---

<!-- .slide: data-background-image="images/transversemercator_tissot.png" data-background-size="auto 95%" -->

---

<!-- .slide: data-background-image="images/lambertcylindrical.png" data-background-size="auto 95%" -->

---

<!-- .slide: data-background-image="images/lambertcylindrical_tissot.png" data-background-size="auto 95%" -->

---

<!-- .slide: data-background-image="images/mollweide.png" data-background-size="auto 95%" -->

---

<!-- .slide: data-background-image="images/mollweide_tissot.png" data-background-size="auto 95%" -->

---

<!-- .slide: data-background-image="images/sinusoidal.png" data-background-size="auto 95%" -->

---

<!-- .slide: data-background-image="images/sinusoidal_tissot.png" data-background-size="auto 95%" -->

---

<!-- .slide: data-background-image="images/gnomonic.png" data-background-size="auto 95%" -->

---

<!-- .slide: data-background-image="images/gnomonic_tissot.png" data-background-size="auto 95%" -->


---

## Maps: Coordinate Systems

Once we have our system of transformation, we need to have a method of representing positions.

Three common baseline methods:

- Spherical coordinates
- Latitude and Longitude
- Degrees, minutes, seconds

Take care with:

- Zero points
- North/South, East/West
- Ranges


---

## Map Viz

- kepler.gl
- Google Maps & Earth
- WorldWide Telescope
- CesiumJS
- bqplot
- Vega & friends
- Cartopy

---

## bqplot review

Construct `Figure` objects from `Mark` objects.  Relate points to each other with `Scale` objects, display them using `Mark` objects that are keyed to a set of `Scale` objects, and apply interaction using `ipywidgets` and `traitlets`.

---

## bqplot objects

- A mark is some mechanism for displaying data.  For example, we might have data that has a set of x and y values, which we can use `Lines` to represent.
- `Scale` objects describe relationships between visual attributes (position) and data values.
- `Axis` objects are where data are placed.
- `Figure` objects contain marks and axes, as well as interaction.

---

## bqplot: Simple Line Plot Review

Our first example will be a simple lineplot.

```#python
import bqplot
import numpy as np

x = np.arange(100)
y = np.random.random(100) + 5

x_sc = bqplot.LinearScale()
y_sc = bqplot.LinearScale()
lines = bqplot.Lines(x = x, y = y, scales = {'x': x_sc, 'y': y_sc})
ax_x = bqplot.Axis(scale = x_sc, label = 'X value')
ax_y = bqplot.Axis(scale = y_sc, label = 'Y value', orientation = 'vertical')
fig = bqplot.Figure(marks = [lines], axes = [ax_x, ax_y])
display(fig)
```


---

## bqplot review

Construct `Figure` objects from `Mark` objects.  Relate points to each other with `Scale` objects, display them using `Mark` objects that are keyed to a set of `Scale` objects, and apply interaction using `ipywidgets` and `traitlets`.

---

## bqplot objects

- A mark is some mechanism for displaying data.  For example, we might have data that has a set of x and y values, which we can use `Lines` to represent.
- `Scale` objects describe relationships between visual attributes (position) and data values.
- `Axis` objects are where data are placed.
- `Figure` objects contain marks and axes, as well as interaction.

---

## bqplot: Simple Line Plot Review

Our first example will be a simple lineplot.

```#python
import bqplot
import numpy as np

x = np.arange(100)
y = np.random.random(100) + 5

x_sc = bqplot.LinearScale()
y_sc = bqplot.LinearScale()
lines = bqplot.Lines(x = x, y = y, scales = {'x': x_sc, 'y': y_sc})
ax_x = bqplot.Axis(scale = x_sc, label = 'X value')
ax_y = bqplot.Axis(scale = y_sc, label = 'Y value', orientation = 'vertical')
fig = bqplot.Figure(marks = [lines], axes = [ax_x, ax_y])
display(fig)
```


---

## bqplot review

Construct `Figure` objects from `Mark` objects.  Relate points to each other with `Scale` objects, display them using `Mark` objects that are keyed to a set of `Scale` objects, and apply interaction using `ipywidgets` and `traitlets`.

---

## bqplot objects

- A mark is some mechanism for displaying data.  For example, we might have data that has a set of x and y values, which we can use `Lines` to represent.
- `Scale` objects describe relationships between visual attributes (position) and data values.
- `Axis` objects are where data are placed.
- `Figure` objects contain marks and axes, as well as interaction.

---

## bqplot: Simple Line Plot Review

Our first example will be a simple lineplot.

```#python
import bqplot
import numpy as np

x = np.arange(100)
y = np.random.random(100) + 5

x_sc = bqplot.LinearScale()
y_sc = bqplot.LinearScale()
lines = bqplot.Lines(x = x, y = y, scales = {'x': x_sc, 'y': y_sc})
ax_x = bqplot.Axis(scale = x_sc, label = 'X value')
ax_y = bqplot.Axis(scale = y_sc, label = 'Y value', orientation = 'vertical')
fig = bqplot.Figure(marks = [lines], axes = [ax_x, ax_y])
display(fig)
```


---

## Geographic Locations

bqplot provides the `Map` mark.

The `scales` property for the `Map` mark should have entries for both `projection` and `color`.  `projection` will be one of the geographic projections (more on that next time) such as `AlbersUSA`.

The `Map` mark must also have `map_data` defined, which is a TopoJSON file.  For our purposes, we will use the built-in `bqplot` function `topo_load`, which can accept one of:

- `map_data/USCountiesMap.json` for US county-level data keyed by FIPS
- `map_data/USStatesMap.json` for US state-level data
- `map_data/EuropeMap.json` for European countries
- `map_data/WorldMap.json` for the full Earth


---

## Intro to cartopy

CartoPy is a toolkit that builds on matplotlib to create fast, easy map representations.

We will be relying on three key concepts:

- Axes projections (similar to our polar projections)
- Coordinate representations
- Shapes

Using these, we will be able to build out many visualizations.

---

## CartoPy: Projections

We start out by constructing an axes in CartoPy that uses a given projection:

```python
import cartopy
import matplotlib.pyplot as plt

fig = plt.figure()
ax = fig.add_subplot(111, projection=cartopy.crs.Mollweide())
ax.coastlines()
```

What does this do?

---

## CartoPy: Coordinate Reference Systems

Transforming from a spherical reference system to a flat reference system is the job of the projection; transforming from one discretization of a sphere to another is the job of the coordinate system.

We can utilize Coordinate Reference Systems to describe the *input* coordinate system and the *rasterization* system are described.

For example, there are several different ways to draw "straight" lines.  We can do both `PlateCarree` and `Geodetic`.

```python
c_lat, c_lon = 40.1164, -88.2434
a_lat, a_lon = -18.8792, 47.5079
fig = plt.figure()
ax = fig.add_subplot(111, projection = cartopy.crs.PlateCarree())
ax.gridlines()
ax.coastlines()
ax.set_global()
ax.plot([c_lon, a_lon], [c_lat, a_lat], transform = cartopy.crs.PlateCarree())
ax.plot([c_lon, a_lon], [c_lat, a_lat], transform = cartopy.crs.Geodetic())
```

---

<!-- .slide: data-background-image="images/map_plot1.png" data-background-size="auto 95%" -->

---

<!-- .slide: data-background-image="images/map_plot2.png" data-background-size="auto 95%" -->


---

## Today

Today, we will use the `Map` mark, our interval selectors, and our county/state level data to generate some visualizations of COVID cases.

Each number we see is a human life, changed.

---

## Time Series Data

We will utilize some data from the NY Times.

[NYT COVID-19 Data](https://github.com/nytimes/covid-19-data)

```
!rm -f us-counties.csv ; wget https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-counties.csv
```

We will build:

1. A basic display of totals.
2. A timeline selector for aggregate values.
3. Interactive display of supplemental data.

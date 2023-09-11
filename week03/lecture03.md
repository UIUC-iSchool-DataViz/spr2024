---
title: Data Storage, Manipulation, and Drawing
layout: lecture
tags:
  - operations
  - distributions
  - scaling
  - dataformats
  - drawing
description: >-
  How are values transformed from 0's and 1's into values we can manipulate and understand?  When we draw something on a screen, how do we represent that internally, and how is that translated into pixels?  What operations can we do on data? 
date: 2023-08-28
---

## Scaling Data

---

## Transformations

We will need to transform data in order to apply consistent visual encoding.
There are many reasons we may need to accomplish this, including color mapping,
applying units, and co-registration or normalization of data.

One of the most important transformations we will have is that of an [Affine transformation](https://en.wikipedia.org/wiki/Affine_transformation).  This is a transformation that preserves:

- Collinearity
- Parallellism
- Convexity
- Ratios of parallel lines
- Barycenters of point sets


---


## Transformations

Affine transformations satisfy:

$ \vec{y} = A\vec{x} + \vec{b} $

<!-- .slide: data-background-image="images/affine_1.svg" data-background-size="30% auto" data-background-position="right 20% bottom 50%" -->


---


## Transformations

Affine transformations satisfy:

$ \vec{y} = A\vec{x} + \vec{b} $

We can use these to accomplish:

- Shifts

<!-- .slide: data-background-image="images/affine_2.svg" data-background-size="30% auto" data-background-position="right 20% bottom 50%" -->


---


## Transformations

Affine transformations satisfy:

$ \vec{y} = A\vec{x} + \vec{b} $

We can use these to accomplish:

- Shifts
- Rotations

<!-- .slide: data-background-image="images/affine_3.svg" data-background-size="30% auto" data-background-position="right 20% bottom 50%" -->


---


## Transformations

Affine transformations satisfy:

$ \vec{y} = A\vec{x} + \vec{b} $

We can use these to accomplish:

- Shifts
- Rotations
- Scaling

<!-- .slide: data-background-image="images/affine_4.svg" data-background-size="30% auto" data-background-position="right 20% bottom 50%" -->


---


## Transformations

<div class="col" data-markdown=true>

In this figure, we can adjust the mixing vectors and the offset.  What do you notice about colinear points and parallel lines?

<div class="fig-container" data-style="height: 600px;" data-file="figures/affine_transformation.html" data-markdown=true>
</div>


---

## Scales and Scaling

Displaying a quantity requires assigning to it a given representation.
A common mechanism for doing this is to vary the color of a particular region
or set of display units with respect to the quantity expressed in those units.

In mathematical notation, we first "normalize" our data value by assigning to a
range:

$g(v) \rightarrow v' \in [0, 1]$

and then, given a color mapping function, assign to this a given color:

$f(v') \rightarrow (R, G, B)$


---


## Scales and Scaling

Group discussion:

- How is this similar to or different from our discussions of "binning" and
  histogramming?
- What are some functions we can use for $g(v)$?
- What are some considerations we need to take into account for variable
  bins?

---

<!-- .slide: data-background-image="https://upload.wikimedia.org/wikipedia/commons/e/e8/1414_Rods_and_Cones.jpg" data-background-size="auto 75%" data-background-position="right 30% bottom 50%"-->

## How Do Colors Work?

<div class="left" data-markdown=true>

Rods (low-light) and cones (color) mediate vision. Humans have about 20 times
as many rods (120 million) as cones (6 million).

Roughly speaking, cones see in the colors red, green and blue.

By OpenStax College [CC BY 3.0](http://creativecommons.org/licenses/by/3.0),
via Wikimedia Commons

</div>

---

## Let's Try It

http://enchroma.com/test/instructions/

---

## Color Responsivity Function

<!-- .slide: data-background-image="images/resp.png" data-background-size="auto 75%" -->


---

## "Naming" Colors

- RGB triplets, often expressed in hexadecimel ("#00FFAA", etc)
- Color spaces
  - HSV (Hue, saturation, value)
  - [CIELAB](https://en.wikipedia.org/wiki/CIELAB_color_space)
  - sRGB, Adobe sRGB
- List of colors by name
  - [Web](https://www.w3schools.com/colors/colors_names.asp)
  - [matplotlib](https://matplotlib.org/2.0.2/examples/color/named_colors.html)

---

## "Naming" Colors: HSV

<div class="fig-container" data-style="height: 650px;" data-file="figures/hsv_space.html" data-markdown=true>
</div>

---

## HSV Wheel

https://commons.wikimedia.org/wiki/File:HSV_color_solid_cylinder.png

By HSV_color_solid_cylinder.png: SharkD derivative work: SharkD Talk \[CC BY-SA 3.0
(http://creativecommons.org/licenses/by-sa/3.0) or GFDL
(http://www.gnu.org/copyleft/fdl.html)\], via Wikimedia Commons


---

## Color Palettes

- Colorbrewer Categories
  - Sequential
  - Diverging
  - Qualitative
- Resources:
- colorbrewer.org
- palettable (package)

---

## Sequential Colormaps

![blues discrete colormap](images/blues_discrete.png)

![blues continuous colormap](images/blues_continuous.png)

---


## Diverging Colormaps

![spectral discrete colormap](images/spectral_discrete.png)

![spectral continuous colormap](images/spectral_continuous.png)

---

## Qualitative Colormaps

![discrete set1 of colors](images/set1_discrete.png)

![continuous set1 of colors](images/set1_continuous.png)

(See?  Works better as discrete!)


---

## It's full of colors

https://commons.wikimedia.org/wiki/File:16777216colors.png


---

## Palette Mapping

![](images/set1_discrete.png)

Assign each value to a specific color or element.

---

## Color Mapping

$f(v) \\rightarrow (R, G, B)$

We can also re-map:

$f(v') \\rightarrow (R, G, B)$

$v' = f(v)$

For instance, with logs or squares.

---

## Color Mapping: Linear Mapping

We map from a range of values to (0, 1):

$ v' = (v - v_0)/(v_1 - v_0) $


---

## Colormaps: `gray`

<!-- .slide: data-background-image="images/gray_colors.png" data-background-size="auto 75%" -->


---


## Colormaps: `gray`

<!-- .slide: data-background-image="images/gray_3d.png" data-background-size="auto 75%" -->

---

## Colormaps: `gist_stern`

<!-- .slide: data-background-image="images/gist_stern_colors.png" data-background-size="auto 75%" -->


---


## Colormaps: `gist_stern`

<!-- .slide: data-background-image="images/gist_stern_3d.png" data-background-size="auto 75%" -->

---

## Colormaps: `jet`

<!-- .slide: data-background-image="images/jet_colors.png" data-background-size="auto 75%" -->


---


## Colormaps: `jet`

<!-- .slide: data-background-image="images/jet_3d.png" data-background-size="auto 75%" -->

---

## Colormaps: `magma`

<!-- .slide: data-background-image="images/magma_colors.png" data-background-size="auto 75%" -->


---


## Colormaps: `magma`

<!-- .slide: data-background-image="images/magma_3d.png" data-background-size="auto 75%" -->

---

## Colormaps: `viridis`

<!-- .slide: data-background-image="images/viridis_colors.png" data-background-size="auto 75%" -->


---


## Colormaps: `viridis`

<!-- .slide: data-background-image="images/viridis_3d.png" data-background-size="auto 75%" -->


---

## Image Coloring

<div class="fig-container" data-style="height: 600px;" data-file="figures/example_coloring_image.html" data-markdown=true>
</div>

---

## Colormapping Images

<div class="fig-container" data-style="height: 640px;" data-file="figures/apply_colormap.html" data-markdown=true>
</div>

---

## Back to Matplotlib

We will explore our first visualization concepts using matplotlib.

Please be sure to see the "cheat sheets" affiliated with this, which can be found at:

[github.com/matplotlib/cheatsheets/](https://github.com/matplotlib/cheatsheets/)

and which have been duplicated in this repository.

---

## How Does Matplotlib Place Elements

Matplotlib has three primary methods of specifying coordinates, each of which is represented by a different **transformation**.

- **data** coordinates
- **axes** coordinates
- **figure** coordinates

These are not exhaustive!  Other transformations exist, such as for the display units, specifying things in inches, and also sub-figure systems.

---

## Matplotlib Coordinate Systems

<!-- .slide: data-background-image="images/matplotlib-coordinates.svg" data-background-size="auto 75%" -->

---

## Specifying Coordinates

We often want to utilize non-data coordinate systems

- Annotations
- Markup
- Positioning of axes and subplots

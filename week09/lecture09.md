---
title: Intermediate Vega Lite
layout: lecture
tags:
  - vega-lite
  - maps
  - brushing-linking
description: >-
  We examine how to apply filters, transforms and parameters toward building a map with time series data.
---

# Intermediate Vega Lite

---

## Time Series Data

We will once again utilize some data from the NY Times.

[NYT COVID-19 Data](https://github.com/nytimes/covid-19-data)

Use the link to the `raw` view.

---

## GeoJSON I - Intro

http://geojson.org/ and http://geojson.io/

```json
{
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [125.6, 10.1]
  },
  "properties": {
    "name": "Dinagat Islands"
  }
}
```

---

## GeoJSON II - Primitive Types

GeoJSON defines several primitive types:

- `Point` with associated `coordinates` (single set of two)
- `LineString` with associated `coordinates` that are lists of two items each
- `Polygon` with holes able to be cut out of it

Multi-part types are defined out of these.

---

## GeoJSON III - MultiPart Types

These are combined into multi-part types as follows:

- `MultiPoint`
- `MultiLineString`
- `MultiPolygon`

---

## Today: vega-lite

- vega-lite
  - transforming data
  - filtering data
- types of marks
  - arc
  - trail
  - image
  - geoshape
- encodings
  - tooltips


---

## Transforming

There are several `transform` options that we have not yet explored in detail.

- `fold`
- `flatten`
- `lookup`
- `window`
- `aggregate`


---

## Transform: `fold`

"`fold`"-ing a dataset transforms it by *expanding* to include additional keys,
to enable an additional parameter for selection.  For instance, if we have a
dataset of the form:

```json
[ {'name': ..., 'prop1': ..., 'prop2': ...}, ... ]
```

we can apply a `fold` of `prop1` and `prop2` to obtain:

```json
[ {'key': 'prop1', 'value': ..., 'name': ..., 'prop1': ..., 'prop2': ...},
  {'key': 'prop2', 'value': ..., 'name': ..., 'prop1': ..., 'prop2': ...} ]
```


---

## Transform: `flatten`

This operates similarly to `fold`, except that the inputs for `prop1` and
`prop2` are allowed to be arrays.  This produces a new set of rows in the
output data, one for each element in the corresponding arrays.

(This is less likely to be useful for input CSV data.)


---

## Transform: `lookup`

Occasionally we will want to use keys for lookup, based on other data rows.  We
can look up either in a named data source or in a fully-specified inline
datasource.

```json
{ "lookup": "primaryKey",
  "from": {
    "data": {"name": "referenceData"}
    "key": "keyInReference",
    "fields": ["prop1", "prop2"],
  }
}
```


---

## Transform: `window`

A `window` transform lets us perform an operation on a set of multiple rows in our dataset.  *Crucially* we can also specify the *order* that they are processed in.  It is defined like this:

```json
  "window": [{
	  "op": ...,
	  "field": ...,
	  "param": ...,
	  "as": ...
  }],
  "sort": [
	{"field": ..., "order": ...}
  ],
  "ignorePeers": ...,
  "groupby": [
	"..."
  ],
  "frame": [...,...]
```

(We will demonstrate this!)


---

## Aggregate Transforms

We have seen the `aggregate` transform before:

```json
{
  "transform": [
    {
      "type": "aggregate",
      "fields": ["state"],
      "ops": ["count"],
      "as": ["state_count"]
    }
  ]
}
```

This creates a new aggregated dataset, with `state` and `state_count`.

---

## Aggregate with Groupby

We can also groupby, and perform (for instance) the maximum value among all the states.

```json
{
  "transform": [
    {
      "type": "aggregate",
      "fields": ["cases"],
      "ops": ["max"],
      "groupby": ["state"],
      "as": ["max_cases"]
    }
  ]
}
```

Now, we get for each `state` the maximum number of cases.

The `joinaggregate` operation may also be useful in this situation.

---

## Filtering

- ranking
- parameters


---

## Marks

- `arc`
- `trail`
- `image`
- `geoshape`

---

## Arc Mark

The arc mark is typically used for displaying radial plots, such as pie or donut charts.

```json
{ 
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "data": {
    "values": [
      {"category": "A", "value": 1},
      {"category": "B", "value": 4},
      {"category": "C", "value": 12},
      {"category": "D", "value": 10},
      {"category": "E", "value": 1},
      {"category": "F", "value": 5}
    ]
  },
  "mark": "arc",
  "encoding": {
    "theta": {"field": "value", "type": "quantitative"},
    "color": {"field": "category", "type": "nominal"}
  }
}  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
```

See also the `arc` configuration, which includes `radius` and `radius2`.

---

## Trail Mark

The `trail` mark can display lines with variable widths.

```json
{
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "data": {
    "sequence": {
      "start": 0,
      "stop": 15.0,
      "step": 0.05,
      "as": "x"
    }
  },
  "transform": [
    {"calculate": "cos(datum.x)", "as": "y"},
    {"calculate": "pow(sin(datum.x), 2) + 0.1", "as": "w"}
  ],
  "mark": "trail",
  "encoding": {
    "x": {"field": "x", "type": "quantitative"},
    "y": {"field": "y", "type": "quantitative"},
    "size": {"field": "w", "type": "quantitative"}
  }
}
```

---

## Image Mark

We'll do more with this one later, but you can use it to display an image at a particular location and from a particular URL.

From the vega-lite docs:

```json
{
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "data": {
    "values": [
      {"x": 0.5, "y": 0.5, "img": "data/ffox.png"},
      {"x": 1.5, "y": 1.5, "img": "data/gimp.png"},
      {"x": 2.5, "y": 2.5, "img": "data/7zip.png"}
    ]
  },
  "mark": {"type": "image", "width": 50, "height": 50},
  "encoding": {
    "x": {"field": "x", "type": "quantitative"},
    "y": {"field": "y", "type": "quantitative"},
    "url": {"field": "img", "type": "nominal"}
  }
}
```

---

## Geoshape Mark

For `geoshape` marks, we need to specify a `projection` as well as a dataset in **TopoJSON** format.  Converting between GeoJSON and TopoJSON can be tricky at times, but is tractable.

```json
{
  "data": {
    "url": "data/us-10m.json",
    "format": {
      "type": "topojson",
      "feature": "counties"
    }
  },
  "projection": {
    "type": "albersUsa"
  },
  "mark": "geoshape",
  ...
}
```

---

## Encoding Channels

- latitude / longitude
- color
- text / tooltip / href


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

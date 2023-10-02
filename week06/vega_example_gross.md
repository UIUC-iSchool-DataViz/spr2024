---
title: vega-lite example Gross Revenue
description: Aggregation and interactivity with IMDB data
layout: vegalite_example
---

{
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "data": {"url": "https://vega.github.io/editor/data/movies.json"},
  "hconcat": [
    {
      "layer": [
        {
          "mark": {"type": "bar", "cornerRadius": 10},
          "encoding": {
            "x": {"field": "IMDB Rating", "type": "quantitative", "bin": true},
            "y": {"aggregate": "sum", "field": "US Gross"},
            "color": {"value": "grey"}
          }
        },
        {
          "transform": [{"filter": {"param": "myfirstparam"}}],
          "mark": {"type": "bar", "cornerRadius": 10},
          "encoding": {
            "x": {"field": "IMDB Rating", "type": "quantitative", "bin": true},
            "y": {"aggregate": "sum", "field": "US Gross"}
          }
        }
      ]
    },
    {
      "mark": {"type": "point", "shape": "diamond"},
      "params": [{"name": "myfirstparam", "select": "interval"}],
      "encoding": {
        "x": {"field": "US Gross", "type": "quantitative"},
        "y": {"field": "Worldwide Gross", "type": "quantitative"},
        "color": {"value": "grey", "condition": {"param": "myfirstparam"}}
      }
    }
  ]
}


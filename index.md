---
layout: index
---

This is the course website for {{site.data.class.course.name}}, instructed by
{% for instructor in site.data.class.instructors -%}
{{instructor.name}} ({{instructor.email}}){% unless forloop.last %}, {% endunless -%}
{%- endfor -%}.

Below, you will find the materials for each week, as well as the [syllabus](syllabus) that
includes contact information and a course outline.

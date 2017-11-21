---
layout: single
title: Data Preprocessing
permalink: /preprocessing/
---

## Introduction


On this page I will hold tutorials, documentation about general data preprocessing steps for Hong Kong data (Currently PCD data(Private) and [Speed-Panel data](https://data.gov.hk/en-data/dataset/hk-td-sm_1-traffic-speed-map))
There are three categories of documents on this page: Code description files for the Data models used in this project, Software design guideline descriptions for current project and a Step by step tutorial for going through the data preprocessing steps for Hong Kong traffic data.

The code for the preprocessing steps is available at:

+ https://github.com/AndresNamm/Incident-Detection-Java
+ https://github.com/AndresNamm/Incident-Detection-Visualization

## Topics


### Code Descriptions

On this code Documentation page I keep more thorough descriptions ofsome important/complex
code files available at [Github](https://github.com/AndresNamm/) This is only a **Supplementary** help in addition to reading the code.
{% assign groups = site.preprocessing-code | group_by: "category" | sort: "name" %}
+ {% for group in groups %}
    <div class="cookie">
       {{ group.name }}
    <div class="cookie">
    {% for item in group.items %}
       <div class="cookie">
           <h3><a href="{{ item.url | relative_url }}">{{ item.title }}</a></h3>
       <div class="cookie">
    {%endfor%}
{%endfor%}


### Design

[Short Summary]({{site.baseurl}}{{page.url}}design) of Design Guidelines for this project with info about responsibility division, UML, testing and deployment

### Gothrough tutorial of the Preprocessing Steps


+ Here you can find info about:
{% for cookie in site.preprocessing-gothrough %}
  <div class="cookie">
    <h3><a href="{{site.baseurl}}{{ cookie.url }}">{{ cookie.title }}</a></h3>
  </div>
{% endfor %}

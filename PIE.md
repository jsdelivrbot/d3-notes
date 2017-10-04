# Creating a Pie Chart

## Set Up

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script src="https://d3js.org/d3.v4.min.js"></script>
    </head>
    <body>
        <svg>
            <g></g>
        </svg>
        <script src="app.js" charset="utf-8"></script>
    </body>
</html>
```

## Set Config Vars

```javascript
var WIDTH = 360;
var HEIGHT = 360;
var radius = Math.min(WIDTH, HEIGHT) / 2;

var dataset = [
    { label: 'Abulia', count: 10 },
    { label: 'Betelgeuse', count: 20 },
    { label: 'Cantaloupe', count: 30 },
    { label: 'Dijkstra', count: 40 }
];
```

## Create an Ordinal Scale

An ordinal scale maps discrete values (can't be interpolated) to discrete values.

```javascript
var mapper = d3.scaleOrdinal();
mapper.range([45, 63, 400]); //list each value for ordinal scales, not min/max
mapper.domain(['Bob', 'Sally', 'Zagthor']); //list each value for ordinal scales, not min/max

console.log(mapper('Bob'));
console.log(mapper('Sally'));
console.log(mapper('Zagthor'));
```

You cannot invert ordinal scales:

```javascript
console.log(mapper.invert(45));
```

## Create the color scale to map labels to colors

D3 comes with lots of pre-made color schemes:

- https://github.com/d3/d3-scale/blob/master/README.md#category-scales
- https://github.com/d3/d3-scale-chromatic#categorical

They're just arrays:

```javascript
console.log(d3.schemeCategory10)
```

Consequently, we can use them when setting a range:

```javascript
var colorScale = d3.scaleOrdinal();
colorScale.range(d3.schemeCategory10);
```

We can generate an array of labels for the domain using JavaScript's map function:

```javascript
colorScale.domain(dataset.map(function(element){
    return element.label;
}));
```

## Set up the svg

Standard:

```javascript
d3.select('svg')
    .attr('width', WIDTH)
    .attr('height', HEIGHT);
```

## Add paths for each pie segment

```javascript
var path = d3.select('g').selectAll('path')
    .data(dataset)
    .enter()
    .append('path')
    .attr('fill', function(d) {
        return colorScale(d.label);
    });
```
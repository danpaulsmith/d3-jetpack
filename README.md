d3-jetpack is a set of nifty convenience wrappers that speed up your daily work with d3.js

[![jetpack](http://36.media.tumblr.com/tumblr_m4kkxd8nWB1rwkrdbo1_500.jpg)](http://myjetpack.tumblr.com/post/23725103159)
  (comic by [Tom Gauld](http://myjetpack.tumblr.com/]))

Here's what's in the package:

#### selection.append / selection.insert

Appending and inserting with classes/ids 

```js
selection.append("div.my-class");
selection.append("div.first-class.second-class");
selection.append("div#someId");
selection.append("div#someId.some-class");

// works with insert, too
selection.insert("div.my-class");
```

#### selection.tspans

For multi-line SVG text

```js
selection.append('text').tspans(['Multiple', 'lines']);
selection.append('text')
    .tspans(function(d) {
        return d.text.split('\n');
    });
```

#### d3.wordwrap

Comes in handy with the tspans..

```js    f
selection.append('text')
    .tspans(function(d) {
        return d3.wordwrap(text, 15);  // break line after 15 characters
    });
```

#### selection.translate

How I hated writing ``.attr('transform', function(d) { return 'translate()'; })`` a thousand times...

```js
svg.append(g).translate([margin.left, margin.top]);
tick.translate(function(d) { return  [0, y(d)]; });
```

#### ƒ

`ƒ` takes a string and returns a function that takes an object and returns whatever property the string is named. This clears away much of verbose `function(d){ return ... }` syntax in ECMAScript 5:

```js
x.domain(d3.extent(items, function(d){ return d.price; }));
```

becomes 

```js
x.domain(d3.extent(items, ƒ('price'));
```

#### attrC and styleC
`attrC` takes the name of an attribute and any number of functions, using the composition of the functions to map data bound to each element its attribute's value. 

```js
circles.attrC('cx', x, ƒ('price'));
```

Instead of 

```js
circles.attrC('cx', function(d){ return x(d.price); });
```

#### selection.appendData

Instead of making an empty selection, binding a data, taking the enter selection and appending elements as separate steps:

```js
svg.selectAll('circle')
    .data(data)
    .enter()
    .append('circle')    
```

Use `appendData`:


```js
svg.appendData('circle', data)
```

#### conventions
`d3.conventions()` appends an `svg` element with a `g` element according to the  [margin convention](http://bl.ocks.org/mbostock/3019563) to the page and returns an object with the following properties:

`width`, `height`, `margin`: size of the `svg` and its margins

`parentSel`: `d3.selection` of the element the `svg` was appended to. Defaults to `d3.select("body")`, but like every other returned value, can be specified by passing in an object: `d3.conventions({parentSel: d3.select("#graph-container"), height: 1300})` appends an svg to `#graph-container` with a height of 1300.

`svg`: `g` element translated to make room for the margins

`x`: Linear scale with a range of `[0, width]`

`y`: Linear scale with a range of `[height, 0]`

`xAxis`: Axis with scale set to x and orient to "bottom"

`yAxis`: Axis with scale set to y and orient to "left"

`drawAxis`: Call to append axis group elements to the svg after configuring the domain. Not configurable.
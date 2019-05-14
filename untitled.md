# ch2



## D3 问题&解决记录

### 问题1 坐标轴相关

#### D3.js 改变x坐标轴末尾多出来的样式

![](http://ww1.sinaimg.cn/large/6f9f3683gy1g2lwz7kzw4j20ov05174h.jpg)

How would I change this to just a straight line \(no vertical lines on either end\)?

Looking at the generated SVG, this code seems to be determining that style: `<path class="domain" d="M0,6V0H824V6"></path>`, which is auto-generated by D3.

#### 2 Answers

This is controlled by [`axis.outerTickSize()`](https://github.com/mbostock/d3/wiki/SVG-Axes#outerTickSize):

> An outer tick size of 0 suppresses the square ends of the domain path, instead producing a straight line.

All you need to do is set `axis.outerTickSize(0)`.

Lars Kotthoff's answer is still valid for d3 versions prior to 4.x, with version 4 it changed to `axis.tickSizeOuter()`. Note that `tickSize()` modifies the outer ticks as well, which means the order of calling `tickSize()` and `tickSizeOuter()` is important.

```javascript
d3.axisBottom(xScale)
    .tickValues(series)
    .tickSize(5)
    .tickSizeOuter(0)
);
```

### 问题2 V3升级V4的问题

#### D3js V3 to V4 Brush changes

'm looking to migrate from d3v3 to d3v4. In particular i'm having difficulties in migrating brushes.

Can someone please have a look at below link and let me know the changes?[http://bl.ocks.org/zanarmstrong/ddff7cd0b1220bc68a58](http://bl.ocks.org/zanarmstrong/ddff7cd0b1220bc68a58)

Some changes i have identified:

d3.time.format -&gt; d3.timeFormat

d3.time.scale -&gt; d3.scaleTime

Facing issues in migrating:

d3.svg.brush -&gt; d3.brushX

Thanks & Regards,

Naishav

#### 2 Answers

It seems rather strange to abuse the `d3.brush` like that when you can code this so much more straightforward with regular events:

```markup
<!DOCTYPE html>
<html>

<head>
  <style>
      body {
      background-color: #393939;
      font-size: 14px;
      font-family: 'Raleway', sans-serif;
    }

    p {
      color: white;
      margin: 50px;
    }

    a {
      color: #4FDEF2;
    }

    .axis {
      fill: gray;
      -webkit-user-select: none;
      -moz-user-select: none;
      user-select: none;
    }

    .axis .halo {
      stroke: gray;
      stroke-width: 4px;
      stroke-linecap: round;
    }

    .handle path {
      stroke: white;
      stroke-width: 3px;
      stroke-linecap: round;
      pointer-events: none;
    }

    .handle text {
      fill: white;
      text-align: center;
      font
  </style>
</head>

<body>

  <script src="https://d3js.org/d3.v4.min.js"></script>

  <script>
    // parameters
    var margin = {
        top: 50,
        right: 50,
        bottom: 50,
        left: 50
      },
      width = 960 - margin.left - margin.right,
      height = 300 - margin.bottom - margin.top;


    // scale function
    var timeScale = d3.scaleTime()
      .domain([new Date('2012-01-02'), new Date('2013-01-01')])
      .range([0, width])
      .clamp(true);

    var formatDate = d3.timeFormat("%b %d");

    var svg = d3.select("body").append("svg")
      .attr("width", width + margin.left + margin.right)
      .attr("height", height + margin.top + margin.bottom)
      .append("g")
      // classic transform to position g
      .attr("transform", "translate(" + margin.left + "," + margin.top + ")");


    svg.append("rect")
      .style("pointer-events", "all")
      .style("fill", "none")
      .attr("width", width)
      .attr("height", height)
      .style("cursor", "crosshair")
      .on("mousedown", function(){
        updatePos(this);
      })
      .on("mousemove", function(){
        if (d3.event.which === 1){
          updatePos(this);
        }
      });

    function updatePos(elem){
      var xPos = d3.mouse(elem)[0];
      handle.attr('transform', 'translate(' + xPos + ",0)");
      text.text(formatDate(timeScale.invert(xPos)));
    }

    svg.append("g")
      .attr("class", "x axis")
      // put in middle of screen
      .attr("transform", "translate(0," + height / 2 + ")")
      // inroduce axis
      .call(d3.axisBottom()
        .scale(timeScale)
        .tickFormat(function(d) {
          return formatDate(d);
        })
        .tickSize(0)
        .tickPadding(12)
        .tickValues([timeScale.domain()[0], timeScale.domain()[1]]))
      .select(".domain")
      .select(function() {
        console.log(this);
        return this.parentNode.appendChild(this.cloneNode(true));
      })
      .attr("class", "halo");

    var handle = svg.append("g")
      .attr("class", "handle")

    handle.append("path")
      .attr("transform", "translate(0," + height / 2 + ")")
      .attr("d", "M 0 -20 V 20")

    var text = handle.append('text')
      .text(formatDate(timeScale.domain()[0]))
      .attr("transform", "translate(" + (-18) + " ," + (height / 2 - 25) + ")");

  </script>
</body>

</html>
```

#### Quick guide for migrating `d3-brush` from D3 v3 to D3 v4 _\(example for brushX\)_

1. Replace `d3.svg.brush()` with `d3.brushX()`.
2. Rename `brushstart` event to `start`, `brushend` to `end`.
3. Don't pass scale to `.x(xScale)`, this method is now missing. Pass brush borders as `.extent([[xScale.range()[0], 0], [xScale.range()[1], brushHeight]])`.
4. In event handler you can get selection as `d3.event.selection`, for getting selected values use `d3.event.selection.map(xScale.invert)`.
5. To setup selection do `.move(brushContainer, selectedDomain.map(xScale))`. To clear selection do `.move(brushContainer, null)`. Note that this will fire event handlers.
6. `.empty()` method is now missing, use `d3.event.selection === null`.
7. Update your CSS, `.extent` is now `.selection`, `.resize` is `.handle` and became a `rect`instead of `g` containing `rect`.

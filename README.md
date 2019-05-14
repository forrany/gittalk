# Initial page



### 可视化资料&参考

#### 各类型图像

**1. 多条线可交互曲线**

[D3 multi-series line chart with tooltips and legend](http://bl.ocks.org/Matthew-Weber/5645518>)

可以点击选择显示数据的折线图

![](http://ww1.sinaimg.cn/large/6f9f3683gy1g2m2m9rbfij20pk0a6mxx.jpg)

**2. 多组密度管理图**

[Density plot with several groups in d3.js](https://www.d3-graph-gallery.com/graph/density_double.html>)

![](http://ww1.sinaimg.cn/large/6f9f3683gy1g2m2n2ofu7j20zd0fy76z.jpg)

**3. 折线图下的tooltip**

[Line Chart with Circle Tooltip D3 V4](https://bl.ocks.org/alandunning/cfb7dcd7951826b9eacd54f0647f48d3>)

可以显示每个具体数据的ToolTip的实现

![](http://ww1.sinaimg.cn/large/6f9f3683gy1g2m2pu8z3oj20rr0eamxn.jpg)

**4. 折线图的tooltip\(2\)**

[Line Chart with Verticle Tooltip D3](http://bl.ocks.org/wdickerson/raw/64535aff478e8a9fd9d9facccfef8929/>)

另一种风格的toolTip

![1556712650909](C:\Users\vincenttgao\AppData\Roaming\Typora\typora-user-images\1556712650909.png)

**4. 动态交互的饼图**

[Interactive Pie Chart Legend with D3](https://codepen.io/lisaofalltrades/pen/jZyzKo>)

具有较好交互动画的饼状图

![](http://ww1.sinaimg.cn/large/6f9f3683gy1g2m2s38xqjj20kt0fkwfv.jpg)

#### d3的brush

**版本异同**

**V3**

* d3.svg.brush\(\)

  创建brush.默认x.y比例关联。extent 为空

* brush.x\(\[scale\]\)

  获取或设置x关联比例

* brush.y\(\[scale\]\)

  获取或设置y关联比例

* brush.entent\(\[values\]\)

  获取或设置brush的范围 必须先关联比例才生效

* brush.clamp\(\[clamp\]\)

  获取或设置brush的夹选范围

* brush.clear\(\)

  清空范围

* brush.empty\(\)

  当且仅当选择刷的范围为空时，返回true； 当brush被创建时，被初始化为空； 当点击背景而不移动时，或者范围被清除，选择刷会变为空的； 如果选择刷有零宽度或零高度，它将被视为空； 当选择刷为空，则它的范围即视为未定义;

* brush.on\(type\[, listener\]\)

  设置或获取指定类型 type 的监听器 listener ；选择刷支持三种类型事件： brushstart - 鼠标按下时，即mousedown； brush - 鼠标移动时，如果范围在改变，即mousemove； brushend – 鼠标弹起/松开时，即mouseup； 需要注意，当鼠标在背景上点击时也会触发”brush”事件，因为选择刷范围会立刻被清除来开始一段新的范围。

* brush.event\(selection\)

  如果 selection 是选择器，立刻触发brush行为到注册的监听器，即三个事件序列： brushstart, brush 和 brushend； 这是非常有用的，在设置完 brush extent 后来触发相应的事件； 当过渡开始于初始设置范围时触发brushstart ， 过渡进行期间每刻都会触发brush ， 过渡结束时触发brushend ；

**V4**

* d3.brush\(\)

  创建2维的brush.

* d3.brushX\(\) 创建x维的brush.
* brush.brushY\(\) 创建y维的brush.
* brush.extent\(\[values\] \|\| function\)

  创建brush范围； 输出指定格式的function也可。 如果没有指定：\(If extent is not specified）则使用d3的默认函数其返还svg的范围。但是svg要有width、height属性。使用css和viewBox设置SVG宽高时又偷懒会有惊喜

* brush.handlesize\(\[size\]\)

  设置拖选的宽度or高度；默认为6 并不是线宽为6；该属性可能是用于拉选是否容易的目的。

* brush.on\(\)

  调用三个事件： start\end\brush

**Quick guide for migrating d3-brush from D3 v3 to D3 v4 \(example for brushX\)**

1. Replace `d3.svg.brush()` with `d3.brushX()`.
2. Rename `brushstart` event to `start`, `brushend` to `end`.
3. Don't pass scale to `.x(xScale)`, this method is now missing. Pass brush borders as `.extent([[xScale.range()[0], 0], [xScale.range()[1], brushHeight]])`.
4. In event handler you can get selection as `d3.event.selection`, for getting selected values use `d3.event.selection.map(xScale.invert)`.
5. To setup selection do `.move(brushContainer, selectedDomain.map(xScale))`. To clear selection do `.move(brushContainer, null)`. Note that this will fire event handlers.
6. `.empty()` method is now missing, use `d3.event.selection === null`.
7. Update your CSS, `.extent` is now `.selection`, `.resize` is `.handle` and became a `rect`instead of `g` containing `rect`.s

**brush 经典例子与实现**

**1.d3-multiple-brushes**

实现了d3不支持的多个brush，地址：[demo](http://bl.ocks.org/ludwigschubert/0236fa8594c4b02711b2606a8f95f605>)

**2.Multiple No Collision Brushes in D3js**

一种不会重叠的多brush方案实现，地址：[demo](http://bl.ocks.org/jssolichin/54b4995bd68275691a23>)

v4实现的版本，地址： [Multiple Brushes V4 ](https://stackoverflow.com/search?q=d3+brush+change+extent+v4>)

**3. Click to Select All**

修改brush默认点击消失的行为，点击自动选择全部，地址: [demo](https://bl.ocks.org/mbostock/87746f16b83cb9d5371394a001cbd772>)

**4. limit size of brush**

限制brush的可选范围和大小

stackflow解决方案： [stackflow讨论](https://stackoverflow.com/questions/12354729/d3-js-limit-size-of-brush>)

一种实现方案\(在线演示\) ： [demo](http://bl.ocks.org/raffazizzi/3691274>)

**brush控制Q&A**

**1. 通过D3获取元素的计算属性，包括width和height等**

**Q**:

when I create a circle inside the `g`, it will have the radious \(`r`\) set instead of the width. Even if I use the `window.getComputedStyle` method on a `circle`, it will return "auto". How to solve this question.

**A**:

**对于svg元素**，使用`selection.node().getBBox()` 可以得到类似一下的值

```javascript
{
    height: 5, 
    width: 5, 
    y: 50, 
    x: 20
}
```

**对于dom元素**，使用`selection.node().getBoundingClientRect()`

**2. Avoid brush selection reset**

**A** :

点击后选择区域之外，会导致选择区域的重置，如何解决这个问题

**Q** :

As I like to say, everything is possible, it’s just a question of how much work is involved. You have a few options:

* Remove the overlay element from the brush after applying it.

```text
svg.append("g")
    .attr("class", "brush")
    .call(d3.brush().on("brush", brushed))
  .selectAll(".overlay")
    .remove();
```

* Register a listener that takes priority for input events that would initiate a brush gesture, namely _mousedown_ or _touchstart_. See [d3/d3-zoom\#66 \(comment\)](https://github.com/d3/d3-zoom/issues/66#issuecomment-255175895) for a related discussion of event propagation; probably the easiest thing is to use a capturing event listener and then call _event_.stopImmediatePropagation \(or _event_.stopPropagation\) to prevent the brush from receiving the initiating input event.
* Use [_brush_.filter](https://github.com/d3/d3-brush/blob/master/README.md#brush_filter) to tell the brush to ignore input events on the overlay \(using _event_.target to detect the receiving element; see [brush.js](https://github.com/d3/d3-brush/blob/master/src/brush.js#L296)\), so that they won’t start a brush gesture.
* Use a _start_ [brush event listener](https://github.com/d3/d3-brush/blob/master/README.md#brush-events) to set the brush selection to something other than null as desired. See [Brush Snapping II](https://bl.ocks.org/mbostock/6232620) for an example, although note that this doesn’t snap the brush on _start_ \(because it intentionally preserves the normal brush behavior of letting you clear the brush selection by clicking on the background\).


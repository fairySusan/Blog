应该display是大多数人接触前端css学习的第一个属性吧。很多人只知道`display: block`、`display: inline`、`display: inline-block`

由于flex语法的普及，又多了一个 `display: flex`

其实display远没有我们想的那么简单...

### display属性的取值
在MDN上display属性的取值一共有六种：

`display:  [ <display-outside> | <display-inside> ] | <display-listitem> | <display-internal> | <display-box> | <display-legacy>`

这里我们只来解释一下<display-outside> <display-inside> <display-legacy>这三种的取值

### display-outside
这些关键字指定了元素的**外部显示类型**，实际上就是其在**流式布局**中的角色（即在流式布局中的表现）。

取值：
* block 块级元素
* inline 行间元素
* run-in (大多数浏览器不支持，这里不讨论)

### display-inside
这些关键字指定了元素的**内部显示类型**，它们定义了该元素**内部内容**的布局方式（假定该元素为非替换元素 non-replaced element）。

取值：
* flow
指明该元素的子元素布局就是普通的‘流布局’，子元素为块状元素就表现为块状元素，为行间元素就表现为行间元素
* flow-root
这个会让该元素生成一个**块元素框**，这个**块元素框**就是块级格式上下文(BFC),这个**块元素框**会保护里面的子元素不受外面的影响。 这个值跟 display: inline-block有关系，后面再讲
* table
这些元素的行为类似于HTML<table>元素。它定义了一个块级别的盒子。
* flex
该元素的行为类似于块元素，并根据flex模型布局其内容。
* grid
该元素的行为类似于块元素，并根据网格模型布局其内容。

-----------------------------------------------------------
因为CSS 2 对于 display 属性使用单关键字语法，也就是只能有一个值。

当display的值是设置的**外部显示类型**，比如`display: block`时，**内部显示类型**的值默认为flow

当display的值是设置的**内部显示类型**，比如`display: flex`时，**外部显示类型**的值默认设置为block

所以当我们给一个div设置`display: flex`时，它在文档流中依然是块状元素的表现，但是div里面的子元素会变成弹性盒子的布局方式


### display-legacy

前文有说：因为CSS 2 对于 display 属性使用单关键字语法，也就是只能有一个值。所以就有了一些特殊的值来指定，一些特殊的情况
* inline-block
  这个还是比较常用的，它相当于 `display: inline flow-root`，指定这个元素的**外部显示类型**是行间元素，而flow-root会让该元素生成一个**块元素框**
  所以我们设置`display: inline-block`可以让该元素不独占一行，同时又可以设置宽度，高度，因为宽度是设置在**块元素框**上的
* inline-table
  相当于 `display: inline table`
* inline-flex
  相当于 `display: inline flex`
* inline-grid
  相当于 `display: inline grid`

> CSS Level 3 规范详细说明了 display 属性的两类取值——显式地指定了外部和内部显示属性的规范——但是还没有被浏览器广泛支持。
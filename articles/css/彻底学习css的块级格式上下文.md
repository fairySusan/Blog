我们在[深入学习css的display属性](https://github.com/fairySusan/Blog/blob/master/articles/css/深入学习css的display属性.md)里有提到`display:flex-root`，可以让一个元素生成一个**块元素框**，这个**块元素框**就是BFC

BFC(Block Formatting Context)，中文“块级格式上下文”，它是css的隐藏技能。可以用这个技能来解决css的绝对定位、浮动、margin带来的一些问题。

什么样的元素会有BFC？

* <html>元素
* 设置display: flex-root的元素
* 设置display: inline-block的元素
* overflow的值设置为除了visible和clip的其他值的块状元素
* float不为none的元素
* position为absolute和fixed的元素
* 设置display: flex的元素的子元素，并且这个子元素display不能是flex，grid，table
* 设置display: grid的元素的子元素，并且这个子元素display不能是flex，grid，table

BFC特性：
* 具有BFC的元素，内部可以包含浮动子元素。这条特性可以解决子元素浮动，父元素高度塌陷的问题
* 具有BFC的元素，外部的浮动元素不会影响到该元素。这条特性可以解决。相邻元素，一个元素浮动会盖在另一个元素的上面，我们给另一个元素触发BFC，那么浮动元素便不会影响到它
* 具有BFC的元素，元素内的内容不会影响到外面。这条特性可以解决margin重叠的问题



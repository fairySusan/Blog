### 块级元素、内联元素

块级元素：
* 块级元素和‘display：block的元素’不是一个概念，因为display：block ｜ list-item | table | flex 的元素都是独占一行
* 块级元素宽度占满父元素的整个宽度， 因此创建了一个‘块’
* css的width属性默认是auto，意味着块状元素默认宽度是父元素宽度的100%

内联元素：
* 内联元素不能设置css的width、height属性
* 内联元素与内联元素并排的排列显示

### width 和 height

width
* 在普通文档流：块级元素的width默认值为auto，意味着块状元素默认宽度是父元素宽度的100%，所以没有必要再次写下面的代码
```css
div {
  width: 100%
}
```
* width的百分比是相对于父元素的width的，即使父元素的width是默认值auto
* 内联元素设置width无效

height
* height不像width那么复杂，也不分块状元素，内联元素的区别
* height默认值是auto，但是这个auto的意思不同于width，它是靠元素里的内容撑起来的高度，如果元素里没有内容，height则为0
* height设置百分比需要注意，父元素必须设置height，否则height设置百分比无效


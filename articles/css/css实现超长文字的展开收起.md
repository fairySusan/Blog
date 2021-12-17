* 知识点一：由于文字高度不固定，我们使用max-height属性来实现高度的展开动画，只要保证设定的max-height比实际文字高就ok
* 知识点二: 使用<input type="checkbox"/>元素来实现选择和取消选择的功能，省去js的代码
* 知识点三：:checked伪类的使用，来隐藏和显示“收起”、“更多”按钮

```html
<input id="check" type="checkbox">
<p>
  个人觉得，display:table-cell最强的应用是可以任意个数列表的等宽效果
</p>
<div class="element">
  <p>
    display:table-cell其他一些应用，例如，两栏自适应布局，垂直居中效果等等都是可以通过其他技术手段模拟出来的，但是，根据列表个数自动等宽的效果，其他CSS是很难模拟的，尤其当需要兼容IE8浏览器的时候。
  </p>
</div>
<label for="check" class="check-in">更多↓</label>
<label for="check" class="check-out">收起↑</label>
```

```css
input {
  position: absolute;
  clip: rect(0,0,0,0)
}
.check-in {
  dispaly: inline-block;
}
.check-out {
  display: none
}
:checked ~ .check-in {
  display: none
}
:checked ~ .check-out {
  display: inline-block
}
.element {
  max-height: 0;
  overflow: hidden;
  transition: max-height .25s;
}
:checked ~ .element {
  max-height: 666px;
}
```
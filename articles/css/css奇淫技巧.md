#### 实现三条横杠的“更多”图标

重点在于`background-clip: content-box;`让背景颜色只在content部分显示，padding部分不显示。第一条杠是border-top,
第二条杠是`width: 140px;height: 10px;`形成的内容区域,第三条杠是border-bottom

```html
<span class="icon-menu"></span>
```

```css
.icon-menu {
  display: inline-block;
  width: 140px;
  height: 10px;
  padding: 35px 0;
  border-top: 10px solid black;
  border-bottom: 10px solid black;
  background-color: black;
  background-clip: content-box;
}
```

同理可得“双层圆点”效果

```html
<span class="icon-dot"></span>
```

```css
.icon-dot {
  display: inline-block; 
  width: 100px; height: 100px; 
  padding: 10px; 
  border: 10px solid; 
  border-radius: 50%; 
  background-color: currentColor; 
  background-clip: content-box; 
}
```
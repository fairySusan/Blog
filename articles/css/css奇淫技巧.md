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

#### 绘制渐变色字体
```html
<span>Hello World</span>
```

```css
span {
  font-size: 100px;
  background-image: linear-gradient(to bottom, orange, blue);
  background-clip: text; // 重点
 -webkit-text-fill-color:transparent // 重点
}
```

#### 绘制流光溢彩的颜色流动效果
```html
<span>Hello World</span>
```

```css
span {
  font-size: 100px;
  background-image: -webkit-linear-gradient(bottom, #3498db, #f47920 10%, #d71345 20%, #f7acbc 30%, #ffd400 40%, #3498db 50%, #f47920 60%, #d71345 70%, #f7acbc 80%, #ffd400 90%, #3498db);
  background-clip: text; // 重点
 -webkit-text-fill-color:transparent // 重点
  animation: textColorAnimat 2s linear infinite;
  background-size: 100% 200%;
}
@keyframes textColorAnimat {
  0% {
    background-position: 0 0; 
  }
  100% {
    background-position: 0 -100%;
  }
}

```


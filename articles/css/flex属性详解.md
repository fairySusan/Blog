flex属性是flex-grow，flex-shrink，flex-basis的缩写，而flex-grow的默认值是0，flex-shrink的默认值是1，flex-basis的默认值是auto，也就是`flex:initial` 就是 `flex: 0 1 auto`

### flex-grow
flex-grow是指在父元素空间还有多余的情况下，元素该如何的增大。它是一个增长系数，渲染引擎会去获取所有子项的flex-grow的值，然后按比例将剩余空间分配给各个子项。若每个子项的flex-grow的值相同那么每个子项分配的剩余空间一样。
flex-grow初始值为0。 缺省值为1

### flex-shrink
flex-shrink是指在父元素空间不足的情况下，元素该如何缩小。初始值为1。缺省值为1

### flex-basis
flex-basis指定子项在主轴上的初始值大小。这个值也是flex容器来判断有多余空间还是刚刚好还是空间不足的依据。初始值是auto，缺省值是0%，写为0时必须加上单位

flex-basis: 0; 指定元素尺寸由内容撑起来。

当一个元素同时被设置了 flex-basis (除值为 auto 外) 和 width (或者在 flex-direction: column 情况下设置了height) , flex-basis 具有更高的优先级.

百分数表示: 该值也可以是一个相对于其父弹性盒容器主轴尺寸的百分数

### flex
一个值的写法：比如 `flex: 1`，此时的1设置的是flex-grow，对应的补全写法是 `flex: 1 1 0%`
双值语法：比如 `flex: 1 0`，此时1是flex-grow，0是flex-shrink，对应的补全写法是 `flex: 1 0 0%`

* `flex: initial => flex: 0 1 auto` 有剩余空间时不增大，空间不足时缩小
* `flex: auto => flex: 1 1 auto` 有剩余空间时增大，空间不足时缩小
* `flex: none => flex: 0 0 auto` 既不增大也不收缩
* `flex: 1 => flex: 1 1 0%`

大多数情况下，开发者只需要将flex设置为以下值之一即可：auto、initial、none、单值写法

### flex|块状化|包裹性
* 设置了`display:flex`的元素，会块状化，宽度跟父元素宽度一致,不具有包裹性
* 设置了`display:flex; flex-direction: row;`的元素的子元素会块状化，具有包裹性
* 设置了`display:flex; flex-direction: column;`的元素的子元素会块状化，宽度跟父元素宽度一致,不具有包裹性

### 思考 flex: 1填满剩余空间的原理？
### 思考 flex: 1填满剩余空间的原理，但是需要设置height: 0; 元素才有高度？
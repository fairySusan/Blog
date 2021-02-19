`flex: none | auto | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]`

像上面这种css语法被称为格式化语法，CSS语法中的特殊符号的含义绝大多数就是正则表达式中的含义。

首先说一下单管道符 | : 表示排他。也就是这个符号前后的属性值都是支持的，且不能同时出现。因此下面这些语法都是支持的：
```
flex: auto;
flex: none;

flex: [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
```
再看 []: 表示范围
`flex: [ <'flex-grow'> <'flex-shrink'> <'flex-basis'> ]`
flex 支持空格分隔的3个值

在看 ? : 表示<'flex-shrink'>可以1个或0个

最后 || : 表示可以 `flex:  [<'flex-grow'> <'flex-shrink'>]` 或者 `flex: <'flex-basis'>`
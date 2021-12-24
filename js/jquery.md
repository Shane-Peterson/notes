# jQuery

![jQuery](images/jquery.jpg)

jQuery 是一个使用相当广泛得 JavaScript 函数库。

## 获取元素

选择表达式为 CSS选择器：

```JavaScript
$(document) // 选择整个文档对象
$('#myId') // 选择ID为myId的网页元素
$('div.myClass') // 选择class为myClass的div元素
$('input[name=first]') // 选择name属性等于first的input元素
```

也可以是 jQuery 特有的：

```javascript
$('a:first') // 选择网页中第一个a元素
$('tr:odd') // 选择表格的奇数行
$('#myForm :input') // 选择表单中的input元素
$('div:visible') // 选择可见的div元素
$('div:gt(2)') // 选择所有的div元素，除了前三个
$('div:animated') // 选择当前处于动画状态的div元素
```

## 链式操作

选中网页元素以后，可以对它进行一系列操作，并且所有操作可以连接在一起，以链条的形式写出来，比如：

```javascript
$('div').find('h3').eq(2).html('Hello')
```

另一种写法：

```javascript
$('div')
  .find('h3')
  .eq(2)
  .html('Hello')
  .end() // 退回到选中所有的 h3 元素的那一步
  .eq(0) // 选中第一个h3元素
  .html('World') // 将它的内容改为World
```

## 创建元素

把要创建的元素传入 jQuery 的构造函数，即可创建新元素。

```javascript
$('<p>Hello</p>')
$('<li class="new">new list item</li>')
$('ul').append('<li>list item</li>')
```

## 移动元素

在目标元素后面插入集合中每个匹配的元素。

```jsx
$('div').insertAfter($('p')) 
```

在匹配元素集合中的每个元素后面插入参数所指定的内容，作为兄弟节点。

```javascript
$('p').after($('div'))
```

两种方法之间最大的区别就是返回的元素不一样。第一种方法返回div元素，第二种方法返回p元素。

使用这种模式的操作方法，一共有四对：

```text
.insertAfter()
.after()
```

```text
.insertBefore()
.before()
```

```text
.appendTo()
.append()
```

```text
.prependTo()
.prepend()
```

## 修改元素的属性

DOM属性（获取或设置页面元素的 DOM 属性）

为每个匹配的元素添加指定的样式类名。

```text
.addClass(className)
```

获取匹配的元素集合中的第一个元素的属性值，或设置每个匹配元素的一个或多个属性。

```text
.attr()
.prop()
```

`.attr()` 方法返回 attributes 的值，而 `prop` 方法返回 property 的值。

获取集合中第一个匹配元素的 html 内容，或设置每一个匹配元素的 html 内容。

```text
.html
```

为匹配的元素集合中的每个元素中移除一个属性。

```text
.removeAttr()
```

移除集合中每个匹配元素上一个、多个或全部样式。

```text
.removeAttr([className])
```

为集合中匹配的元素删除一个属性（property)。

```text
.removeProp(propertyName)
```

获取匹配的元素集合中第一个元素的当前值或设置匹配的元素集合中每一个元素的值。

```text
.val()
```
---

CSS 属性（设置或获取元素的 CSS 相关的属性）

获取匹配元素集合中的第一个元素的样式属性的值，或设置每个匹配元素的一个或多个 CSS 属性。

```text
.css()
```

在匹配的元素集合中获取第一个元素的的当前计算宽度或给每个匹配的元素设置宽度。

```text
.width()
```

获取匹配元素集合中的第一个元素的当前计算高度值，或设置每一个匹配元素的高度值。

```text
.height()
```

在匹配的元素集合中，获取第一个元素的当前坐标，或设置每一个元素的坐标，坐标相对于文档。

```text
.offset()
```

获取匹配元素中第一个元素中的当前坐标，相对于 offset parent 的坐标（offset parent 指离该元素最近的而且被定位过的祖先元素）

```text
.position()
```
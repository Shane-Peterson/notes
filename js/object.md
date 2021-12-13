# JavaScript 对象基本用法

![Object](images/object.png)

ECMA-262 把对象定义为：“无序属性的集合，其属性可以包含基本值、对象或者函数。”

## 声明对象的两种语法

第一种是使用 new 操作符后跟 Object 构造函数，如下所示：

```JavaScript
let person = new Object();
```

另一种方式是使用对象字面量表示法。对象字面量是对象定义的一种简写方式，目的在于简化创建包含大量属性的对象的过程。

```JavaScript
let person = {}
```

## 如何删除对象的属性

使用 `delete` 操作符可以删除对象的某个属性。

```JavaScript
delete obj.xxx 或 delete obj['xxx']
```

## 如何访问对象的属性

一般来说，访问对象属性时使用的都是点表示法，这也是很多面向对象语言中通用的语法。不过，在 JavaScript 也可以使用方括号表示法来访问对象的属性。在使用方括号语法时，应该将要访问的属性以字符串的形式放在方括号中，如下面的例子所示。

```JavaScript
alert(person["name"]);
alert(person.name);
```

从功能上看这两种访问对象属性的方法没有任何区别。但方括号语法的主要优点是可以通过变量来访问属性，例如：

```JavaScript
let propertyName = "name";
alert(person[propertyName]);
```

## 如何修改或增加对象的属性

修改和增加对象属性是一回事，当属性存在时，修改；当属性不存时，则增加。

直接赋值：

```JavaScript
let obj = {name: 'shane'} //name 是字符串
obj.name = 'shane' //name 是字符串
obj['name'] = 'shane'
obj['na' + 'me'] = 'shane'
let key = 'name'; obj[key] = 'shane'
```

批量赋值：

```JavaScript
Object.assign(obj, {age: 18, gender: 'man'})
```

## in 操作符

`in` 操作符会在通过对象能够访问给定属性时返回 `true`，无论该属性存在于实例中还是原型中。

## hasOwnProperty() 方法

`hasOwnProperty()` 方法会在属性存在于实例中时才返回 `true`。

## 检测一个属性是否存在于原型中

通过同时使用 `hasOwnProperty()` 方法和 `in` 操作符，就可以确定该属性到底是存在与对象中，还是存在于原型中，如下所示。

```JavaScript
function hasPrototypeProperty(object, name) {
	return !object.hasOwnProperty(name) && (name in object);
}
```

## 修改原型（原型式继承）

```JavaScript
let obj = Object.create(common)
```

`Object.create()` 在传入一个参数的情况下，与如下代码相同。

```JavaScript
function object(o) {
	function F() {}
	F.prototype = o;
	return new F();
}
```
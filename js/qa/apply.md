![apply](../images/qa.jpg)

# apply、call、bind 的用法分别是什么？

每个函数都包含两个继承而来的方法：`apply()` 和 `call()`。这两个方法的用途都是在特定的作用域中调用函数，实际上等于设置函数体内的 this 对象的值。

## apply

`apply()` 方法接收两个参数：一个是在其中运行函数的作用域，另一个是参数数组。其中，第二个参数可以是 Array 的实例，也可以是 arguments 对象。例如：

使用方法：

```jsx
function sum(num1, num2) {
  return num1 + num2;
}

function callSum1(num1, num2) {
  return sum.apply(this, arguments); // 传入 arguments 对象
}

function callSum2(num1, num2) {
  return sum.apply(this, [num1, num2]); // 传入数组
}

console.log(callSum1(10, 10)); // 20
console.log(callSum2(10, 10)); // 20
```

在上面这个例子中，`callSum1()` 在执行 sum 函数时传入了 this 作为 this值（因为是在全局作用域中调用的，所以传入的就是 window 对象）和 arguments 对象。

而 callSum2 同样也调用了 `sum()` 函数，但它传入的则是 this 和一个参数数组。这两个函数都会正常执行中并返回正确的结果

<aside>
💡 在严格模式下，未指定环境对象而调用函数，则 this 值不会转型为 window。除非明确把函数添加到某个对象或者调用 apply() 或 call()，否则 this 值将是 undefined。

</aside>

原理：

```jsx
Function.prototype.apply = function (context) {
  context = context ? Object(context) : window
  context.fn = this
  let args = [...arguments][1]
  if (!args) {
    return context.fn()
  }
  let result = context.fn(...args)
  delete context.fn;
  return result
}
```

## call

`call()` 方法与 `apply()` 方法的作用相同，它们的区别仅在于接收参数的方式不同。对于 `call()` 方法而言，第一个参数是 this 值没有变化，变化的是其余参数都直接传递给函数。换句话说，在使用 `call()`
方法时，传递给函数的参数必须逐个列举出来，如下面的例子所示。

使用方法：

```jsx
function sum(num1, num2) {
  return num1 + num2;
}

function callSum(num1, num2) {
  return sum.call(this, num1, num2)
}

console.log(callSum(10, 10)) // 20
```

在使用 call() 方法的情况下，`callSum()` 必须明确的传入每一个参数。结果于使用 apply() 没有什么不同。至于是使用 `apply()` 还是 `call()`，完全取决于你采取那种给函数传递参数的方式最方便。

---

事实上，传递参数并非 apply() 和 call() 真正的用武之地；它们真正强大地方是能够扩充函数赖以运行的作用域。比如

```jsx
window.color = "red";
let o = {color: "blue"};

function sayColor() {
  console.log(this.color)
}

sayColor(); // red
sayColor.call(this); // red
sayColor.call(window); // red
sayColor.call(o); // blue
```

上面的代码中，`sayColor()` 作为全局函数定义的，而且当在全局作用域中调用它时，它确实会显示`"red"`——因为对 `this.color` 的求值会转换成对 `window.color` 的求值。

而 `sayColor.call(this)` 和 `sayColor.call(window)`，则是两种显式的在全局作用域中调用函数的方式。但是，当运行 `sayColor.call(o)`
时，函数的执行环境就不一样了，因为此时函数体内的 this 对象指向了 o，于是结果显示的是 `"blue"`。

使用 `call()` 或（`apply()`）来扩充作用域的最大好处，就是对象不需要与方法有任何耦合关系。

---

原理：

```jsx
Function.prototype.call = function (context) {
  context = context ? Object(context) : window
  context.fn = this
  let args = [...arguments].slice(1)
  let result = context.fn(...args)
  delete context.fn
  return result
}
```

## bind

函数绑定会创建一个新函数，新函数可以在特定的 this 环境中以指定参数调用另一个函数。

使用方法：

```jsx
const module = {
  x: 42,
  getX: function () {
    return this.x;
  }
};

const unboundGetX = module.getX;
console.log(unboundGetX()); // The function gets invoked at the global scope
// expected output: undefined

const boundGetX = unboundGetX.bind(module);
console.log(boundGetX());
// expected output: 42
```

原理：

一个简单的 `bind()` 函数接受一个函数和一个坏境，并返回一个在给定环境中调用给定函数的函数，并且将所有参数原封不动传递过去，

```jsx
function bind(fn, context) {
  return function () {
    return fn.apply(context, arguments);
  }
};
```

缺点：

被绑定函数与普通函数相比有更多的开销，它们需要更多的内存，同时也因为多重函数调用稍微慢一点。

## 函数的柯里化

函数柯里化（function currying）用于创建已经设置好了一个或多个参数的函数。

使用方法：

```jsx
let handler = {
  message: "Event handled",
  handleClick: function (name, event) {
    console.log(this.message + ":" + name + ":" + event.type);
  }
};

let btn = document.getElementById("my-btn");
btn.addEventListener("click", handler.handleClick.bind(handler, "my-btn"), false);

```

原理：

函数柯里化和函数绑定是一样的：使用一个闭包返回一个函数。两者的区别在于，当函数被调用时，返回的函数还需要设置一些传入的参数。

```jsx
function bind(fn, context) {
  let args = Array.prototype.slice.call(arguments, 2);
  return function () {
    let innerArgs = Array.prototype.slice.call(arguments);
    let finalArgs = args.concat(innerArgs);
    return fn.apply(context, finalArgs);
  }
}
```

缺点：

柯里化函数和绑定函数提供了强大的动态函数创建供功能。它们都能用于创建复杂的算法和功能，当然两者都不应滥用，因为每个函数都会带来额外的开销。
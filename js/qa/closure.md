![closure](../images/qa.jpg)

# 什么是闭包？

闭包是基于词法作用域书写代码时所产生的自然结果，当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是在当前词法作用域之外执行。换句话说，如果一个函数用到了外部的变量，那么这个函数加这个变量就叫做闭包（有权访问另一个函数作用域中的变量的函数）。创建闭包的常见方式就是在一个函数内部创建另一个函数。

在定时器、事件监听器、Ajax请求、跨窗口通信、Web Workers 或任何其他的异步（或者同步）任务中，只要使用了回调函数，实际上就是在使用闭包。

## 闭包的用途是什么？

既然闭包无处不在，那它的用途自然不胜枚举，所有用到闭包的地方都可以说是它的用途。比如用来实现模块：

```javascript
function CoolModule() {
  var something = "cool";
  var another = [1, 2, 3];

  function doSomething() {
    console.log(something);
  }

  function doAnother() {
    console.log(another.join("!"))
  }

  return {
    doSomething: doSomething,
    doAnother: doAnother
  };
}

var foo = CoolModule();
foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

闭包的形成和垃圾回收机制有关，一般来说，当函数执行完毕后， 局部活动对象就会被垃圾回收，内存中仅保存全局作用域，闭包的情况又有所不同。上面的代码中 CoolModule()
调用后会返回一个包含内部函数的对象，形成对内部函数的引用，导致局部活动对象无法被垃圾回收，闭包因此产生。

```jsx
foo = null // 解除对内部函数的引用，以便释放内存
```

## 闭包的缺点是什么？

第一条：由于闭包会携带包含他的函数的作用域，由此会比其他函数占用更多的内存。过度使用闭包可能会导致内存占用过多。

第二条：闭包只能取得包含函数中任何变量的最后一个。比如

```javascript
function createFunctions() {
  const result = new Array()
  for (var i = 0; i < 10; i++) {
    result[i] = function () {
      return i
    }
  }
  return result
}

createFunctions()[1]() // 10
```

这个函数会返回一个函数数组。表面上看，似乎每个函数都应该返回自己的索引值，即位置 0 的函数返回 0，位置 1 的函数返回 1，以此类推。但实际上，每个函数都返回 10.
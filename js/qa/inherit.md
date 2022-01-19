![inherit](../images/qa.jpg)

# 你如何理解 JS 的继承？

JS 的继承可分为两种形式，第一种为ES5 写法基于原型链的继承，其基本思想是利用原型让一个引用类型继承另一个引用类型。

我们知道每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。那么，假如我们让原型对象等于另一个类型的实例，结果会怎样呢？显然，此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着一个指向另一个构造函数的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实例与原型的链条。

基于原型链的几种继承模式中，属寄生组合式继承最为高效率，其大致代码如下。

```jsx
function inheritPrototype(subType, superType) {
  let prototype = Object.create(superType.prototype); // 创建对象
  prototype.constructor = subType; // 增强对象
  subType.prototype = prototype; // 指定对象
}

function SuperType(name) {
  this.name = name;
  this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function () {
  console.log(this.name);
};

function SubType(name, age) {
  SuperType.call(this, name);
  this.age = age;
}

inheritPrototype(SubType, SuperType);
SubType.prototype.sayAge = function () {
  console.log(this.age);
};
```

另一种为ES6 写法通过 extends 关键字实现继承，这比 ES5 通过修改原型链实现继承更加清晰和方便，例如

```jsx
class SuperType {
  constructor(name) {
    this.name = name;
    this.colors = ["red", "blue", "green"];
  }

  sayName() {
    return this.name;
  }
}

class SubType extends SuperType {
  constructor(name, age) {
    super(name); // 调用父类的 constructor(name)
    this.age = age;
  }

  sayAge() {
    return super.sayName() + ' ' + this.age;
    // 调用父类的 sayName()
  }
}
```
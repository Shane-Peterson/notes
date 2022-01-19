![promise](../images/qa.jpg)

# 对 Promise 的了解

## Promise 的用途

所谓 Promise，简单来说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）。有了 Promise 对象可以将异步操作以同步的流程表达出来，避免了层层嵌套的回调函数。此外，Promise
对象提供统一的接口，使得控制异步操作更加容易。

## 如何创建一个 new Promise

```jsx
let promise = new Promise(function (resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */) {
    resolve(value)
  } else {
    reject(error)
  }
});
```

Promise 构造函数接受一个函数作为参数，该函数的两个参数分别是 resolve 和 reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

resolve 函数的作用是，将 Promise 对象的状态从“未完成”变为“成功”（即从 Pending 变为 Resolved），在异步操作成功时调用，并将异步操作的结果作为参数传递出去；reject 函数的作用是，将 Promise
对象的状态从“未完成”变为“失败”（即为 Pending 变为 Rejected），在异步操作失败时调用，并将异步操作报出的错误作为参数传递出去）。

## 如何使用 Promise.prototype.then

Promise 实例具有 then 方法，即 then 方法是定义在原型对象 Promise.prototype 上的。它的作用是为 Promise 实例添加状态改变时的回调函数。

```jsx
promise.then(
  function (value) {
    // success
  },
  function (error) {
    // failure
  }
);
```

## 如何使用 Promise.all

Promise.all 方法用于将多个 Promise 实例包装成一个新的 Promise 实例。

```jsx
let p = Promise.all([p1, p2, p3,])
```

p 的状态由 p1、p2、p3 决定，分成两种情况。

1. 只有 p1、p2、p3 的状态都变成 Fulfilled，p 的状态才会变成 Fulfilled，此时 p1、p2、p3 的返回值组成一个数组，传递给 p 的回调函数。
2. 只要 p1、p2、p3 中有一个被 Rejected，p 的状态就变成 Rejected，此时第一个被 Rejected 的实例的返回值会传递给 p 的回调函数。

## 如何使用 Promise.race

Promise.race() 方法同样是将多个 Promise 实例包装成一个新的 Promise 实例。

```jsx
let p = Promise.race([p1, p2, p3])
```

上面的代码中，只要 p1、p2、p3 中有一个实例率先改变状态，p 的状态就跟着改变。那个率先改变的 Promise 实例的返回值就传递给 p 的回调函数。
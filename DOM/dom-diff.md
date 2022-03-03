# 虚拟 DOM 和 DOM diff

![DOM diff](images/dom-diff.jpg)

## 虚拟 DOM

虚拟 DOM 是一个代表 DOM 树的对象，通常含有标签名、标签上的属性、事件监听和子元素们，以及其他属性。

## 虚拟 DOM 的优点

第一条：减少操作频率，虚拟 DOM 可以将多次操作合并为一次操作，比如你添加 1000 个节点，DOM 可能需要操作 1000 次，但虚拟 DOM 只需要操作 1 次。

第二条：缩小操作范围，虚拟 DOM 借助 DOM diff 可以把多余的操作省掉，比如你添加 1000 个节点，其实只有 10 个是新增的。

第三条：跨平台，虚拟 DOM 不仅可以变成 DOM，还可以变成小程序、iOS 应用、安卓应用，因为虚拟 DOM 本质上只是一个 JS 对象。就像下面这样：

```jsx
const vNode = {
  key: null,
  props: {
    children: [ 
      { type: 'span', ... },
      { type: 'span', ... }
    ]
    className: "red" 
    onClick: () => {} 
  },
  ref: null,
  type: "div",
  ...
}
```

```jsx
const vNode = {
  tag: "div", 
  data: {
    class: "red", 
    on: {
      click: () => {}
    }
  }，
  children: [
    { tag: "span", ... },
    { tag: "span", ... }
  ],
  ...
}
```

## 虚拟 DOM 的缺点

需要额外的创建函数，如 createElement 或 h，但可以通过 JSX 或其他方式来简化成 XML 写法。而使用 JSX 或 .vue 文件的缺点就是需要添加额外的构建过程。

## DOM diff 是什么

DOM diff 就是一个函数，我们称之为 patch。

```jsx
Patches = patch(oldVNode, newVNode);
```

patches 就是要运行的 DOM 操作，可能长这样：

```jsx
[
	{type: 'INSERT', vNode: ... },
	{type: 'TEXT', vNode: ... },
	{type: 'PROPS', propsPatch: [...]}
]
```

## DOM diff 可能的大概逻辑

**Tree diff**

将新旧两棵树逐层对比，找出哪些节点需要更新。如果节点是组件就看 Component diff，如果节点是标签就看 Element diff。

**Component diff**

如果节点是组件，就先看组件类型，类型不同直接替换（删除旧的）；类型相同则只更新属性，然后深入组件做 Tree diff（递归）。

**Element diff**

如果节点是原生标签，则看标签名，标签名不同直接替换，相同则只更新属性，然后进入标签后代做 Tree diff（递归）。

## DOM diff 的优点

排除多余的 DOM 操作，提升页面渲染效率。

## DOM diff 的问题

第一条：同级节点对比存在 bug，会出现识别错误的问题。比如 DOM diff 中的 key 问题，这个问题在 React 中也存在。

破解方法就是给他一个唯一的 id，这个 id 可以是随机数、字符串、数字……

第二条：让我们看一个小的 React 代码示例

```jsx
function WelcomeMessage(props){
  return (
    <div className="welcome">
      Welcome {props.name}
    </div>
  )
}
```

假设 props.name 的初始值是"Tom"，后来改为"Jerry"。整个代码中唯一的变化是 prop，不需要更改 DOM 节点或比较标签内的属性，但是，使用 DOM diff，有必要完成所有步骤来识别更改。

在开发过程中，我们可以看到大量这样的细微变化，比较UI中的每个元素无疑是一个开销。


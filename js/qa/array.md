![array](../images/qa.jpg)

# 如何实现数组去重？

假设有数组 array = [1,5,2,3,4,2,3,1,3,4]，请写一个函数 unique，使得 unique(array) 的值为 [1,5,2,3,4]，也就是把重复的值都去掉，只保留不重复的值。

## 使用 Set

```jsx
array = [1, 5, 2, 3, 4, 2, 3, 1, 3, 4]

function computationalComplexity() {
  console.time("answer time");

  // code
  function unique(arr) {
    return [...new Set(arr)]
  }

  console.log(unique(array)) // [1, 5, 2, 3, 4]
  console.timeEnd("answer time");
}

computationalComplexity() 
```

缺点：不支持对象去重，并且可能也许大概会有兼容性问题，比如不支持 IE6。

## 不使用 Set

```jsx
array = [1, 5, 2, 3, 4, 2, 3, 1, 3, 4]

function computationalComplexity() {
  console.time("answer time");

  // code
  function unique(a) {
    return array.filter((element, index, array) => {
      return array.indexOf(element) === index;
    });
  }

  console.log(unique(array)); // [1, 5, 2, 3, 4]
  console.timeEnd("answer time");
}

computationalComplexity()
```

缺点：不支持对象去重，并且因为 indexOf 底层使用 === 来进行判断，而 NaN === NaN 结果是 false，因此使用 indexOf 查不到 NaN 这个元素，也就是说返回的数组中永远不会包含 NaN。

## 使用 Map/WeakMap 以支持对象去重。

```jsx
array = [1, 5, 2, 3, 4, 2, 3, 1, 3, 4,
  [1, 2, 3],
  [4, 5, 6],
  [1, 2, 3],
];

function computationalComplexity() {
  console.time("answer time");

  // code
  function unique(arr, key) {
    return [...new Map(arr.map((x) => [key(x), x])).values()];
  }

  b = unique(array, JSON.stringify);
  console.log(b); // [1, 5, 2, 3, 4, [1,2,3], [4,5,6]]
  console.timeEnd("answer time");
}

computationalComplexity()
```

缺点：无法对 undefined、function、Symbol()、Bigint() 去重，可以说 JSON 不支持的数据类型都无法去重（由于不支持的数据类型序列化后都会返回 undefined，而 Map 的键支持任何数据类型，又由于向
Map 中追加多个相同的键时，它只保留最后一个。因此返回的数组中会包含一个也是最后一个添加到 Map 中的键为 undefined 的值）。

![sort](../images/qa.jpg)

# 数据排序

给出正整数数组 array = [2,1,5,3,8,4,9,5]。请写出一个函数 sort，使得 sort(array) 得到从小到大排好序的数组[1,2,3,4,5,5,8,9]，新的数组可以是在 array 自身上改的，也可以是完全新开辟的内存。不得使用 JS 内置的 sort API。

```jsx
function sort(array) {
  for (let i = 0; i < array.length - 1; i++) {
    for (let j = 0; j < array.length - 1 - i; j++) {
      if (array[j] > array[j + 1]) {
        [array[j], array[j + 1]] = [array[j + 1], array[j]];
      }
    }
  }
  return array;
}

console.log(sort(array)) // [1, 2, 3, 4, 5, 5, 8, 9]
```
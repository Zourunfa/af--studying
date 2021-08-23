## 事件坐标：screenX,clientX,pageX,offsetX的区别

![image-20210311103502730](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210311103502730.png)

**e.screenX/e.screenY:**
事件发生时鼠标相对于电脑屏幕的坐标。e.screenX,e.screenY的最大值不会超过屏幕分辨率。

**e.clientX/e.clientY:**
事件发生时鼠标在浏览器内容区域的坐标。
浏览器内容区域就是浏览器窗口中用来显示网页的可视区域，不包括滚动条和工具栏，也不随滚动条的移动而移动。
当浏览器窗口缩小时，e.clientX/e.clientY的最大值也会缩小。

**e.pageX/e.pageY:**
事件发生时鼠标相对于整个页面的坐标。整个页面是说整个网页的区域。

**e.offsetX、e.offsetY:**
事件发生时鼠标相对于事件源元素的坐标，事件源元素就是绑定事件的那个元素。





由最开始我们总结的 `flat` 特性知道，`flat` 函数执行是会跳过空位的。ES5 大多数数组方法对空位的处理都会选择跳过空位包括：`forEach()`, `filter()`, `reduce()`, `every()` 和 `some()` 都会跳过空位。

```js
// reduce + 递归
Array.prototype.fakeFlat = function(num = 1) {
  if (!Number(num) || Number(num) < 0) {
    return this;
  }
  let arr = [].concat(this);
  return num > 0
    ? arr.reduce(
        (pre, cur) =>
          pre.concat(Array.isArray(cur) ? cur.fakeFlat(--num) : cur),
        []
      )
    : arr.slice();
};
const arr = [1, [3, 4], , ,];
arr.fakeFlat()
// [1, 3, 4]

// foEach + 递归
Array.prototype.fakeFlat = function(num = 1) {
  if (!Number(num) || Number(num) < 0) {
    return this;
  }
  let arr = [];
  this.forEach(item => {
    if (Array.isArray(item)) {
      arr = arr.concat(item.fakeFlat(--num));
    } else {
      arr.push(item);
    }
  });
  return arr;
};
const arr = [1, [3, 4], , ,];
arr.fakeFlat()
// [1, 3, 4]
```

## 由于空位的处理规则非常不统一，所以建议避免出现空位。

**ES5 对空位的处理，就非常不一致，大多数情况下会忽略空位。**

- `forEach()`, `filter()`, `reduce()`, `every()` 和`some()` 都会跳过空位。
- `map()` 会跳过空位，但会保留这个值。
- `join()` 和 `toString()` 会将空位视为 `undefined`，而`undefined` 和 `null` 会被处理成空字符串。

**ES6 明确将空位转为 `undefined`。**

- `entries()`、`keys()`、`values()`、`find()` 和 `findIndex()` 会将空位处理成 `undefined`。
- `for...of` 循环会遍历空位。
- `fill()` 会将空位视为正常的数组位置。
- `copyWithin()` 会连空位一起拷贝。
- 扩展运算符（`...`）也会将空位转为 `undefined`。
- `Array.from` 方法会将数组的空位，转为 `undefined`。



## async await语法

- 执行async函数，返回的是Promise对象
- await相当于Promise的then
- try..catch可捕获异常但是只能捕获同步的异常


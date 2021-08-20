# forEach与map

### map

map() 方法创建一个新数组，其结果是该数组中的每个元素是调用一次提供的函数后的返回值

**语法**：

```
var new_array = arr.map(function callback(currentValue[, index[, array]]) { Return element for new_array }[, thisArg])
```

**参数**:

callback 函数会被自动传入三个参数：数组元素，元素索引，原数组本身

- 因为map生成一个新数组，当你不打算使用返回的新数组却使用map是违背设计初衷的
- map 不修改调用它的原数组本身
- 如果 thisArg 参数提供给map，则会被用作回调函数的this值。否则undefined会被用作回调函数的this值，此时this指向window

```javascript
let arr = [1 ,2 ,3];
let newArr = arr.map((item, index, arr) => {
    return item * 2
})
console.log(newArr) //[ 2, 4, 6 ]
```

### forEach

forEach() 方法对数组的每个元素执行一次给定的函数。

**语法**:

```
rr.forEach(callback(currentValue [, index [, array]])[, thisArg])
```

**参数**: 可依次向 callback 函数传入三个参数

- 数组当前项的值
- 数组当前项的索引
- 数组对象本身
- 如果 thisArg 参数有值，则每次 callback 函数被调用时，this 都会指向 thisArg 参数。如果省略了 thisArg 参数，或者其值为 null 或 undefined，this 则指向全局对象
- forEach() 为每个数组元素执行一次 callback 函数；与 map() 或者 reduce() 不同的是，它总是返回 undefined 值，并且不可链式调用
- forEach() 被调用时，不会改变原数组，也就是调用它的数组

```javascript
let person = ['张三', '李四', '王五'];
person.forEach((item, index, arr) => {
    console.log(item);
}) // 张三  李四  王五
```

### 二者区别

- map()会分配内存空间存储新数组并返回，forEach()不会返回数据。
- forEach()允许callback更改原始数组的元素。map()返回新的数组。
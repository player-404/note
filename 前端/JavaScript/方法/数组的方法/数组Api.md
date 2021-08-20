# 数组Api

### 1. Array.from

将两类对象转为真正的数组：类似数组的对象和可遍历`（iterable）`的对象（包括 `ES6` 新增的数据结构 `Set` 和 `Map`）



### 2. Array.of

用于将一组值，转换为数组

```javascript
let a = Array.of(1,'ds',4);
console.log(a) //[ 1, 'ds', 4 ]
```



### 3. copyWithin

将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组

参数如下：

- target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
- start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示从末尾开始计算。
- end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示从末尾开始计算

```javascript
let array = [1,2,3,4,5,6,7];
let newA = array.copyWithin(0, 4, 5);
console.log(newA) //[5, 2, 3, 4, 5, 6, 7]
```



### 4.findIndex & find

findIndex: 找出符合条件的index

```javascript
1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```

Find: 找出符合条件的值

```javascript
[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
}) // 10

```



### 5. includes()

用于判断数组是否包含给定的值

```javascript
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true
```



### 6.fill()

使用给定值，填充一个数组



### 7.flat()，flatMap()

将数组扁平化处理，返回一个新数组，对原数据没有影响



将`sort()`默认设置为稳定的排序算法
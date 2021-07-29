# instanceOf

instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上

**语法**:

```
object instanceof constructor
```

**参数**:

- object 某个实例对象
- constructor 某个构造函数

```javascript
const Person = function () {
    this.name = '张三'
}
let p = new Person();
console.log(p instanceof Person); //true
```

手写：

原理：实例的原型链上一级一级查找是否有该构造函数的原型，有返回true 没有返回false

```javascript
function newInstanceOf(left, right) {
    let leftPro = left.__proto__;
    while (true) {      //while 循环直至return
        if (!leftPro) return false;
        let rightPro = right.prototype;
        if (leftPro === rightPro) {
            return  true;
        }
        leftPro = leftPro.__proto__; // 重新赋值为下一级的__proto__   有点递归的意思
    }
}
```
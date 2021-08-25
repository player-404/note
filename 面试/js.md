# js面试题

### 1.填写log输出

```javascript
function test() {
    let a = b = 0;
}
test();
console.log(b); // 0
console.log(a);// not defined
```

* `let a` 产生块级作用域，无法在作用于之外获取 故 not defined

* `b = 0` 不带生命关键字，则默认使用var，声明在全局，故 可以访问值为0



### 2.填写log输出

```javascript
let str = false + 1;
let str1 = true + 1;
console.log(str, str1);
```

* str与str1计算时，false与true进行了隐式转换

  false隐式转换为0

  true隐式转换为1

  故 str结果为 0 + 1 为 1

     str1结果为 1 + 1 为 2

```javascript
if (typeof(a) && -true) {
    console.log(123)
}
// 输出123
```



### 3.写出以下输出

```javascript
function Perosn(name, age) {
    var a = 0;
    this.name = name;
    this.age = age;
    function instance() {
        a++;
        console.log(a);
    }
    this.say = instance;
}
let p = new Perosn();
p.say(); // 1
p.say(); // 2
let p1 = new Perosn();
p1.say();// 3
```

new 关键字的过程：

* 在内存中创建一个新对象
* 这个新对象内部的[[prototype]]特性被赋予为构造函数的protptype属性
* 构造函数内部的this被赋予为这个新对象(即this指向新对象)
* 执行构造函数内部的代码（给新对象添加属性）
* 如果构造函数返回非空对象，则返回该对象，否则，返回刚创建的新对象

p 为第一个person的实例，调用say方法a++，故a为1，之后再次调用，故a为2

P1 为第二个person的实例，两个实例之间互不干扰，故a为1



### 4. 写出以下输出

```javascript
var getName = function () {
    console.log('软猫2号');
}

function A() {
    console.log('A');
    getName = function () {
        console.log('软猫3号');
    }
}

A.sex = function () {
    console.log('A sex');
}
A.prototype.sex = function () {
    console.log('A prototype');
}

function getName() {
    console.log('软猫1号')
}
getName(); // 软猫2号
A.sex(); //  A sex
new A.sex; // A sex
A();   // A
getName(); // 软猫3号
new A().sex(); //  A  A protortype
```

* 调用getName:

  > 函数声明会在任何代码执行之前先被读取并添加到执行上下文，这个过程叫做函数声明提升

  **`函数声明提升的优先级大于变量提升`**

  ❗️**`只有var声明的变量才存在变量提升`**

  根据这个原则：

  ```javascript
  let getName = function() {
    console.log('软猫2号');
  }
  function getName() {
    console.log('软猫1号');
  }
  ```

  实际上的代码为：

  ```javascript
  function getName() {
    console.log('软1号')   // 函数提升
  }
  let getName = function() {
    console.log('软猫2号') //覆盖
  }
  ```

  故最终输出软猫2号

#### ❗️let const 存在暂时性死区 未声明之前不能使用，不存在变量提升


### 1.操作符

#### 逗号操作符

```javascript
let num = 1, num2 = 2, num3 = 3;
//等同于
let num = 1;
let num2 = 2;
let num3 = 3;
```

在使用逗号操作符分隔值，最终会返回表达式中的最后一个值

```javascript
let num = (1,2,3);
console.log(num); //3
```

#### 一元操作符

...待笔记



### 2.声明

***`var`:***

* 在方法之外声明则为全局变量
* 在方法之内声明的变量则为局部变量

```javascript
var a = 123; //全局变量
function test(params) {
    var a = 456;
    console.log(a); // 局部变量
}
test()
console.log(a); //123
```

* 在方法之内，缺省`声明`关键字声明的变量为全局变量

```javascript
function test(params) {
    a = 456;
    console.log(a); // 全局变量
}
test()
console.log(a); // 456
```




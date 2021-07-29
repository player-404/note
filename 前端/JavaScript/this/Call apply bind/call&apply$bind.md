# call bind apply

### call

call() 方法使用一个`指定的 this 值`和单独给出的一个或多个参数`来调用一个函数`

**语法**：

```javascript
function.call(thisArg, arg1, arg2, ...)
function Production(name, pice) {
    this.name = name;
    this.pice = pice;
}
function Food(name, pice) {
    Production.call(this, name, pice); //调用 production 构造函数 并指定this, 传入参数
}
console.log(new Food('chiese', 5).name) //chiese
```

### apply

apply() 方法`调用`一个具有`给定this值`的`函数`，以及以`一个数组（或类数组对象）的形式`提供的`参数`。

**语法**：func.apply(thisArg, [argsArray])

**参数**：

- thisArg：必选的。在 func 函数运行时使用的 this 值。请注意，this可能不是该方法看到的实际值：如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象
- argsArray： 可选的。一个数组或者类数组对象，其中的`数组元素`将`作为单独的参数`传给 func 函数

```javascript
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
arr1.push.apply(arr1, arr2);
console.log(arr1) //[1, 2, 3, 4, 5, 6]
```

### bind

bind() 方法`创建一个新的函数`，在 bind() 被调用时，这个`新函数的 this `被`指定`为 bind() 的`第一个参数`，而其余参数将作为新函数的参数，`供调用时使用`。

**语法**： function.bind(thisArg[, arg1[, arg2[, ...]]])

**参数**：

- thisArg 调用绑定函数时作为 this 参数传递给目标函数的值；
- arg1, arg2 当目标函数被调用时，被预置入绑定函数的参数列表中的参数。

**返回值**: `返回一个原函数的拷贝`，并拥有指定的 this 值和初始参数

```javascript
this.x = 9; //在全局属性下创建了一个属性 x
let obj = {
    x: 81, //在对象obj上创建了一个属性x
    fun() {
        return this.x;
    }
}
console.log(obj.fun()); //81

let fun = obj.fun;  //将对象方法赋值给全局变量fun
console.log(fun()); // 9 this指向全局
let fun1 = fun.bind(obj);//将this指向obj,并将复制fun的新函数返回
console.log(fun1()); //81
```
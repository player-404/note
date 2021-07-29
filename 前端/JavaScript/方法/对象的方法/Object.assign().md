### Object.assign的理解

**作用**：Object.assign可以实现对象的合并。

**语法**：Object.assign(target, ...sources)

```javascript
//Object.assign() 方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象
let per1 = {a: 1, b: 2};
let per2 = {c: 3, d: 2};
let per3 = Object.assign(per1, per2);
console.log(per3); // { a: 1, b: 2, c: 3, d: 2 }

//不可枚举的变量不会合并
let d = 1;
let e = {num: 1};
let g = Object.assign(e, d);
console.log(g); // { num: 1 }

//数组也可以进行合并
let arr = [1,2,3];
let obj = { a: 1, b:2};
let ass = Object.assign(obj, arr);
console.log(ass); //{ '0': 1, '1': 2, '2': 3, a: 1, b: 2 }
```

**解析**：

- Object.assign会将source里面的可枚举属性复制到target，如果和target的已有属性重名，则会覆盖。
- 后续的source会覆盖前面的source的同名属性。

```javascript
 //合并中对象的相同属性会被覆盖
   const a = {name: '张三', age: 21};
   const b = {name: '李四', age: 22};
   const person = {};
   let c = Object.assign(person, a, b); //后面的对象会覆盖前面的同名对象
```

- Object.assign复制的是属性值，如果属性值是一个引用类型，那么复制的其实是引用地址，就会存在引用共享的问题。

```javascript
//引用数据类型会引发数据共享问题
obj.a = 999
console.log(ass);
```
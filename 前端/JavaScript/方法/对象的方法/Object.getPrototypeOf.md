# Object.getPrototypeOf

Object.getPrototypeOf() 方法返回指定对象的原型（内部[[Prototype]]属性的值）。

```javascript
function Person() {
    this.name = '张三';
}

console.log(Object.getPrototypeOf(Person)); //Perosn的原型
let p = new Person();
console.log(Object.getPrototypeOf(p));// p的原型
```


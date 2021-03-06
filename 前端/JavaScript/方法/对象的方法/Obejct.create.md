# Object.create

### Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。

**语法：** Object.create(proto，[propertiesObject])

**参数：**：

- proto -> 新创建对象的原型对象。
- propertiesObject -> 可选。需要传入一个对象，该对象的属性类型参照Object.defineProperties()的第二个参数。如果该参数被指定且不为 undefined，该传入对象的自有可枚举属性(即其自身定义的属性，而不是其原型链上的枚举属性)将为新创建的对象添加指定的属性值和对应的属性描述符。

```javascript
let person = {
    isHuman: false,
    say() {
        console.log(`my name is ${this.name}, Am I human ${this.isHuman}`);
    }
};

let newPerson = Object.create(person); // person对象用作newPerosn的__proto__
```
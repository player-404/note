# Promise

**处理异步任务，`Promise`解决es5之前处理异步任务产生的回调地狱的问题**

### Promise的三种状态

- fulfilled(resolve): 解决
- pending: 等待
- reject: 拒绝

### 创建promise

使用new关键字创建，传入一个`执行期函数(同步执行)`，状态为`pending`

```javascript
let p = new Promise(() => {
}); //传入执行器函数executor
```

### fulfilled(解决)

创建Promise fulfilled状态的两种方法

```javascript
let p3 = Promise.resolve();
let p4 = new Promise((resolve, reject) => resolve());
```

**fulfilled 可以传入一个参数 且会将这个参数转换为`PromiseResult`**

```javascript
let p5 = Promise.resolve(1);
console.log(p5) //Promise<fulfilled>: 1
//多余参数会被忽略
let p6 = new Promise(((resolve, reject) => resolve(1,2,3,4)));
console.log(p6)//Promise<fulfilled>: 1
```

**即使是error也可成为fulfilled 状态的参数**

```javascript
let p7 = Promise.resolve(new Error('foo'));
console.log(p7) //Promise<fulfilled>: Error: foo
```

**当传入的是另一个Promise则会保留该Promise的状态**

```javascript
//当传入的是另一个promise则会保留该promise的状态
let p8 = new Promise(() => {})
console.log(p8) // Promise<pending>
let p9 = Promise.resolve(p8);
console.log(p9) //Promise<pending>
```

### reject(拒绝)

创建Promise reject状态的两种方法

```javascript
let rej1 = new Promise((resolve, reject) => {
    reject();
})
console.log(rej1); // Promise {<rejected>: undefined}
let rej2 = Promise.reject();
console.log(rej2); // Promise {<rejected>: undefined}
```

**传入的参数会成为reject状态的理由**

```javascript
// 传入的参数回成为promise reject状态的理由
let rej3 = Promise.reject(1,2,3,4);
console.log(rej3);// Promise {<rejected>: 1}
let rej4 = Promise.reject(Promise.resolve(1));
console.log(rej4) // Promise {<rejected>: Promise}
```

### Promise.then（）

**then接受两个参数: onResolve处理程序(可选) onReject处理程序(可选) 这个参数是互斥的**

```javascript
let p10 = new Promise(((resolve, reject) => {
    //...此处的代码同步执行
    resolve('ok');
}))
p10.then(data => { //异步执行的
    console.log('data');
})
// then非函数参数会被忽略
let test = new Promise(((resolve, reject) => {
    resolve("i'm ok");
}))
test.then('123'); //忽略
```



**then方法会返回一个新的promise实例，一般状态为resolve**



**下面是then方法返回promise实例的几种情况:**

**then未传入任何参数: 返回promise实例状态继承原promise状态**

```javascript
//例子1
let p = new Promise(() => {

})
console.log(p); // Promise  {pending };
let p2 = p.then();
console.log(p2); // Promise { pending }; 继承原promise状态
//例子2
let p4 = Promise.resolve('a');
let p5 = p4.then();
console.log(p5); //Promise <fulfilled> a;
```



**then: 传入reslove处理程序参数返回promise实例状态的几种情况：**

- 未显式返回任何参数，返回Promise.resolve包装的值为undefined包装的promise实例

```javascript
//then没有显式返回值，返回的promise.resolve()包装的值为undefined
let p1 = new Promise((resolve, reject) => {
    resolve()
})
console.log(p1); // Peomise { fulfilled } undefined
let p3 = p1.then(() => {
    console.log(123)
})
console.log(p3); // promise  <fulfilled> undefined
```

- 显示的返回了值，则then返回返回promise.reslove包装的该值的实例

```javascript
//then方法返回的值则为新primose实例包装的值
let p9 = Promise.resolve();
let p10 = p9.then(() => { return 'ok'});
console.log(p10); //Promise<fulfilled> ok;
```

- 即使返回error,同样返回的是fulfilled(resolve)状态的promise实例

```javascript
let p12 = Promise.resolve();
let p13 = p12.then(() => {
    return new Error('error');
})
console.log(p13);// Promise<fulfilled> error
```

- 输出错误时, then方法则返回的是reject状态的promise实例

```javascript
let p11 = Promise.resolve();
let p12 = p11.then(() => {
    throw Error('error');
})
console.log(p12); // Promise<reject> error
```

- 返回promise时，then返回的promise实例状态则会继承该promise的状态

```javascript
let p14 = new Promise(((resolve, reject) => {
    resolve('ok');
}))
let p15 = p14.then(() => new Promise((resolve, reject) => {reject()}));
console.log(p15); // Promise<reject> undefined
```



**then: 传入reslove处理程序参数返回promise实例状态的几种情况**

- then一般返回的仍是reslove状态的promise实例

```javascript
let p16 = Promise.reject();
let p17 = p16.then(undefined, () => {});
console.log(p17) // Promise<fulfulled> undefined
```

- 返回promise、error、输出error、不返回值、返回值与传入resolve处理函数结果一致
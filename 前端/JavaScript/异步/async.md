# async await

### async 关键字

async声明异步函数, 函数中的代码是同步执行的

```javascript
async function run() {
    console.log(1); //函数中的代码是同步执行的
    return 2;
}
run();
console.log(3); // 1 3
```

async函数默认返回promise.resolve状态的promise实例

```javascript
async function run1() {
    console.log(1);
}
console.log(run1()) // Promise<fulfilled> undefined
run1().then(() => {
    console.log(2);
});
console.log(3); //1 3 2
```

async 可以return一个值 该值会被返回的promise实例包装

```javascript
async function run2() {
    return 1;
}
console.log(run2());// Promise<fulfilled> 1
run2().then((value) => {
    console.log('run2', value); // run2 1
})
```

返回promise pending 则新返回的promise实例状态也为pending resolve同样如此

```javascript
async function r() {
    return new Promise(() => {});
}
console.log(r()); // Promise<pending> undefined
```

同样显示返回reject 或 输出错误则会返回peomise.reject

```javascript
async function run3() {
    return Promise.reject('this is a error');
}
//console.log(run3()); // Promise<reject> this is a error
run3().catch((error) => {
    console.log(error); // this is a error
})

async function run4() {
    throw 'this is a another error';
}
// console.log(run4()); // Promise<reject> this is a another error
run4().catch((error) => {
    console.log(error); // this is a another error
})
```

### await关键字

- await 会时代码暂停代码的执行，让出代码的执行线程，先执行其他同步代码，再待await promise状态改变再继续执行，若await为普通值，则不必等待promise状态改变
- await 会将promise的状态结果返回

```javascript
async function  run5() {
    console.log(123);
    let a = await Promise.resolve('ok'); //awiat 会暂停执行
    console.log(a); // ok
}
console.log(run5()); // Promise<fulfilled> ok
run5();
```

awiat promise状态未确定 则会一直暂停代码执行， async声明的异步函数则返回promise pending状态

```javascript
async function run6() {
    console.log(123);
    let a = await new Promise(() => {}); //状态为pending 后面的代码永远不会执行
    console.log(a);
    console.log("'it's not a await");
}
run6(); //promise<pending> undefined
```

await promise为reject状态: await之后的代码不会在执行，异步函数也返回promise 为reject状态

```javascript
async function run7() {
    return await reject('not ok'); // 返回reject 之后的代码不会再执行 

}
// console.log(run7()); Promise<reject> not ok
run7().catch((error) => {
    console.log(`my error is ${error}`) //尝试再异步函数返回 awiat值，无效 异步函数reject 值无法传递过来
})
```

**借用`红宝书`解释总结await一波**

> JavaScript运行是再碰到await关键字时，会记录再哪里暂停，等到右边的值可用了，Javasctipt 运行时会向消息对列中推送一个任务，这个任务会恢复异步函数的执行（待同步代码执行完毕）。因此，即使await后面跟着一个立即可用的值，函数 的其余部分也会被异步求值。

```javascript
//依次执行
function randomDelay(id) {
    const rand = Math.random() * 1000;
    return new Promise(((resolve, reject) => {
        setTimeout(() => {
            resolve();
            console.log(`${id} finished`)
        }, rand)
    }))
}
async function run() {
    const t = Date.now();
    await randomDelay(1);
    await randomDelay(2);
    await randomDelay(3);
    console.log(`spend ${Date.now() - t}`);
}
run().then(() => {
    console.log('all ok')
})
//随机执行
async function run1() {
    let p = randomDelay(1);
    let p1 = randomDelay(2);
    let p2 = randomDelay(3);
    await  p;
    await p1;
    await p2;
}
run1().then(() => {
    console.log('all ok')
});
```
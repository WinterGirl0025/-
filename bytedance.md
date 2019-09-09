##1.闭包的概念和使用setTimeout函数每秒输出一个数字依次递增
* **闭包的概念**:闭包是在函数中return一个函数以此来保留return函数所在的作用域及其中的变量，在其他地方调用被return的函数可以获取该函数编写时所在作用域的变量及函数（**当函数可以记住并访问所在词法作用域时，就产生了闭包**）
* **闭包的优缺点**：优点在于不必设定太多的全局变量，方便进行模块化开发，缺点在于容易造成内存泄漏，闭包使用的不恰当会导致占用不少空间无法清除
* **清除闭包的方法**：将使用完成的闭包函数置为null,便于清理
* 最近使用的闭包典型--函数当作第一级值类型并到处传递，在定时器、ajax请求，跨窗口通信等异步或同步任务中使用回调函数就是闭包
>setTimeout，每秒输出1个数
```
function setTime() {
    for (var i = 0; i < 10; i++) {
        (
            function () {
                var j = i;
                setTimeout(() => {
                    console.log(j);
                }, j * 1000);
            }
        )(i)
    }
}
```
>使用ES6块级作用域=》let会在每次for循环的时候声明一个新的变量，下次迭代的时候使用上次迭代结束的值初始化
```
function setTime() {
    for (let i = 0; i < 10; i++) {
        setTimeout(() => {
            console.log(i);
        }, i * 1000)
    }
}
```
##2.用css写一个梯形
* 可以采用border，border像画框一样增大宽度可以显示，如果两边颜色不同可以看到分界线是斜线分割的
* 可以利用这一特性画梯形、三角形、正方形和长方形等形状
* 梯形---height设为0，非0情况会撑开整体高度，上方留白
等腰梯形----非等腰或者直角梯形可修改border-left和border-right的宽度或颜色来调整
```
 .ladder{
     height:0;
     width:100px;
     border-bottom:100px solid red;
     border-left:100px solid transparent;
     border-right:100px solid transparent;
 }
//正方形
.square{
     height:0;
     width:0;
     border-top:100px solid red;
     border-bottom:100px solid red;
     border-left:100px solid red;
     border-right:100px solid red;
}
 .square {
     width: 0;
     border-bottom: 100px solid red;
     border-left: 100px solid red;
     border-right: 100px solid red;
 }
//三角形
.trang{
     height: 0;
     width: 0;
     border-bottom: 100px solid red;
     border-left: 100px solid transparent;
     border-right: 100px solid transparent;
 }
//鼠标滑过会变成红色的x号/去掉before和after的transform就是加号
 .add:hover{
     color: rgb(255, 101, 126);
 }
 .add {
     border: 1px solid;
     width: 100px;
     height: 100px;
     border-radius: 50%;//外边框是否为圆形
     color: pink;
     transition: color .25s;
     position: relative;
 }
 .add::before {
     content: '';
     position: absolute;
     left: 50%;
     top: 50%;
     width: 80px;
     margin-left: -40px;
     margin-top: -5px;
     border-top: 10px solid;
     transform: rotate(45deg)
 }
 .add::after {
     content: '';
     position: absolute;
     left: 50%;
     top: 50%;
     height: 80px;
     margin-left: -5px;
     margin-top: -40px;
     border-left: 10px solid;
     transform: rotate(45deg)
 }
//鼠标滑过会变成红色的x号/去掉transform就是加号
 .ladder:hover{
     outline: 20px solid rgb(255, 101, 126);
 }
 .ladder {
     width: 100px;
     height: 100px;
     outline: 20px solid pink;
     transition: outline .25s;
     outline-offset: -68px;
     transform: rotate(45deg);
 }
```
##3.事件循环--微任务和宏任务。给定一个带promis的代码段判断输出
* 事件循环的机制是在js执行过程中产生执行环境，执行环境会被加入到执行栈中，如果遇到异步代码会被挂起放到任务中（分为微任务和宏任务）队列中
* 一旦执行栈为空eventloop会从任务队列中拿取需要执行的的代码放入执行栈执行
* 宏任务包括script、setTimeout、setInterval等，微任务包括promise、promise.nexttick
* 执行的时候先执行script宏任务，截下来有异步代码就先执行微任务
>执行顺序=》
一、执行同步代码，属于宏任务
二、执行栈为空，查询是否有微任务需要执行
三、执行微任务直到微任务队列为空
四、必要的化渲染UI
五、eventloop执行下一个宏任务，执行宏任务的异步代码
**Promise 执行器中的代码会被同步调用，但是回调是基于微任务的**
```
//测试题：
setTimeout(function () {
    console.log(1)
}, 0);
new Promise(function (resolve) {
    console.log(2)
    for (var i = 0; i < 10000; i++) {
        i == 9999 && resolve()
    }
    console.log(3)
}).then(function () {
    console.log(4)
});
console.log(5);
//增强版自测题：
console.log(1)
setTimeout(function () {
    console.log(2);
    let promise = new Promise(function (resolve, reject) {
        console.log(7);
        resolve()
    }).then(function () {
        console.log(8)
    });
}, 1000);
setTimeout(function () {
    console.log(10);
    let promise = new Promise(function (resolve, reject) {
        console.log(11);
        resolve()
    }).then(function () {
        console.log(12)
    });
}, 0);
let promise = new Promise(function (resolve, reject) {
    console.log(3);
    resolve()
}).then(function () {
    console.log(4)
}).then(function () {
    console.log(9)
});
console.log(5)
```
##4.post和get的异同
post和get的底层都是tcp/ip，均为tcp连接
get是不安全的，因为URL是可见的，可能会泄露私密信息，如密码等； post较get安全性较高
get方式只能支持ASCII字符，向服务器传的中文字符可能会乱码。（还需要转个码，encode...） post支持标准字符集，可以正确传递中文字符
post是向服务器提交数据，数据在url中不可见，并产生两个数据包，先发送header服务器响应100 continue后继续发送data
get是从服务器拿取数据，传送的数据在url中可见，由于url长度限制传输数据量有限一般限制在1k，产生一个数据包发送
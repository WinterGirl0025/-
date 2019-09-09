[toc]
## js中哪些操作会触发隐式类型转换
if语句的括号--转化为bool判断、+/-/*等将string转换为number、==将左右变量类型转换再比较
## js造成内存泄漏本人知道的情况
垃圾回收器会定期扫描内存，当某个内存中的值被引用为零时就会将其回收。当前变量已经使用完毕但依然被引用，导致垃圾回收器无法回收这就造成了内存泄漏
1.大量不合时宜的闭包--占用内存
2.意外的全局变量--在函数内声明变量不带let，var或const，意外生成全局变量
3.使用过的setTimeout和setInterval没有被clear

##  JSON.parse,strinify,不可枚举的能转吗?
经测试，不能转，console.log(Json.stringly(a)),a中不可枚举属性不会显示
parse也是一样，当a中变量设置为可枚举时才会console出来
* Object.defineproperty修改属性默认特性，创建新属性时不指定的话默认新属性不可写，不可删除也不可枚举->writable/enumerable/configurable，但如果是修改已存在的属性则没有这种默认
*  _表示只能通过对象方法访问的属性如_age

## 如何判断一个变量为数组
typeof只能判断简单的数据类型，如boolean、number、string、undefined、function、object,无法判断数组、null和通过构造函数返回的自定义对象
判断数组可以采用Array.isArray()、instanceof、constructor和Object.prototype.toString.call()

### 函数名和变量名重复的时候优先函数名

## 雪碧图和字体图标
* 字体图标：高清保真，因为是SVG图形，灵活性，可以设置大小，颜色等，开源的字体库很多
* 减少对服务器的请求次数，维护麻烦，如果修改其中的一张图，你需要修改整张图/高清失真，为了适应不同的分辨率，可能要准备多个规格的图片


## 什么是IIFE立即执行函数？如何实现？
函数声明的时候就立即执行，希望js把函数定义当作函数表达式而不是函数声明
function(){}()==>相当于function(){}; ();没有意义在函数声明后加()和前面的function无关
解析为函数表达式时()就表示立即执行
比如 (function () { counter1(); })();
var i = function(){ counter1(); }();
!function () { counter1(); }();

## css样式的引入方式：
link外联式/style内联式/@import在style中/在标签的style中内嵌式

## css单位包括绝对单位和相对单位
* 绝对单位包括cm/mm/in英尺/pt磅，印刷的点数/pc--1pc = 12pt（常用于传统印刷，少用于前端）
* 相对单位px像素（取决于屏幕分辨率）/%百分/em--1em等于当前元素字体大小/rem--1rem等于根元素字体大小
line-height的百分比是相对于父元素的font-size值来计算的 
vertical-align的百分比是相对于当前元素的line-height值来计算的

## Cookie
>执行流程：访问应用的时候,来到服务器。服务器设置一个cookie，在做响应的时候会通过set-cookie响应头将cookie带给浏览器。
* Cookie实际上是一小段的文本信息。客户端请求服务器，如果服务器需要记录该用户状态，**就使用response向客户端浏览器颁发一个Cookie**。客户端浏览器会把Cookie保存起来。当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器
* **Cookie具有不可跨域名性**
* 修改、删除-->修改某个Cookie，只需要新建一个同名的Cookie(覆盖)
-->删除某个Cookie，只需要新建一个同名的Cookie，并将maxAge设置为0(设置过期时间)
* Cookie的secure属性为true。浏览器只会在HTTPS和SSL等安全协议中传输此类Cookie
>把登录信息如账号、密码等保存在Cookie中，并控制Cookie的有效期，下次访问时再验证Cookie中的登录信息(危险)

>把密码加密后保存到Cookie中，下次访问时解密并与数据库比较。略微安全

>登录时查询一次数据库，以后访问验证登录信息时不再查询数据库。实现方式是把**账号按照一定的规则加密后，连同账号一块保存到Cookie中。下次访问时只需要判断账号的加密规则是否正确**


## Session
>执行流程：浏览器发起一个请求到服务器,服务器先检查你是否携带了一个叫做SESSIONID的cookie
>-->1.<font color="purple">如果有携带</font>，会将此cookie的值取出来（比如为aaa123），然后从服务器的session池中找到ID为aaa123的session返回给调用者。
>-->2.<font color="purple">如果没有携带这个SESSIONID的cookie</font>，那么服务器将会自动创建一个session对象并且生成一个随机字符串（如aaa123）作为此session的ID保存到session池中。在服务器为客户端浏览器作响应的时候自动创建一个键为“SESSIONID” 值为“aaa123”的cookie对象让浏览器储存起来以便下次再访问的时候带过来

* Session是服务器端使用的一种记录客户端状态的机制，增加了服务器的存储压力。
* Session在用户第一次访问服务器的时候自动创建,只要用户继续访问，服务器就会更新Session的最后访问时间，并维护该Session
* session超时-->为防止内存溢出，服务器会把长时间内没有活跃的Session从内存删除。这个时间就是Session的超时时间

##类型转换
* JSON.stringly：不安全的JSON值--undefined、function、symbol等，对象中会忽略，数组中返回null
* toBoolean：undefined、null、false、+0、-0、NaN、""--转化为false
* parseInt是对字符串的转换，非字符串会被转化为字符串后再执行
>  **+其中一个为字符串，执行字符串拼接/否则执行数字加法**
> if、while、for、？：、||等会执行强制bool转换
> ==判断对象时，看是否为同一个对象
> ==字符串和数字-->转数字
> ==其他类型和bool比较-->转换为数字比较
> ! 进行bool值显式强制类型转换

## constructor
constructor 属性是专门为 function 而设计的，它存在于每一个function 的prototype 属性中。这个constructor 保存了指向 function 的一个引用

## promise generator async/awit
1. async是generator的语法糖，*替换成async，yield替换成await
2. 多个await不存在继发关系可以同时触发，在async外定义后在async函数中用一个await后跟promise.all()同时触发
3. 解决回调地狱，使用更语义化、便捷
4. 如果其中一个await出现reject整个async就废掉了，解决方法是把await放在try/catch模块中或在后添加.catch()
* promise写法比回调函数的写法改进了很多，但是代码是promise API,操作本身的语义不易看出

* Generator语义比promise清晰，用户必须有一个任务执行器自动执行Generator函数,co模块promise

* async实现最简洁，最符合语义，将Generator写法中的自动执行器改在语言层提供，不暴露给用户，await命令后面，可以是promise对象和原始类型的值


## 事件流
>事件流即从页面中接收事件的顺序（**即当你点击了一个按钮，你不仅仅点击了按钮，同时也点击了包含按钮的容器，甚至你点击了整个文档页面**）

- IE是事件冒泡流--从点击的元素开始向上（不那么具体的元素如document）传递像水底的泡泡上升
- 事件捕获流--从上向下传播直到点击的具体元素--用意在于在事件到达预设目标之前捕获它

## DOM事件流
包括三个阶段-->事件捕获阶段、处于目标阶段、事件冒泡阶段
* 事件捕获阶段：为截获事件提供机会
* 事件冒泡阶段：对事件做出响应
* addEventListener和removeEventListener最后一个参数是boolean，true表示捕获阶段调用事件处理函数，false表示在事件冒泡阶段调用事件处理函数
* **一般建议在事件冒泡阶段处理事情，可以最大限度兼容浏览器**，只在需要事件到达目标前捕获才用true
* currentTarget处理事件的当前目标/Target事件的目标
* 取消冒泡--.stopPropagation()/IE--cancelBubble==true

## DOM的增删查改
一、节点创建型API

1.1 createElement--仅创建需要appendchild添加
1.2 createTextNode--仅创建
1.3 cloneNode----克隆节点
1.4 createDocumentFragment----存储临时的节点用来准备添加到文档中/fragment.appendChild(li);然后再appendfragement

二、页面修改形API（包括删除和添加）（删）（改）

2.1 appendChild(追加为子元素)
2.2 insertBefore(插入前面)
2.3 removeChild(删除子元素)
2.4 replaceChild(替换子元素)

三 节点查询型API(查)

3.1 document.getElementById
3.2 document.getElementsByTagName
3.3 document.getElementsByName
3.4 document.getElementsByClassName
3.5 document.querySelector和document.querySelectorAll
3.6 elementFromPoint()--返回位于页面指定位置的元素（x,y）css坐标

四 元素属性型操作（属性节点的操作）

一般只操作html标签上的属性如id；class之类的；
但是style属性如宽高背景色之类的一般写在css里面所以要通过element.currentStyle : window.getComputedStyle(element,pseduoElement)来操作


4.1 getAttribute()  (获取属性)
4.2 createAttribute()  (创建属性)
4.3 setAttribute()  (设置属性)
4.4 romoveAttribute()  (删除属性)
4.5 element.attributes（将属性生成数组对象）

## create和new
* new操作符会将构造函数的prototype指定的原型对象赋值给新对象的<font color=red>Prototype</font>
* Object.create(proto)将参数proto指定的原型对象赋值给新对象的<font color=red>Prototype</font>
* 如果参数为null的话，Object.create则会创建空对象。

>特别需要指出的是Object.create(null)和new Object()的区别：两者都是创建空对象，但是**new创建出的空对象会绑定Object的prototype原型对象，但是Object.create(null)的空对象是没有任何属性的。**

* create-->var o = Object.create(Base);
```
Object.create = function(Base){
    var F = function(){};
    F.prototype = Base;
    return new F();
}
```
* new-->var o = new Base();
```
var o = {};
o.__proto__ = Base.prototype;
Base.call(o);
```
## 原型链继承
利用原型让一个对象继承另一个对象的属性和方法
```
function super(){
    this.isprototype = true;
}
super.prototype.getsuperValue = function(){
    return this.isprototype;
}
function sub(){
    this.subprototype = false;
}
sub.prototype = new super();//继承super
sub.prototype.getsubValue = function(){
    return this.subprototype;
}
var instance = new sub();
instance.getsuperValue();  //true
```
## 原型继承的缺点
1.引用类型的属性被所有实例共享
2.在创建 Child 的实例时，不能向Parent传参
## 构造函数继承
```
function parent{
    name=['aa','bb']
}
function child(name){
    parent.call(this,name)
}
var child1 = new child('cc');
console.log(child1.name);   //'aa','bb','cc'
var child2 = new child();
console.log(child2.name);  //'aa','bb'
```
可以向parent传参，避免了引用类型的属性被所有实例共享


## js预编译
主要总结为两点-->变量声明声明提升，函数声明整体提升
## css预编译
扩展CSS功能的写法，比如less,sass-->作用是把上面这段浏览器不认识的代码，还原为浏览器认识的CSS标准发给浏览器解析.

## 前端设计模式
* <font color=red>单例模式</font>--保证一个类只有一个实例，并且提供一个访问它的全局访问点，比如：全局缓存、浏览器中的window对象
-->可以用来划分命名空间，减少全局变量的数量
可以被实例化，且实例化一次，再次实例化生成的也是第一个实例
* <font color=red>观察者模式(发布订阅者)</font>--对象间的一种一对多的依赖关系，一个对象的状态发生变化时，所有依赖于他的对象都将得到通知
-->指定发布者，给发布者添加缓存列表用于存放回调函数通知订阅者，发布消息的时候，发布者会遍历这个缓存列表，依次触发里面存放的订阅者回调函数
* <font color=red>工厂模式</font>--封装多个类，依据传入的类型选择其中一个进行实例化

## 用户行为对缓存的影响
* 刷新 或F5-->浏览器直接对本地的缓存文件过期，但是会带上If-Modifed-Since，If-None-Match（如果上一次response带Last-Modified, Etag）
* 强制刷新ctrl+F5 -->浏览器不仅会对本地文件过期，而且不会带上If-Modifed-Since，If-None-Match，相当于之前从来没有请求过
* 地址栏回车/点击-->浏览器发起请求，按照正常流程，本地检查是否过期，然后服务器检查新鲜度，最后返回内容

## 进程与线程
进程之间相互独立，一个进程由一个或多个线程组成，多个线程在进程中协作完成任务，同一进程下的各个线程之间共享程序的内存空间
>进程是cpu资源分配的最小单位
浏览器是多进程的
浏览器之所以能够运行，是因为系统给它的进程分配了资源（cpu、内存）
简单点理解，每打开一个Tab页，就相当于创建了一个独立的浏览器进程

浏览器的渲染进程是多线程的--GUI渲染线程与JS引擎线程是互斥的，当JS引擎执行时GUI线程会被挂起（相当于被冻结了），GUI更新会被保存在一个队列中等到JS引擎空闲时立即被执行。
## #页面渲染及重绘、重排
<font color='red'>重绘：</font>重新绘制受到影响的部分到屏幕，一般是外观改变，如背景变色、字体变色等
<font color='red'>重排（回流）：</font>节点尺寸发生变化或节点发生增删等操作，浏览器会使渲染树中受到影响的部分失效，重新构造,比如改变某div的尺寸如margin/padding/border/width/height，添加删除dom节点、浏览器窗口尺寸改变等
><font color='blue'>重绘不一定需要重排（比如颜色的改变），重排必然导致重绘（比如改变网页位置）</font>

优化：
1.不要一条一条地修改 DOM 的样式。可以先定义好 css 的 class，然后修改 DOM 的 className
2.动画的 HTML 元件使用 fixed 或 absoult 的 position，那么修改他们的 CSS 是不会 reflow 的
## add(1)(2)
```
function add(x){
    var sum = x;
    var tmp = function(y){
        sum = sum+y;
        return tmp;
    }
    tmp.toString = function(){return sum;};
    tmp.valueOf = function(){return sum;};
    return tmp;
}
console.log(add(1)(2)(3)) //-->1+2+3=6
```

## 扁平化
* 数组
flat-->arr.flat()
实现原理--while(arr.some(Array.isArray)){
    arr = [].concat(...arr);
}
* 对象

## XSS攻击--跨站脚本攻击
1. 在网页 input 或者 textarea 中输入 <script>alert('xss')</script>或者其他脚本
2. 直接使用 URL 参数攻击
```
https://www.baidu.com?jarttoTest=<script>alert(document.cookie)</script>
```
防御：
1. 输入过滤
2. 将不可信数据插入到HTML URL里时，对这些数据进行URL编码
3. 使用 HttpOnly Cookie--HttpOnly就是在设置cookie时接受这样一个参数，一旦被设置，在浏览器的document对象中就看不到cookie了
## CSRF攻击--跨站请求伪造
```
a银行网站通过get请求转账
http://www.bank.com/Transfer.php?BankId=11&money=1000
攻击-->登陆a，然后访问危险网站b
<img src='http://www.bank.com/Transfer.php?BankId=11&money=1000'>
```
防御：
>可以完全伪造用户的请求，该请求中所有的用户验证信息都是存在于 cookie 中，因此黑客可以在不知道这些验证信息的情况下直接利用用户自己的 cookie 来通过安全验证。要抵御 CSRF，关键在于在请求中放入黑客所不能伪造的信息，并且该信息不存在于 cookie 之中
1. 验证 HTTP Referer 字段，利用 HTTP 头中的 Referer 判断请求来源是否合法
2. 在请求地址中添加 token 并验证
3. 在 HTTP 头中自定义属性并验证

## 动态作用域静态作用域（词法作用域）
动态作用域是在运行时确定的，词法作用域是在写代码或定义的时候确定的，js没有动态作用域，但this机制很像动态作用域
## const为什么变量不能改变，对象中的可以
const定义的基本类型不能改变，但是定义的对象是可以通过修改对象属性等方法来改变的

## display，float，position
'display'值为'none'，则'position' 和 'float'无作用。这种情况下，不生成box
position为fixed或absolute且float为none或未设置则box位置由left等属性设置
float为其他值，则元素浮动，且为块级框
绝对定位、弹性布局和网格布局容器的内容项的display属性会被块级化
只要设置了position：absolute或float中任意一个，都会让元素以display：inline-block的方式显示

## MVVM的理解
是指model-view-viewmodel
model--数据模型
view--UI组件，负责将数据模型转化为ui展现
viewmodel--监听模型数据的改变和控制视图行为，同步view和model的对象，**将模型转化为视图方式为数据绑定，将数据转化为模型的方式是dom事件监听，实现双方称之为数据的双向绑定**

## v-if/v-show
v-show初始渲染消耗大，会将组建渲染到页面，如果不显示相当于display:none；
v-if会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建
如果频繁切换v-show更合适，如果条件改变较少v-if更合适

## vue事件绑定
* 内联直接把js写在标签上调用方法
* 调用methods里定义的方法
* 方法传参，方法直接在调用时在方法内传入参数
```
 <button @click="deleteData('111')">执行方法传值111</button>
```
* 方法传参，传入事件对象
```
 <button data-aid='123' @click="eventFn($event)">事件对象</button>  
```

## vue双向绑定
利用defineproperty劫持data中数据的get和set，在每次读写数据的时候通知订阅者进行对应的操作，在get阶段查找还有没有需要订阅的对象，添加后设target为空，在set触发时对订阅者发布消息执行订阅者对应的函数操作。

## 前后端常用通讯方式-- ajax 、websocket
* ajax适用于大量数据的传输--服务器只能应答
* websocket适用于少量数据的实时交互--是一种全双工通信机制，两端可以及时地互发事件，互发数据，相互通信
1.建立在tcp协议之上，服务器端实现较容易
2.可以发送文本或二进制
3.和http兼容良好，握手阶段采用http协议，端口也是80或443
4.不存在同源限制
5.协议符号是ws，基于ssl加密是wss
6.不兼容低版本ie，数据发送量小
发送一个http请求以发起连接，获得服务器响应后连接变成ws/wss

## ajax和axios
* Ajax 指的是 XMLHttpRequest（XHR）隶属于原始js中，核心使用XMLHttpRequest对象，多个请求之间如果有先后关系的话，就会出现回调地狱。
* axios 是一个基于Promise 用于浏览器和 nodejs 的 HTTP 客户端，本质上也是对原生XHR的封装，只不过它是Promise的实现版本
>axios特征：
1.从浏览器中创建 XMLHttpRequest，
2.体积较小，
3.支持 Promise API，
4.自动转换JSON数据，
5.客户端支持防止CSRF**让你的每个请求都带一个从cookie中拿到的key, 根据浏览器同源策略，假冒的网站是拿不到你cookie中得key的**

## vue组件间传值
父子组件：props和$emit
兄弟组件：eventBus和vuex或向共同的父节点传递再获取
## eventbus
非父子关系的组件进行通信时，可以使用一个空的Vue实例作为中央事件总线。
1.新建一个bus.js
```
import Vue from 'vue';
export default new Vue();
```
2.在数据发出页send和数据接收页get中引入bus.js
```
import Bus from 'xx/bus.js'
```
3.send中在触发函数中添加
```
触发函数(){
    Bus.$emit('dataname',datavalue)
}
```
4.get中在获取函数中添加
```
获取函数(){
    Bus.$on('dataname',value=>{
        console.log(value)
    })
}
```

## vuex
* 为应用中的所有组件提供集中式的状态存储与操作，保证了所有状态以可预测的方式进行修改。
* dispatch：操作行为触发方法，是唯一能执行action的方法。
* actions：操作行为处理模块，包含同步/异步操作，支持多个同名方法，按照注册的顺序依次触发
* commit：状态改变提交操作方法。对mutation进行提交，是唯一能执行mutation的方法
* mutations：状态改变操作方法。方法只能进行同步操作，且方法名只能全局唯一
* state：页面状态管理容器对象

## vue-router
* mode的hash模式会在url中插入#，在对服务器进行资源请求的时候不会匹配#之后的内容，#之后的内容仅用于前端指导浏览器动作，因此改变hash不会导致页面重载
1.push()会对window的hash进行了直接的赋值，hash的改变会自动添加到浏览器的访问历史
2.replace()它并不是将新路由添加到浏览器访问历史栈顶，而是替换掉当前的路由
>window.location.hash属性读取.

* mode的history模式是浏览器历史记录栈提供的接口，通过back(),forward(),go()等方法，我们可以读取浏览器历史记录栈的信息，进行各种跳转操作
* pushState(),replaceState()使得我们可以对浏览器历史记录栈进行修改，但不会触发请求
>用到了HTML5的新特性，需要浏版本的支持

>比如http://oursite.com/#/user/id
>hash模式仅改变hash部分的内容，而hash部分是不会包含在http请求中的,history模式则将url修改的就和正常请求后端的url一样,**这种向后端发送请求的话，后端没有配置对应/user/id的get路由处理,会返回404错误**

* 路由守卫-->
1.全局的router.beforeEach/afterEach
2.路由独享守卫--在每个路由中定义的beforeEnter
3.组件内守卫--beforeRouteEnter/beforeRouteUpdate (2.2 新增)/beforeRouteLeave
## vue的diff算法
## #1.vue是怎么更新节点的

**直接渲染到真实dom上会引起整个dom树的重绘和重排**
先根据真实DOM生成一颗virtual DOM，当virtual DOM某个节点的数据改变后会生成一个新的Vnode，然后Vnode和oldVnode作对比，发现有不一样的地方就直接修改在真实的DOM上，然后使oldVnode的值为Vnode。

## #2.虚拟dom--virtual DOM
真实的DOM的数据抽取出来，以对象的形式模拟树形结构

## #3.diff比较方式
采取diff算法比较新旧节点的时候，比较只会在同层级进行, 不会跨层级比较

## 生命周期
beforeCreate--实例创建，但data和methods未初始化好
created--实例创建完成，data和methods创建，没有开始编译模板**最早调用methods的地方**
beforeMounte--模板完成，还未挂载，页面还是旧的
mounted--挂载完成，**最早通过插件或操作修改dom节点的地方**
beforeUpdate--data数据更新了，页面未更新
updated--更新结束
beforeDestory--销毁前，this。data。methods还处于可用状态
destroyed--销毁结束

## refs属性
ref用于给元素或子组件注册引用信息
可以在$refs中通过ref注册时的名称获取到元素或子组件
>而$refs相对document.getElementById的方法，会减少获取dom节点的消耗。
## 渲染函数render()
## webpack
## vue与其他框架的对比
## 移动端适配方案
1.通过媒体查询的方式即CSS3的meida queries--通过查询设备的宽度来执行不同的 css 代码，最终达到界面的配置。
@media screen and (max-width: 600px) { /*当屏幕尺寸小于600px时，应用下面的CSS样式*/}
* 优点：调整屏幕宽度的时候不用刷新页面即可响应式展示
可以做到设备像素比的判断，方法简单，成本低，特别是对移动和PC维护同一套代码的时候

* 缺点：代码量比较大，维护不方便

2.Flex弹性布局
viewport是固定的：
```
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
```
高度定死，宽度自适应，元素都采用px做单位
3.rem布局

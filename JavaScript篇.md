### 1、浏览器提供给js哪些API

控制台打印：console.log()

弹窗提醒：alert()

获取dom元素：document.getElementById()、document.getElementByClassName()、document.getElementByTagName()、document.querySelector()、document.querySelectorAll()

添加dom元素：document.createElement()、document.createTextNode()、el.appendChild()

储存：window.localstorage.setItem()、window.localstorage.getItem()、window.sessionStorage.setItem()、window.sessionStorage.getItem()

### 2、script标签一般放在那里,为什么,defer和async的区别

script标签一般放在body后面

如果将script放在<head>里，浏览器解析HTML，发现script标签时，会先下载完所有这些script，再往下解析其他的HTML，浏览器最多只能同时下载两个JS，且浏览器下载JS时，不能同时并行解析HTML，会让网页内容呈现滞后，导致用户感觉到卡。

但是将script放在尾部的缺点，是浏览器只能先解析完整个HTML页面，再下载JS。而对于一些高度依赖于JS的网页，就会显得慢了。所以将script放在尾部也不是最优解。

script如果没有defer或async，浏览器遇到js文件会立即加载并执行指定的脚本，也就是说不等待后续载入的文档元素，读到就加载并执行。如果带有defer或async，那么加载js文件，相对于解析html是异步的，加载js文件不阻断html文件的解析。

带有async属性的script，只要下载完就执行，所以这可能导致后面的js文件比前面的js文件先执行；同时，它的执行会阻断html的解析

带有defer属性的script，在HTML解析完成之后再执行，同时，能保证js文件按照引入的顺序，有序执行。

### 3、闭包

闭包是指有权访问另外一个函数作用域中的变量的函数，是函数内部与外部连接的桥梁

正常函数执行完毕后，里面声明的变量被垃圾回收处理掉，但是闭包可以让作用域里的变量，在函数执行完之后依旧保持，没有被垃圾回收处理掉。

缺点： ①闭包会导致内存占用过高，因为变量都没有释放内存；②内存泄漏

解决办法：退出函数之前，将不使用的局部变量全部删除或者赋为null

优点：①可以读取函数内部的变量；②让这些变量的值始终保持在内存中，不会在函数被调用后被自动清除。

**实现一个闭包**——封装变量

```js
var fn = function() {
    var name = '孙某人'
    return {
        getName: function() {
            return name
        },
        setName: function(newName) {
            return name = newName
        }
    }
}
var fn1 = new fn()
console.log(fn1.name); // undefined
console.log(fn1.getName()); // 孙某人
```

### 4、原型链

原型链就是js引擎在访问一个对象属性的时候，如果对象自身没有找到该属性的情况下会去它的原型对象中查找，如果还没有，会去它的原型对象的原型对象中查找，递归访问`_proto_`，如此循环直到顶层的Object对象的原型对象为止的这么个机制

**原型链：** 是由子对象对父对象进行多次原型继承形成的链式关系。当调用子对象的某个属性或方法时，javascript会向上遍历原型链，直到找到为止，没有则返回undefined。

![image-20201026125231016](D:\GitHubRepository\JavaScript篇.assets\image-20201026125231016.png)

**原型链继承的三种方法**

https://zhuanlan.zhihu.com/p/160868667

原型链的终点：**Object.prototype**

### 5、数组去重

通过ES6的新特性Set，它类似于数组，但其中的元素都是唯一的，没有重复的，所以可以利用Set实现数组去重。

var arr = [1, 2, 3, 1, 2]; var newArr= [...new Set(arr)]

### 6、如何判断变量是数组还是object

用typeof判断数组或者object，返回的均为object；

用instanceof、constructor、Object.prototype.toString.call()判断都可以，用法如下：

```javascript
var arr = new Array();
var arr = ['aa','bb','cc'];
var obj = {
	'a': 'aa',
	'b': 'bb',
	'c': 'cc'
};
console.log(arr instanceof Array); //true
console.log(arr instanceof Object); //true
console.log(obj instanceof Array); //false
console.log(obj instanceof Object); //true

console.log(arr.constructor === Array); //true
console.log(arr.constructor === Object); //false
console.log(obj.constructor === Object); //true

var a = NaN;
var b= '222';
var c = null; 
var d = false;
var e = undefined;
var f = Symbol();
var arr = ['aa','bb','cc'];
var obj = {
'a': 'aa',
'b': 'bb',
'c': 'cc'
};
var res = Object.prototype.toString.call(arr);
console.log(res); //[object Array]
var res2 = Object.prototype.toString.call(obj);
console.log(res2); //[object Object]
var res3 = Object.prototype.toString.call(a);
console.log(res3); //[object Number]
var res4 = Object.prototype.toString.call(b);
console.log(res4); //[object String]
var res4 = Object.prototype.toString.call(c);
console.log(res4); //[object Null]
var res5 = Object.prototype.toString.call(d);
console.log(res5); //[object Boolean]
var res6 = Object.prototype.toString.call(e);
console.log(res6); //[object Undefined]
var res7 = Object.prototype.toString.call(f);
console.log(res7); //[object Symbol]
```

### 7、js给input提供了哪些监听事件

1.onfocus  当input 获取到焦点时触发

2.onblur  当input失去焦点时触发，注意：这个事件触发的前提是已经获取了焦点再失去焦点的时候会触发相应的js

3.onchange 当input失去焦点并且它的value值发生变化时触发

4.onkeydown 在 input中有键按住的时候执行一些代码

5.onkeyup 在input中有键抬起的时候触发的事件，在此事件触发之前一定触发了onkeydown事件

6.onclick  主要是用于 input type=button，当被点击时触发此事件

7.onselect  当input里的内容文本被选中后执行一段，只要选择了就会触发，不是非得全部选中

8.oninput  当input的value值发生变化时就会触发，不用等到失去焦点（与onchange的区别）

### 8、javascript数据基本类型有哪些？

null、boolean、string、undefined、number、symbol(ES6新加)

### 9、事件代理

​		事件委托也叫事件代理，是利用事件冒泡机制，减少DOM操作。当HTML中一个子元素的click、mouseup、mousdown、keyup、keydown等等事件被触发，那么其父元素上的相应事件也会被触发。当这样的子元素一多，在事件被触发之后执行的DOM操作就会很多，如果将这些操作委托给父元素，那么将可以减少很多的DOM操作。

#### 9.1 阻止事件冒泡

在除IE以外其他的浏览器中通过`e.stopPropagation()`方式阻止事件的冒泡；

在IE浏览器中通过`e.cancleBubble=true`阻止事件冒泡。

**实现一个事件代理**

```html
<ul id="box">
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
    <li>7</li>
    <li>8</li>
</ul>
```

```js
var oBox = document.getElementById('box')
oBox.onclick = function(e) {
    console.log(e.target.innerHTML)
}
```

### 10、import、export以及export default的区别

export用于对外输出本模块(一个文件可以理解为一个模块)变量的接口
import用于在一个模块中加载另一个含有export接口的模块。

1、export与export default均可用于导出常量、函数、文件、模块等
2、你可以在其它文件或模块中通过import+(常量|函数|文件|模块)名的方式，将其导入，以便能够对其进行使用
3、在个文件或模块中，export、import可以有多 个，export default仅有一个
4、通过export方式导出， 在导入时要加{ }, export default则不需要，即export与export default可以实现同样的目的，只是用法有些区别。

### 11、手写原生ajax

```js
function ajax (method = 'get', url, data = '', success) {
    // 第一步：创建XMLHttpRequest对象，用于在后台与服务器交换数据
    var xhr;
    if (window.XMLHttpRequest) {
        //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
        xhr = new XMLHttpRequest();
    } else {
        // IE6, IE5 浏览器执行代码
        xhr = new ActiveXObject("Microsoft.XMLHTTP");
    }

    if(method.toLowerCase === 'post') {
        // 如果请求方式是post的话，还需要加一个请求头
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        
        // 第二步：使用open方法设置与服务器的交互信息；
        // open 三个参数：open(method, URL, Asynchronous) => 第一个表示请求的方式，第二个表示请求的地址，第三个表示是否支持异步执行
        xhr.open(method, url ,false);

        // 第三步：发送请求
        xhr.send(data);
    } else {
        xhr.open(method, url ,false);
        xhr.send(null);
    }
    
    // 第四步：注册事件，当onreadystatechange状态改变就会调用
    xhr.onreadystatechange = function() {
        // readyState === 4说明请求已完成
        // 0: 请求未初始化
        // 1: 服务器连接已建立
        // 2: 请求已接收
        // 3: 请求处理中
        // 4: 请求已完成，且响应已就绪
        if(xhr.readyState === 4){
            if(xhr.status === 200 || xhr.status === 304){
                console.log(xhr.responseText)
                if(success) {
                    success()
                }
            }
        }
    }
}
```

### 12、手写一个深拷贝

```java
function deepCopy(obj){
    //判断是否是简单数据类型，
    if(typeof obj == "object"){
        //复杂数据类型
        var result = obj.constructor == Array ? [] : {};
        for(let i in obj){
            result[i] = typeof obj[i] == "object" ? deepCopy(obj[i]) : obj[i];
        }
    }else {
        //简单数据类型 直接 == 赋值
        var result = obj;
    }
    return result;
}
```

### 13、防抖函数、节流函数

**防抖**——触发高频函数事件后，n秒内函数只能执行一次，如果在n秒内这个事件再次被触发的话，那么会重新计算时间。

**节流**——高频事件触发，但在n秒内只会执行一次，所以节流会稀释函数的执行频率。

```js
// 防抖函数
function debounce(fn, wait) {
    wait = wait || 0
    var timerId;
    function helper() {
        if(timerId) {
            clearTimeout(timerId)
            timerId = null
        }
        timerId = setTimeout(function() {
            fn()
        }, wait)
    }
    return helper
}

// 节流函数
function throttle(fn, holder) {
    var last;
    var timer;
    holder = holder || 500
    return function() {
        var context = this;
        var args = arguments
        var curTime = +new Date() 
        if(last && curTime < last + holder) {
            clearTimeout(timer)
            timer = setTimeout(function() {
                last = curTime
                fn.apply(context,args)
            }, holder)
        } else {
            last = curTime
            fn.apply(context, args)
        }
    }
}
```

### 14、Object.prototype.toString. call() 、 instanceof 以及 Array.isArray()的区别

**Object.prototype.toString.call()**

每一个继承`Object 的对象都有 toString`方法，如果`toString`方法没有重写的话，会返回`[Object type]`，其中`type`为对象的类型。但当除了`Object`类型的 对象外，其他类型直接使用`toString`方法时，会直接返回都是内容的字符串， 所以我们需要使用`call`或者`apply`方法来改变`toString`方法的执行上下文。

这种方法对于所有基本的数据类型都能进行判断，即使是`null`和`undefined`。`Object.prototype.toString.call()`常用于判断浏览器内置对象时。

```js
const an = ['Hello','An'];
an.toString(); // "Hello,An"
Object.prototype.toString.call(an);  // "[object Array]" 
Object.prototype.toString.call('An'); // "[object String]"
Object.prototype.toString.call(1); // "[object Number]"
Object.prototype.toString.call(Symbol(1)); // "[object Symbol]"
Object.prototype.toString.call(null); // "[object Null]"
Object.prototype.toString.call(undefined); // "[object Undefined]"
Object.prototype.toString.call(function(){}); // "[object Function]"
Object.prototype.toString.call({name: 'An'}); // "[object Object]"
```

**instanceof **

`instanceof`的内部机制是通过判断对象的原型链中是不是能找到类型的`prototype`。 

使用`instanceof`判断一个对象是否为数组，`instanceof`会判断这个对象的原型链上是否会找到对应的`Array`的原型，找到返回`true`，否则返回`false`。

```js
 [] instanceof Array; // true
```

 但`instanceof`只能用来判断对象类型，原始类型不可以。并且所有对象类型`instanceof Object`都是 true。 

```js
[] instanceof Object; // true
```

**Array.isArray()**

功能：用来判断对象是否为数组

`instanceof`与`isArray`当检测到`Array`实例时，`Array.isArray`优于`instanceof`，因为`Array.isArray`可以检测出 `iframes`

```js
var iframe = document.createElement('iframe'); 
document.body.appendChild(iframe);
xArray = window.frames[window.frames.length-1].Array; 
// Correctly checking for Array
var arr = new xArray(1,2,3); // [1,2,3]
Array.isArray(arr); // true
// Considered harmful, because doesn't work though iframes
Object.prototype.toString.call(arr); // true
arr instanceof Array; // false 
```

`Array.isArray()`是`ES5`新增的方法，当不存在`Array.isArray()`，可以用`Object.prototype.toString.call()`实现。

```js
if (!Array.isArray) {
    Array.isArray = function(arg) {
        return Object.prototype.toString.call(arg) === '[object Array]';
    }; 
}
```

### 15、事件模型、怎么在事件捕获阶段触发事件

JavaScript事件模型主要分为3种：原始事件模型、DOM2事件模型、IE事件模型。

#### **原始事件模型**

这是一种被**所有浏览器都支持的事件模型**，对于原始事件而言，**没有事件流**，事件一旦发生将马上进行处理，有两种方式可以实现原始事件：

（1）在html代码中直接指定属性值：`<button id="demo" type="button" onclick="doSomeTing()" />`

（2）在js代码中为:`document.getElementsById("demo").onclick = doSomeTing()`

优点：所有浏览器都兼容

缺点：

1）逻辑与显示没有分离；

2）相同事件的监听函数只能绑定一个，后绑定的会覆盖掉前面的，如：a.onclick = func1; a.onclick = func2;将只会执行func2中的内容。

3）无法通过事件的冒泡、委托等机制（后面会讲到）完成更多事情。

#### **DOM2事件模型**

此模型是W3C制定的标准模型，现代浏览器（IE6~8除外）都已经遵循这个规范。W3C制定的事件模型中，一次事件的发生包含三个过程：(1).事件捕获阶段，(2).事件目标阶段，(3).事件冒泡阶段。

事件捕获：当某个元素触发某个事件（如onclick），顶层对象`document`就会发出一个事件流，随着DOM树的节点向目标元素节点流去，直到到达事件真正发生的目标元素。在这个过程中，事件相应的监听函数是不会被触发的。

事件目标：当到达目标元素之后，执行目标元素该事件相应的处理函数。如果没有绑定监听函数，那就不执行。

事件冒泡：从目标元素开始，往顶层元素传播。途中如果有节点绑定了相应的事件处理函数，这些函数都会被一次触发。

所有的事件类型都会经历事件捕获但是只有部分事件会经历事件冒泡阶段,例如submit事件就不会被冒泡。 

**事件的传播是可以阻止的：**

　　• 在W3c中，使用`stopPropagation()`方法
　　• 在IE下设置`cancelBubble = true;`
　　在捕获的过程中`stopPropagation()`后，后面的冒泡过程就不会发生了。

**标准的事件监听器该如何绑定：**

`addEventListener("eventType","handler","true|false");`其中`eventType`指事件类型，**注意不要加`on`前缀，与IE下不同**。第二个参数是处理函数，第三个即用来指定是否在捕获阶段进行处理，一般设为false来与IE保持一致(默认设置)，除非你有特殊的逻辑需求。监听器的解除也类似：`removeEventListner("eventType","handler","true|false");`

#### **IE事件模型**

IE不把对象传到事件处理函数中，由于在任意时刻只会存在一个事件，所以IE把事件`Event`作为全局对象`window`的一个属性；`IE`的事件模型只有两步，先执行元素的监听函数，然后事件沿着父节点一直冒泡到`document`。绑定监听函数的方法是：`attachEvent( "eventType","handler")`，其中`evetType`为事件的类型，如`onclick`，注意要加`on`。解除事件监听器的方法是 `detachEvent("eventType","handler")`

#### 怎么在事件捕获阶段触发事件

在为事件绑定监听事件`addEventListener`时，将第三个参数设置为`true`就好.

### 16、各种排序算法的时间复杂度及空间复杂度

<img src="D:\typora笔记\面试复习题.assets\image-20201127142556767.png" alt="image-20201127142556767" style="zoom:67%;" />

### 17、各种查询算法的时间复杂度

<img src="D:\typora笔记\面试复习题.assets\image-20201127143012448.png" alt="image-20201127143012448" style="zoom:67%;" />

### 18、new的实现原理

1、创建一个空对象obj

2、将新对象 obj 的 `__proto__ `链接到构造函数的原型 prototype，

3、执行构造函数方法，将属性和方法添加到this引用的对象中

4、如果构造函数中没有返回其它对象，那么返回this，即创建的这个的新对象，否则，返回构造函数中返回的对象。

### 19、箭头函数为什么不能使用new

没有自己的 this，无法调用 call，apply。 

没有 prototype 属性 ，而 new 命令在执行时需要将构造函数的 prototype 赋值给新的对象的 `__proto__`

### 20、Promise

- Promise.prototype.then

实例方法，为 Promise 注册回调函数，函数形式：`fn (vlaue){}`，`value`是上一个任务的返回结果，`then`中的函数一定要`return`一个结果或者一个新的`Promise`对象，才可以让之后的`then`回调接收。

- Promise.prototype.catch

实例方法，捕获异常，函数形式：`fn(err){}`，`err`是`catch`注册 之前的回调抛出的异常信息。

- Promise.race

类方法，多个`Promise`任务同时执行，返回最先执行结束的`Promise`任务的结果，不管这个`Promise`结果是成功还是失败。

- Promise.all

类方法，多个`Promise`任务同时执行。如果全部成功执行，则以数组的方式返回所有`Promise`任务的执行结果。如果有一个`Promise`任务`rejected`，则只返回`rejected`任务的结果。

### 21、promise内部有几种状态

promise有三种状态：pending、reslove、reject 。

pending就是未解决，resolve为成功，reject为失败。

### 22、promise里面new Error()，用try catch可以捕获吗？

能。直接在调用链的末尾加上`.catch(error => alert(error))`

### 23、面向对象三大特性

三大特性：封装，继承，多态

封装
---->减少了大量的冗余代码
---->封装将复杂的功能封装起来，对外开放一个接口，简单调用即可。

继承–单根性，传递性
---->减少了类的冗余代码
---->让类与类之间产生关系，为多态打下基础

多态

实现多态，有二种方式，覆盖，重载。
覆盖，是指子类重新定义父类的虚函数的做法。
重载，是指允许存在多个同名函数，而这些函数的参数表不同（或许参数个数不同，或许参数类型不同，或许两者都不同）

### 24、js操作dom的方法

```js
document.getElementById('');
document.getElementsByClassName('')
document.getElementsByTagName('')
document.querySelector('')
document.querySelectorAll('')
```

### 25、diff算法

diff算法其实就是对DOM进行比较不同的一种算法补丁：用来更新DOM

<img src="D:\typora笔记\面试必看\面试复习题.assets\image-20201202115424693.png" alt="image-20201202115424693" style="zoom:50%;" />

`patch`函数接收两个参数`Vnode`和`oldVnode`分别代表新的节点和之前的旧节点

如果两个节点都是一样的，那么就深入检查他们的子节点。如果两个节点不一样那就说明旧节点被改变了，就可以直接替换为新节点。

如果旧节点没有，则删除真实元素的子节点
如果旧节点没有子节点而新节点有，则将新节点的子节点添加到真实元素中
如果两者都有子节点，则执行`updateChildren`函数比较子节点

`updateChildren`函数主要作用是：将新旧节点的子节点提出来做比较

时间复杂度：所有的节点按层级比较，只会遍历一次，O(n)

### 26、单例怎么实现

单例模式，也叫单子模式，是一种常用的软件设计模式。单例对象的类必须保证只有一个实例存在。许多时候整个系统只需要拥有一个的全局对象，这样有利于我们协调系统整体的行为。

<img src="D:\typora笔记\面试必看\面试复习题.assets\image-20201202114353419.png" alt="image-20201202114353419" style="zoom:67%;" />

### 27、let 、const和var的区别

一）var声明变量存在变量提升，let和const不存在变量提升；var定义的变量，没有块的概念，可以跨块访问，不能跨函数访问。

二）let、const都是块级局部变量，且使用const声明变量需要赋初始值，同时用const声明的变量如果是简单数据类型，那么只能进行一次赋值，即声明后不能再修改；如果声明的是复合类型数据，可以修改其属性

三）同一作用域下let和const不能声明同名变量，而var可以

### 28、作用域与作用域链

作用域是在运行时代码中的某些特定部分中变量，函数和对象的可访问性。换句话说，作用域决定了代码区块中变量和其他资源的可见性。作用域最大的用处就是隔离变量，不同作用域下同名变量不会有冲突。

当我们取一个变量的值的时候，在当前作用域查询不到，就会向上一级作用域去查，直到查到全局作用域，这么一个查找过程形成的链条就叫做作用域链。

### 29、JavaScript继承方式

**原型链继承**

基本思想：利用原型让一个引用类型继承另一个引用类型的属性和方法，即让原型对象等于另一个类型的实例；

**构造函数继承**

基本思想：在子类型构造函数的内部调用超类型构造函数，通过使用apply()和call()方法可以在将来新创建的对象上执行构造函数；

**组合继承**

基本思想：将原型链和借用构造函数技术组合到一起。使用原型链实现对原型属性和方法的继承，用借用构造函数模式实现对实例属性的继承。这样既通过在原型上定义方法实现了函数复用，又能保证每个实例都有自己的属性；

**原型式继承**

基本思想：不用严格意义上的构造函数，借助原型可以根据已有的对象创建新对象；

ES5新增Object.create规范了原型式继承，接收两个参数，一个用作新对象原型的对象和（可选的）一个为新对象定义额外属性的对象，在传入一个参数的情况下，Object.create()和object()行为相同。

**寄生式继承**

基本思想：寄生式继承是与原型式继承紧密相关的一种思路，它创造一个仅用于封装继承过程的函数，在函数内部以某种方式增强对象，最后再返回对象。

**寄生组合式继承**

基本思想：通过借用构造函数来继承属性，通过原型链的混成形式来继承方法，不必为了指定子类型的原型而调用超类型的构造函数，只需要超类型的一个副本。本质上，就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型

### 30、instanceof的原理

instanceof 主要的实现原理就是只要右边变量的 prototype 在左边变量的原型链上即可。因此，instanceof 在查找的过程中会遍历左边变量的原型链，直到找到右边变量的 prototype，如果查找失败，则会返回 false
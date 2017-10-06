# 前端面试
## 前言
### 从几道面试题开始
- JS中使用 **typeof** 能得到哪些类型？ 
	- 考点：**js变量类型**
- 何时用 **==** 何时用 **===**?
	- 考点：**强制类型转换**
- **window.onload** 和 **DOMContentLoaded** 的区别？
	- 考点：**浏览器渲染过程**
- 用JS创建10个`<a>`标签，点击的时候弹出相应的序号。
	- 考点：**作用域** 
- 简述如何实现一个**模块加载器**，实现类似**require.js**的基本功能。
	- 考点：**JS模块化** 
- 实现数组的**随机排序**
	- 考点：**JS基础算法**	 

## JS基础知识（上）

### 变量类型与计算

#### 问题
- JS中使用 **typeof** 能得到哪些类型？
- 何时用 **==** 何时用 **===**?
- JS有哪些内置函数？
- JS按照存储方式区分为哪些类型，并描述其特点。
- 如何理解 **JSON**？

#### 知识点

##### 变量类型
- **值类型** vs **引用类型**
	- 值类型：直接将值存在变量的内存空间里面
	- 引用类型：另外开辟内存空间，将那个空间的地址存在变量里面。(为了共用内存空间)
- **typeof** 运算符详解

	``` javascript
	typeof undefined //undefined
	typeof 'abc' //string
	typeof 123 //nubmer
	typeof true //boolean
	typeof {} //object
	typeof [] //object
	typeof null //object
	typeof console.log //function
	```
	- **typeof**只能区分值类型的详细类型，不能区分函数之外的引用类型的详细类型。可以返回的值为`undefined `,`string `,`nubmer `,`boolean `,`object `,`function`。

##### 变量计算
- 强制类型转换
	- 字符串拼接
	- == 运算符
	- 逻辑运算符

#### JS内置函数 -- 数据封装类对象
- Object
- Array
- Boolean
- Number
- String
- Function
- Date
- RegExp
- Error

#### JSON 是一个JS对象
- JSON.stringify({a: 1})将JSON转化成字符串。
- JSON.parse('{"a": 1}')将字符串按照JSON的格式解析。

### 原型和原型链

#### 题目

- 如何准确判断一个变量是**数组类型**
- 写一个原型链继承的例子
- 描述 **new** 一个对象的过程
- zepto（或其他框架）源码中如何使用原型链

#### 知识点
- 构造函数
- 构造函数 - 扩展
- 原型规则和示例
- 原型链

#### 构造函数 - 扩展
- var a = {} 其实是 var a = new Object() 的语法糖
- var a = [] 其实是 var a = new Array() 的语法糖
- function Foo () {...} 其实是 var Foo = new Function(...)
- 使用 instanceof 判断一个函数是否是一个变量的构造函数

#### 原型规则和示例
- 所有的引用类型（数组，对象，函数），都具有对象特性，即可自由扩展属性（除了null以外）
- 所有的引用类型（数组，对象，函数），都有一个`_proto_`属性，属性值是一个普通的对象（除了null以外）
- 所有的函数，都有一个 prototype 属性，属性值也是一个普通的对象
- 所有的引用类型（数组，对象，函数），`_proto_`属性值指向它的构造函数的 "prototype" 属性值。
- 当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的 `_proto_`（即它的构造函数的 prototype ）中寻找。

#### 解题
- 如何准确判断一个变量是**数组类型**
	- Array.isArray(arr) === true
	- arr instanceof Array === true
	- arr.constructor === Array (但是构造器是可以被改变的，所以并不一定准确)
	- Object.prototype.toString.apply(arr) === '[object Array]' 
- 写一个原型链继承的例子
	- 略
- 描述 **new** 一个对象的过程
	- 创建一个新对象
	- this 指向这个对象
	- 执行代码，即对this赋值
	- 返回 this
	
#### 写一个贴近实际开发原型链继承
- 写一个封装DOM查询的例子

``` javascript
function Elem(id){
    this.elem = document.getElementById(id)
}
Elem.prototype.html = function (val) {
    var elem = this.elem;
    if(val){
        elem.innerHTML = val
        return this //链式操作
    } else {
        return elem.innerHTML
    }
} 
Elem.prototype.on = function (type, fn){
    var elem = this.elem;
    elem.addEventListener(type,fn)
}
var div1 = new Elem ('div1')
```
``` javascript
class Elem {
    constructor(id){
        this.elem = document.getElementById(id)
    };
    html (val) {
        var elem = this.elem;
        if(val){
            elem.innerHTML = val
            return this //链式操作
        } else {
            return elem.innerHTML
        }
    };
    on(type, fn){
        var elem = this.elem;
        elem.addEventListener(type,fn)
    }
}
var div1 = new Elem ('div1')
```

## JS基础知识（下）

### 异步和单线程

#### 题目
- 同步和异步的区别是什么？分别举一个同步和异步的例子
- 一个关于 setTimeout 的笔试题
- 前端使用异步的场景有哪些

#### 知识点
- 什么是异步（对比同步）
- 前端使用异步的场景
- 异步和单线程 

#### 何时需要异步
- 在可能需要等待的情况
- 等待过程中不能像 alert 一样阻塞程序运行
- 因此，所以 “等待的情况” 都需要异步

#### 前端使用异步的场景
- 定时任务：setTimeout，setInverval
- 网络请求：ajax 请求，动态`<img>`加载
- 事件绑定

#### 异步和单线程

``` javascript
console.log(1);
setTimeout(function(){
	console.log(2);
});
console.log(3);
``` 
- 执行第一行，打印 1
- 执行setTimeout后，传入setTimeout的函数会被暂存起来，不会立即执行（单线程的特点，不能同时干两件事）
- 执行最后一行，打印300
- 待所有程序执行完，处于空闲状态时，会立马看有没有暂存起来的要执行。
- 发现暂存起来的setTimeout中的函数无需等待时间，就立即拿过来执行

#### 同步和异步的区别是什么？分别举一个同步和异步的例子
- 同步会阻塞代码执行，而异步不会
- alert是同步，setTimeout是异步

### 其他知识
#### 问题
- 获得2017-06—10格式的日期
- 获得随机数，要求是长度一致的字符串格式
- 写一个能遍历对象和数组的通用 forEach 函数

#### 日期
```
Date.now() //获取当前时间毫秒数
var dt = new Date() 
dt.getTime() //获取毫秒数
dt.getFullYear() //年
dt.getMounth() //月(0 - 11)
dt.getDate() //日(0 - 31) 
dt.getHours() //小时(0 - 23)
dt.getMinutes() //分钟(0 - 59)
dt.getSeconds() //秒(0 - 59)
```

#### Math
- 获取随机数 Math.random() 返回一个0 ~ 1的随机数

#### 数组API
- forEach 遍历所有元素
- every 判断所有元素是否都符合条件
- some 判断是否有至少一个元素符合条件
- sort 排序
- map 对元素重新组装，生成新数组
- felter 过滤符合条件的元素

#### 对象API
- for in
- hasOwnProperty 是否是私有属性

#### 解答
- 获得2017-06—10格式的日期

``` javascript
function formatDate(dt){
    var dt = dt || new Date();

    var year = dt.getFullYear();
    var month = dt.getMonth() + 1;
    var date = dt.getDate();
    if (month < 10) {
        month = "0" + month;
    }
    if(data < 10) {
        date = "0" + month;
    }
    return year + '-' + month + '-' + date
}

var dt = new Date();
console.log(formatDate(dt))
```

- 获得随机数，要求是长度一致的字符串格式

``` javascript
var random1 = Math.random()
var random2 = random + '0000000000'
var random3 = random2.slice(0, 10)
console.log(random3)
```

- 写一个能遍历对象和数组的通用 forEach 函数

``` javascript
function forEach(obj, fn){
	var key;
	if(obj instanceof Array){
		obj.forEach(function (item, index){
			fn(item, index)
		})
	} else {
		for(key in obj){
			fn(obj[key], key)
		}
	}
}
```

## JS-Web-API（上）

### DOM操作（Document Object Model）
- DOM 是哪种基本的数据结构
	- 树
- DOM 操作常用的API有哪些
	- 获取DOM节点，以及节点的 property 和 Attribute
	- 获取父节点，获取子节点
	- 新增节点，删除节点
- DOM 节点的 attr 和 property 有什么区别
	- property 只是一个js对象的属性的修改
	- Attribute 是对html标签属性的修改 
#### DOM节点操作
- 获取DOM节点
	- **getElementById("id")**
	- **getElementsByClassName("class")** 类数组
	- **getElementsByTagName("div")** 类数组
	- **getElementsByName("name")** 只有部分标签的name可以生效，比如表单、表单元素、img、iframe等
	- **querySelector('div p #demo .demo')** css选择器的写法,querySelector永远选择一组里面的第一个，所以返回的不是一个类数组而是一个具体的元素。
	- **querySelectorAll('div p #demo .demo')** 一个类数组的集合的话，那么就用querySelectorAll()方法。
	- 两个方法的问题在于，他们返回的不像前面四个是一个实时改变的元素，而是一个副本。当我们用这两个方法选择出来元素之后，我们把本身那个元素修改一下，会发现我们选择出来的那个元素没有变化。
- property
	- nodeType
	- nodeName
- Attribute
	- ...

#### DOM结构操作
- 新增节点
- 获取父节点
- 获取子节点
- 删除节点

### BOM操作（Browser Object Model）
### 题目
- 如何检测浏览器的类型
- 拆解url的各部分

### 知识点
- navigator
- screen
- location
- history

### navigator & screen
``` javascript
// navigator
var isChrome = ua.indexOf('Chrome')
console.log(isChrome)

// screen
console.log(screen.width)
console.log(screen.height)
```

### location & history
``` javascript
//location
console.log(location.href)
console.log(location.host)
console.log(location.protocol) //'http:' 'https:'
console.log(location.pathname) // '/learn/199'
console.log(location.search)
console.log(location.hash)

//history
history.back()
history.forward()
```

### 事件
#### 题目
- 编写一个通用的事件监听函数
- 描述事件冒泡流程
- 对于一个无限下拉加载的页面，如何给每个图片绑定事件

#### 知识点
- 通用事件绑定
- 事件冒泡
- 代理

### Ajax
#### 题目
- 手动编写一个ajax,不依赖第三方库
- 跨域的几种实现方式

#### 知识点
- XMLHttprequest
- 状态码说明
- 跨域

#### XMLHttpRequest

``` javascript
function AJAX(json) {
	var url = json.url,
		method = json.method,
		flag = json.flag,
		data = json.data,
		callBack = json.callBack,
		xhr = null;
	if(window.XMLHttpRequest) {
		xhr = new window.XMLHttpRequest();
	}else {
		xhr = new ActiveXObject('Mircosoft.XMLHTTP');
	}			
	if(method == 'get') {
      	url += '?' + data + new Date().getTime(); 
		xhr.open('get', url, flag);
	}else {
		xhr.open('post', url, flag);
	}
	xhr.onreadystatechange = function () {
		if (xhr.readyState === 4 && xhr.status === 200) {
			// 数据已经可用了
			callBack(xhr.responseText);
		}
	}
	if(method == 'get') {
		xhr.send();
	}else {
		xhr.setRequestHeader('Content-Type', 'application/x-www-form-urle');
		xhr.send(data);
	}	
}
```

#### readyState说明
- 0（未初始化），此时已经创建了一个XMLHttpRequest对象，还没有调用send()
- 1（载入），已调用send()方法，正在发送请求
- 2（载入完成）send()方法执行完成，已经接收到全部响应内容
- 3（交互）正在解析响应内容
- 4（完成）响应内容解析完成，可以在客户端调用了

#### 状态码说明（status）
- 2xx 表示成功处理请求，如 200
- 3xx 需要重定向，浏览器直接跳转
- 4xx 客户端错误，如404
- 5xx 服务端错误

### 跨域
- 浏览器有同源策略，不允许 ajax 访问其他域接口
- 跨域条件：协议、域名、端口，有一个不同就算跨域

#### 可以跨域的三个标签
- `<img src = xxx>`
- `<link href = xxx>`
- `<script src = xxx>`

#### 三个标签的场景
- `<img>`用于打点统计，统计网站可能是其他域
- `<link>``<script>`可以使用CDN，CDN也是其他域
- `<script>`可以用于JSONP

#### 跨域注意事项
- 所有的跨域请求都必须经过信息提供方的允许
- 如果未经允许即可获取，那是浏览器同源策略出现漏洞

#### JSNP实现原理

### 存储
#### 题目
- cookie和sessionStorage、localStorage有什么区别

#### 知识点
- cookie
- sessionStorage、localStorage

#### cookie
- 本身是用于客户端与服务端通信
- 但是它有本地存储的功能，于是被借用
- 使用document.cookie = ... 获取和修改即可

#### cookie用于存储的缺点
- 存储太小，只有4kb
- 所有的http请求都会带着，会影响获取资源的效率
- API简单，需要封装才能用 document.cookie = ... 

#### sessionStorage、localStorage
- HTML5专门为了存储而设计，最大容量为5MB
- API简单易用：localStorage.setItem(key, value);localStorage.getItem(key)
- IOS safari 隐藏模式下，getItem(key)会报错
- 建议使用try-catch封装

## Git
### 常用Git命令
- git add .
- git commit -m "xxx"
- git push origin master
- git pull origin master
- git branch 
- git checkout -b xxx/git checkout xxx
- git merge xxx

## 模块化
- 这就是一个面试的问题
### 知识点
- 不使用模块化的情况
- 使用模块化的情况
- AMD
- CommonJS

### 不使用模块化
- util.js getFormatDate函数
- a-util.js aGetFormatDate函数 使用getFormatDate
- a.js 使用aGetFormatDate

``` javascript
//util.js
funcvtion getFormatDate(date, type) {
	//......
}

//a-util.js
function aGetFormatDate(data) {
	return getFormatDate(data, 2)
}

//a.js
var dt = new Date();
console.log(aGetFormatDate(dt))

//html里
<script src="util.js"></script>
<script src="a-util.js"></script>
<script src="a.js"></script>
//这些代码中的函数必须是全局变量，才能暴露给使用方。全局变量污染
//a.js 知道要引用 a-util.js, 但是他知道还需要依赖于 util.js 吗？
```

### AMD
- require.js
- 全局define函数
- 全局require函数
- 依赖JS会自动、异步加载

``` javascript
//util.js 
define(function () {
	return {
		getFormatDate: function(data, type) {
			//...
		}
	}
})

//a-util.js
define(['./util.js'], function (util) {
	return {
		aGetFormatDate: function(data) {
			return util.getFormatDate(data, 2)
		}
	}
})

//a.js
define(['./a-util.js'], function (aUtil) {
	return {
		printDate: function (date) {
			console.log(aUtil.aGetFormatDate(data))
		}
	}
})

//main.js
require(['./a.js'], function (a) {
	var data = new Date();
	a.printDate(date);
}) 

//html使用
<script src="require.min.js" data-main="./main.js"></script>
```

### CommonJS
- node.js模块化规范，现在被大量用于前端，原因：
- 前端开发依赖的插件和库，都可以从 npm 获取
- 构建工具的高度自动化，使得使用 npm 的成本非常低
- CommonJS 不会异步加载JS，而是同步一次性加载出来
- 需要构建工具支持
- 一般和 npm 一起使用

``` javascript
//util.js
module.exports = {
	getFormatDate: function (date, type) {
		//...
	}
}

//a-util.js
var util = require('util.js');
module.exports = {
	aGetFormatDate: function (date) {
		return util.getFormatDate(date, 2);
	}
}
``` 

### AMD 和	CommonJS的使用场景
- 需要异步加载JS，使用AMD
- 使用 npm 之后，建议使用CommonJS

### 重点总结
- AMD
- CommonJS
- 两者的区别

## 构建工具
- grunt
- gulp
- fis3(百度出的)
- webpack

## 运行环境
### 前言
- 浏览器可以通过访问链接来得到页面的内容
- 通过绘制和渲染，显示页面的最终的样子
- 整个过程中，我们需要考虑什么问题？ 

### 页面加载

#### 问题
- 从输入url到得到html的详细过程
- **window.onload** 和 **DOMContentLoaded** 的区别？

#### 知识点
- 加载资源的形式
- 加载一个资源的过程
- 浏览器渲染页面的过程 

#### 加载资源的形式
- 输入url（或跳转页面）加载 html
- 加载 html 中的静态资源

#### 加载一个资源的过程
- 浏览器根据 DNS 服务器得到域名的 IP 地址
- 向这个 IP 的机器发送 http 请求
- 服务器收到、处理并返回 http 请求
- 浏览器得到返回内容

#### 浏览器渲染页面的过程
- 根据 HTML 结构生成 DOM Tree
- 根据 CSS 生成 CSSOM
- 将 DOM 和 CSSOM 整合形成 RenderTree
- 遇到 `<script>` 时，会执行并阻塞渲染

### 性能优化

#### 原则 
- 多使用内存，缓存或者其它方法
- 减少CPU计算，减少网络
- 用户体验方面的优化

#### 加载资源优化
- 静态资源的压缩合并
- 静态资源的缓存
- 使用 CDN 让资源加载更快
- 使用服务端渲染，数据直接输出到 HTML 中

#### 渲染优化
- CSS 放前面，JS 放后面
- 懒加载（图片懒加载、下拉刷新更多）
- 减少 DOM 查询，对DOM查询做缓存
- 减少 DOM 操作，多个操作尽量合并在一起执行
- 事件节流 （利用计时器做一些延时操作）
- 尽早执行操作 （如 DOMContentLoaded）

### 安全性

#### 知识点
- XSS 跨站请求攻击
- XSRF 跨站请求伪造

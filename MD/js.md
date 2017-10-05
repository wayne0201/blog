# HTML、CSS
1. 盒模型的宽高值计算方式？
	
		![](https://github.com/lj614418910/blog/blob/master/images/20170321180119900.png)
	
		![](https://github.com/lj614418910/blog/blob/master/images/20170321180159166.png)
		
2. 边界合并，边界塌陷？

	- 边界合并是指上下两个元素的外边距重合在一起。
	- 边界塌陷是指父子级的两个元素的外边距重合在一起了。

3. 标准模式与怪异模式区别？

	- 在标准模式页面按照HTML,CSS的定义渲染，而在怪异模式就是浏览器为了兼容很早之前针对旧版本浏览器设计，并未严格遵循W3C标准而产生的一种页面渲染模式。浏览器基于页面中文件类型描述的存在以决定采用哪种渲染模式，如果存在一个完整的DOCTYPE则浏览器将会采用标准模式，如果缺失就会采用怪异模式。下面介绍标准模式和怪异模式之间的区别。
	- 二者最主要区别是盒模型的不同。
	
		- ![](https://github.com/lj614418910/blog/blob/master/images/20170321180119900.png)
	
		- ![](https://github.com/lj614418910/blog/blob/master/images/20170321180159166.png)	

	- 图片元素的垂直对齐方式：对于inline元素和table-cell元素，标准模式下vertical-align属性默认取值为baseline，在怪异模式下，table单元格中的图片的vertical-align属性默认取值为bottom，因此在图片底部会有及像素的空间。
	- `<table>`元素中的字体：CSS中，对于font的属性都是可以继承的，怪异模式下，对于table元素，字体的某些元素将不会从body等其他封装元素中继承得到，特别是font-size属性。
	- 内联元素的尺寸：标准模式下，non-replaced inline元素无法自定义大小，怪异模式下，定义这些元素的width，height属性可以影响这些元素显示的尺寸。
	- 元素的百分比高度：
		- CSS中对于元素的百分比高度规定如下：百分比为元素包含块的高度，不可为负值，如果包含块的高度没有显示给出，该值等同于auto，所以百分比的高度必须在父元素有高度声明的情况下使用。
		- 当一个元素使用百分比高度时，标准模式下，高度取决于内容变化，怪异模式下，百分比高度被正确应用。
	- 元素溢出的处理：标准模式下，overflow取默认值visible，在怪异模式下，该溢出会被当做扩展box来对待，即元素的大小由其内容决定，溢出不会裁减，元素框自动调整，包含溢出内容。
	- box-sizing的概念？
		- box-sizing 属性允许您以特定的方式定义匹配某个区域的特定元素，默认值是content-box。还可以设置为box-sizing。
		- content-box：
			- padding和border不被包含在定义的width和height之内。对象的实际宽度等于设置的width值和border、padding之和，即 ( Element width = width + border + padding)
			- 此属性表现为标准模式下的盒模型。
		- border-box：
			- padding和border被包含在定义的width和height之内。对象的实际宽度就等于设置的width值，即使定义有border和padding也不会改变对象的实际宽度，即 ( Element width = width )
			- 此属性表现为怪异模式下的盒模型。

4. 简要说下BFC(Block Formatting Context)是什么？有哪些应用？

	- bfc全称是block format context——块级格式化上下文
	- 浮动元素、绝对定位元素，不是块级盒的块级包含块(比如inline-block、table-cell、table-capation)和overflow值不为visible的块级盒子为它们的内容建立了一个新的块级排版上下文。
	- 在一个块级排版上下文中，盒子是从包含块顶部开始，垂直的一个接一个的排列的，相邻两个盒子之间的垂直的间距是被margin属性所决定的，在一个块级排版上下文中相邻的两个块级盒之间的垂直margin是折叠的。参与BFC的布局的只有普通流normal flow中的块级盒，而float、position值不为relative\static的元素是脱离BFC这一布局环境的，不参与BFC的布局
	- 在一个块级排版上下文中，每个盒子的左外边是触碰到包含块的左边的（对于从右向左的排版，则相反），即使在有浮动元素参与的情况下也是如此(即使一个盒子的行盒是因为浮动而收缩了的)，除非这个盒子新建了一个块级排版上下文(在某些情况下这个盒子自身会因为floats而变窄)。
	- 应用：
		- 边距合并
		- 浮动的高度塌陷问题
		- 让文字环绕块级元素的方法
		- margin的折叠 

5. 要求大容器宽度自由伸缩的情况下，A/B/C三个元素的宽度比始终是1:1:1，如何实现？
	- 弹性布局:

		```
		.wrapper{
		    display: flex;
		}
		.A,.B,.C{
		    flex: 1;
		    height: 100px;
		    border: 1px solid red;
		}
		```
		```
		.wrapper{
			font-size: 0;
			width:100%;		
      	}
		.A,.B,.C{
		   display: inline-block;
			width:33.3%;
			box-sizing: border-box;
			height: 100px;
			border: 1px solid red;
		}//inline-block记得要把元素之间的空格清掉font-size: 0;
		```
		```
		 .wrapper{
            width:100%;
            overflow: hidden;
        }
        .A,.B,.C{
            float: left;
            width:33.3%;
            box-sizing: border-box;
            height: 100px;
            border: 1px solid red;
        }//这种方法记得清除浮动
		```

6. 居中
	- 已知子元素宽高:100px;
	
	```
	.wrapper{
		position: relative;
	}
	.A{
		position: absolute;
		left: 50%;
		top: 50%;
		margin-left: -50px;
		margin-top: -50px;
		height: 100px;
		width: 100px;
	}
	```
	```
	.wrapper{
	    position: relative;
	}
	.A{
	    position: absolute;
	    left: 0;
	    top: 0;
	    bottom: 0;
	    right: 0;
	    margin: auto;
	    height: 100px;
	    width: 100px;
	}
	```
	- 宽高未知

	```
	.wrapper{
		position: relative;
	}
	.A{
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate3d(-50%, -50%, 0);
	}
	```
	```
	.wrapper{
		display: flex;
		justify-content: center;
		align-items: center;
	}
	```
	```
	.wrapper{
        text-align: center;
    }
    .A{
        display: inline-block;
        vertical-align: middle;
        background: red;
    }
    .null{
        display: inline-block;
        vertical-align: middle;
        width: 0;
        height: 100%;
    }//null为添加一个行级块元素给.A用于垂直对齐的。
	```

	
# js
1. **柯里化**
	- 柯里化又称部分求值。一个柯里化的函数首先会接受一些参数，接受这些参数之后，函数并不会立即求值，而是继续返回另一个函数，刚才传入的参数在函数形成的闭包中被保存起来。待到函数被真正需要求值的时候，之前传入的所有参数都会被一次性用于求值。

2. **高阶函数**
	- JavaScript的函数其实都指向某个变量。既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。

3. **this的四种用法**
	- 在一般函数方法中使用 this 指代全局对象。
	- 作为对象方法调用，this 指代上级对象（谁调用指向谁）。
	- 作为构造函数调用，this 指代new 出的对象
	- apply、call 调用 ，apply、call方法作用是改变函数的调用对象，此方法的第一个参数为改变后调用这个函数的对象，this指代第一个参数

4. **Ajax原理**
	- Ajax主要依靠两个对象，**new XMLHttpRequest()**和**new ActiveXObject(‘Microsoft.XMLHTTP’)**。一般主流浏览器支持前一个方法，IE6以下用第二个方法。
	- 对象方法
	 	* **open(‘method’, ‘url’, ‘true’):** 建立对服务器的调用，method参数可以是get、post或者put。url参数可以是相对url或者绝对url，第三个参数是选择异步还是同步，异步是true。
	 	* **send(content):** 向服务器发送请求。
	 	* **setRequestHeader(‘label’, ‘value’):** 把指定首部设置为提供的值，**在设置任何首部之前必须调用open方法。** 
	- 对象的属性
	 	* **onreadystatechange** 状态改变触发器,给其赋值一个函数，每次状态改变的时候都会执行一次这个函数。
	 	* **readyState** 对象的状态
	 		- 0代表未初始化，此时已经创建了一个XMLHttpRequest对象。
	 		- 1代表读取中，此时代码已经调用XMLHttpRequest的open方法并且XMLHttpRequest已经将请求发送到服务期。
	 		- 2代表已读取，此时已经通过open方法把一个请求发送到服务端，但是还没有收到。
	 		- 3代表交互中，此时已经收到http响应头部信息，但是消息主体部分还没有完全接受。
	 		- 4代表完成，此时响应已经被完全接受。
	 	* **status** 服务器返回的状态码(200为请求成功)。
	 	* **responseText** 服务器进程返回数据的文本版本。
	 
	```
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

5. **jsonp原理**
	- web页面上script引入js文件不受跨域的影响，不仅如此，凡是有src属性的标签都不受同源策略的影响。
	- 但是无法监控script的src的加载状态，不知道数据有没有获取完成，所以我们要事先定义好处理函数。
	- 然后在数据里面开头写上这个函数名，引入进来之后等到文件全部加载完毕，刚好就是调用我们已经定义好的函数，并且参数就是后面的那一串数据。

6. **数组方法**
	- **concat()** 方法用于连接两个或多个数组；该方法不会改变现有的数组，而是返回被连接后的新数组。也可以用es6的展开运算符...。**[...arr1,...arr2,...arr3]**
	- **join()** 方法用于把数组中的所有元素放入一个字符串。元素是通过指定的分隔符进行分隔的。**split()** 方法用于把一个字符串分割成字符串数组。如果把空字符串 ("") 用作第一个参数，那么每个字符之间都会被分割。
	- **slice()** 方法可从已有的数组中返回选定的元素。(不会修改原数组)
		* 第一个参数：必需。规定从何处开始选取。如果是负数，那么它规定从数组尾部开始算起的位置。也就是说，-1 指最后一个元素，-2 指倒数第二个元素，以此类推。
		* 第二个参数：可选。规定从何处结束选取。该参数是数组片断结束处的数组下标。如果没有指定该参数，那么切分的数组包含从 start 到数组结束的所有元素。如果这个参数是负数，那么它规定的是从数组尾部开始算起的元素。
		* 返回一个新的数组，包含从 start 到 end （不包括该元素）的 arrayObject 中的元素。
	- **reverse()** 方法用于颠倒数组中元素的顺序。该方法会改变原来的数组，而不会创建新的数组。
	- **push()** 方法可向数组的末尾添加一个或多个元素，并返回新的长度。
	- **pop()** pop() 方法用于删除并返回数组的最后一个元素。
	- **shift()** 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值。
	- **unshift()** 方法可向数组的开头添加一个或更多元素，并返回新的长度。
	- **splice()** 方法可删除从index处开始的零个或多个元素，并且用参数列表中声明的一个或多个值来替换那些被删除的元素。如果从数组中删除了元素，则返回的是含有被删除的元素的数组。(会修改原数组)
	- **sort()** 方法用于对数组的元素进行排序。第一个参数是需要传一个函数。
		* 该函数要比较两个值，然后返回一个用于说明这两个值的相对顺序的数字。比较函数应该具有两个参数 a 和 b，其返回值如下：
<br/>1）若 a 小于 b，在排序后的数组中 a 应该出现在 b 之前，则返回一个小于 0 的值。
<br/>2）若 a 等于 b，则返回 0。
<br/>3）若 a 大于 b，则返回一个大于 0 的值。
		* 因为数字比较的时候会选第一位数字比较，多位数的时候会不准。所以要修改一下里面的回调函数。
	
	```
	arr.sort(function sortNumber(a,b){
		return a - b
	})
	```
	
7. **类数组转化成数组**
	- **Array.from(arrayLink, [mapfn],[thisArg])** 从一个类似数组或可迭代的对象中创建一个新的数组实例。第一个参数是想要转变成真实数组的类数组对象或可遍历对象，第二个参数是最后生成的数组会经过该函数的加工处理后返回。第三个参数是执行第二个参数函数时的this指向
	- **Array.prototype.slice.call(arrayLink)**, 返回一个新数组，不改变原对象。
	- **Array.prototype.splice.call(arrayLink, 0)**, 返回一个新数组，但是会删掉原对象的值。

8. 数组去重

	```
	Array.prototype.unique = function () {
	      var len = this.length,
	      arr = [],
	      obj = {};
	      for (var i = 0; i < len; i++) {
	            if (!obj[this[i]]) {
	                  obj[this[i]] = 1;
	                  arr.push(this[i]);
	            }
	      }
	      return arr;
	}
	```

9. 深度克隆

	```
	function deepCopy(src,tar){
	      var tar= tar|| {} ;
	      for (var prop in src){
	            if (typeof (src[prop]) == 'object'){
	                   tar[prop] = (src[prop] instanceof Array ) ? [] : {};
	                  deepCopy(src[prop],tar[prop]);
	            }else {
	                   tar[prop] = src[prop];
	            }
	      }
	 
	      return tar;
	}
	``` 
	
10. assign实现
	- Object.assign()实现（浅层克隆）

		```
		Object.prototype._assign = function(...arr){
			var _self = arr.shift() || {}
			arr.forEach(function(item){
				for(var prop in item){
					if(item.hasOwnProperty(prop)){
						_self[prop] = item[prop]
					}
				}
			})
			return _self;
		}
		```
	- Object.assign()实现（深层克隆）
	
		```
		Object.prototype._assignCopy = function(...arr){
			var _self = arr.shift() || {}
			arr.forEach(function(item){
		    	for(var prop in item){
		        	if(item.hasOwnProperty(prop)){
		            	if(typeof (item[prop]) == 'object'){
		                	_self[prop] = ((_self[prop]) instanceof Array)? []:{}
		                	Object._assignCopy( _self[prop],item[prop])
		            	}else{
		                	_self[prop] = item[prop]
		            	}
		        	}
		        
				}
			})
			return _self;
		}
		```
	
11. 实现bind函数

	```
	Function.prototype.bind = function (that){
	    var _this = this;
	    var slice = Array.prototype.slice;
	    var _arg = slice.call(argument, 1)
	    return function(){
	        return _this.apply(that, _arg)
	    }
	}
	```
12. class和prototype的区别

	![](https://github.com/lj614418910/blog/blob/master/images/20160314212504_39150.png)
	
	![](https://github.com/lj614418910/blog/blob/master/images/20160116201909_44777.png)
	
13. filter实现

	```
	Array.prototype._filter = function (callback,thisVal){
		var _self = thisVal || this;
		var _arr = [];
		_self.forEach(function(item, index, arr){
		    if(callback(item,index,arr)){
			_arr.push(item);
		    }
		})
		return _arr;
	}
	```

	map实现
	
	```
	Array.prototype._map = function (callback,thisVal){
		var _self = thisVal || this;
		var _arr = [];
		_self.forEach(function(item, index, arr){

			_arr.push(callback(item,index,arr));
		})
		console.log(_arr);
		return _arr;
	}
	```
	
14. jsop实现
	- 前端封装函数
	
		```
		function jsonp(url,data, callbackName){
			var url = url || "http://localhost:8080";
			var callbackName = callbackName || "callback";
			var data = data || {};
			if(~url.indexOf('?')){
				url = `${url}&callback=${callbackName}`;
			}else{
				url = `${url}?callback=${callbackName}`;
			}
			for(var prop in data){
				url = `${url}&${prop}=${data[prop]}`;
			}
			var $Script = document.createElement('script');
			$Script.type = 'text/javascript';
			$Script.src = url;
			document.body.appendChild($Script);	
		}
		
		window.callback = function (data){
			console.log(data);
		}
		
		jsop('http//....', {a:'a',b:'b'}, 'callback');
		``` 
	- node后端封装


		```
		const http = require('http');
		const url = require('url');
		var data = {name:'li',year:18};
		
		let hp = http.createServer(function(req, res){
			let obj = url.parse(req.url, true);
		
			if(obj.pathname === "/"){
				res.end(obj.query.callback + '(' + JSON.stringify(data) +')');
			}
		});
		hp.listen(8080);
		```	

15. find函数实现

	```
	function findIt(obj, str){
		var Obj = obj;
		var flag = true;
		var arr = str.split('.');
		arr.forEach(function(val, index){
			if(Obj[val]){
				Obj = Obj[val];
			}else{
				flag = false;
				return false;
			}
		});
		if(flag){
			return Obj;
		}else{
			return undefined;
		}
	}
	```
16. 两个栈生成一个队列

	```
	var arr1 = [1,2,3,4,5];
	var arr2 = []
	Array.prototype.enqueue = function(ele){
		this.push(ele);
	}
	Array.prototype.dequeue = function(){
		var len1 = this.length;
		for(let i = 0; i < len1; i++){
			arr2.push(this.pop());
		}
		var tar = arr2.pop();
		var len2 = arr2.length;
		for(let i = 0; i < len2; i++){
			this.push(arr2.pop());
		}
		return tar;
	}
	arr1.enqueue(6);
	arr1.dequeue();
	console.log(arr1);
	```

17. 针对移动端开发，不期望用户放大屏幕，只要求“视口(viewport)”宽度等于设备(屏幕)宽度，视口高度也等于设备（屏幕）高度，如何设置？
	- `<meta name = "viewport" content = "width=device-width,height=device-height,initial-scale = 1.0,maximum-scale = 1.0">`
	- meta viewport有6个属性：width,innitial-scale,minimum-scale,maimum-scale,height,user-scalable。
分别为设置layout viewport的宽度、页面初始缩放值、允许用户的最小缩放值、允许用户的最大缩放值、layout viewport的高度，是否允许用户缩放。

18. `data-`属性的作用是什么？
	- 存储页面或应用程序的私有自定义数据。
	- 可通过.dataset来获取和设置。
	- .dataset有兼容性问题，所以可以用getAttribute/setAttribute方法来获取和设置。

19. 请简述一下cookie、localStorage、sessionStorage的区别？
	- 共同点：都是保存在浏览器端，且同源的。
	- 区别：
	- cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递。而sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。
	- 存储大小限制也不同，cookie数据不能超过4k，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。
	- 数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。
	- 作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localStorage 在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的。
	- Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。
	- Web Storage 的 api 接口使用更方便。

20. 使用jquery，找到id为'selector'的select标签中拥有'data-target'属性，且值为'isme'的option的值。
	```
	var value;
	$('select#selector option').each(function(){
	    if ($(this).data('target') == 'isme') {
	        value = $(this).val();
	    }
	});
	```
	```
	var value = $('#selector option[data-target="isme"]').val();
	```

21. 请优化以下代码
	
	```
	for (var i = 0; i < document.getElementsByTagName('a').length; i ++) {
		document.getElementsByTagName('a')[i].onmouseover = function() {
			this.style.color = 'red';
		}
		document.getElementsByTagName('a')[i].onmouseout = function() {
			this.style.color = '';
		}
	}
	```
	```
	//优化后
	var elems = document.getElementsByTagName('a');
    var len = document.getElementsByTagName('a').length;

    for (var i = 0; i < len; i ++) {
        elems[i].onmouseover = function() {
            this.style.color = 'red';
        }
        elems[i].onmouseout = function() {
            this.style.color = '';
        }
    }
	```
	
22. 设计一个算法，将两个有序数组，合并为一个有序数组，请不要使用concat以及sort方法。

	```
	function merge(arr1, arr2){
        var arr = [];
        while(arr1.length != 0 || arr2.length != 0){
            if(arr1[0] == undefined){
                arr.push(arr2.shift());
            }else if(arr2[0] == undefined) {
                arr.push(arr1.shift());
            }else{
                if(arr1[0] <= arr2[0]){
                    arr.push(arr1.shift());
                } else {
                    arr.push(arr2.shift()); 
                }
            }
        }
        return arr;
    }
	```
	

## DOM(Document Object Model 文档对象模型)
1. 查看元素节点。
	- **getElementById("id")**
	- **getElementsByClassName("class")** 类数组
	- **getElementsByTagName("div")** 类数组
	- **getElementsByName("name")** 只有部分标签的name可以生效，比如表单、表单元素、img、iframe等
	- **querySelector('div p #demo .demo')** css选择器的写法,querySelector永远选择一组里面的第一个，所以返回的不是一个类数组而是一个具体的元素。
	- **querySelectorAll('div p #demo .demo')** 一个类数组的集合的话，那么就用querySelectorAll()方法。
	- 两个方法的问题在于，他们返回的不像前面四个是一个实时改变的元素，而是一个副本。当我们用这两个方法选择出来元素之后，我们把本身那个元素修改一下，会发现我们选择出来的那个元素没有变化。


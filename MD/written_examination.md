# 笔试题
1. css，要求
	- 首先一个div元素A在屏幕中垂直居中
	- 元素A的左右边距为15px
	- 元素内部有文本“A”，字体大小为20px，水平垂直居中；
	- 元素A的高为宽度的二分之一

2. arguments是数组吗？如果不是，那是什么？怎么转化成数组？
3. 写出下面输出什么？

	```javascript
	if([] == false) {console.log(1)}
	if({} == false) {console.log(2)}
	if([]) {console.log(3)}
	if([1] == [1]) {console.log(4)}
	```

4. 写出下面输出顺序？

	```javascript
	async function async1(){
	    console.log("async1 start");
	    await async2()
	    console.log("async1 end");
	};
	
	async function async2(){
	    console.log("async2");
	};
	
	    
	
	setTimeout(function() {
	    console.log('setTimeout');
	},0);
	    
	console.log("start");
	
	async1();
	
	new Promise(function (resolve){
	    console.log("Promise1");
	    resolve();
	}).then(function(){
	    console.log("Promise2");
	});
	
	console.log("end");
	```

5. 修改下面错误代码输出如下，可以使用es6。

	```
	0
	1
	2
	3
	jSer
	js
	-------------
	jSer
	jquery
	-------------
	jSer
	vue
	-------------
	jSer
	react
	-------------
	```
	``` javascript
	var obj = {
        name: 'jSer',
        skill: ['js', 'jquery', 'vue', 'react'],
        say:function (){
            for (var i = 0 ,len = this.skill.length; i < len; i++) {
                setTimeout(function (){
                    console.log(this.name);
                    console.log(this.skill[i]);
                    console.log('-------------')
                }, 0)
                console.log(i)
            }
        }
    }
    obj.say();

	```
	
6. 实现js中的bind函数，要求除了改变this指向，还需要返回的新函数的原型链上有原有的函数和原有函数的原型。

7. 实现节流函数

8. 实现函数function fn(arr, n, sum){...},arr是一个乱序的但是不重复的一个数组。然后在这个数组里选n个数，加起来等于sum。返回一种结果就行。比如:
	
	``` javascript
	//输入
	fn([1,3,5,4,2], 2, 9)
	//输出
	[4,5]
	```

9. 实现以下inherit继承函数,要求child继承parent，而且child修改prototype不会影响到parent
	
	```
	var child = inherit(parent, {
		name: 'child'
	})
	```
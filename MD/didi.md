# 滴滴面经
###编程题
1. 有一个数组
	- 正负整数，
	- 无序
	- 找出相加等于10的
	- 每项不重复
	- 输入:[2,1,4,6,3,5,8,9]
	- 输出:[[2,8],[1,9],[4,6]]
	- **解法1:**

	```javascript
	let arr = [8,5,4,6,3,2,1,9];
	let sum10 = (arr) => {
		let obj = {};
		let newArr = [];
		arr.forEach(function(item) {
			if(~arr.indexOf(10-item) && !obj[10-item] && !obj[item] && item!= 10 - item) {
				obj[item] = 10 - item; 
				newArr.push([item,10-item]);
			}
		})
		return newArr;
	}
	console.log(arr);
	console.log(sum10(arr));
	```
	
	- **解法2:**为了实现更好的时间复杂度，不能够使用js数组的sort方法(原理为冒泡排序，时间复杂度略高)。所以自己手写了一个快排。然后将排序好的数组，用i、j两个索引，一个从前，一个从后，开始遍历。

	```javascript
	let qSort = (arr) => {
	    let len = arr.length;
	    if(!len){
	        return arr;
	    }
	    let left = [];
	    let right = [];
	    let mid = arr.splice(Math.floor(len / 2), 1)[0];
	    arr.forEach(v => {
	        if(v > mid){
	            right.push(v);
	        }else{
	            left.push(v);
	        }
	    })
	    return [...qSort(left), mid, ...qSort(right)];;
	};
	let arr = [2,1,4,6,3,5,8,9];
	let sum10 = (arr) => {
	    arr = qSort(arr);
	    let newArr = [];
	    let len = arr.length;
	    let i = 0;
	    let j = len - 1;
	    while(i <= j){
	        if(arr[i] + arr[j] == 10){
	            newArr.push([arr[i], arr[j]]);
	            i ++;
	            j --;
	        } else if(arr[i] + arr[j] > 10){
	            j --;
	        } else if(arr[i] + arr[j] < 10){
	            i ++;
	        }
	    }
	    return newArr;
	}
	console.log(arr);
	console.log(sum10(arr));
	```
2. 已知有两个栈，有pop、push、getSize接口，请用这2个栈实现1个队列，包含enqueue和dequeue接口

	```javascript
	var arr1 = [1,2,3,4,5];
   	var arr2 = []
    Array.prototype.enqueue = function(ele){
        this.push(ele);
    }
    Array.prototype.dequeue = function(){
        var len1 = this.length;//或者用getSize
        for(let i = 0; i < len1; i++){
            arr2.push(this.pop());
        }
        var tar = arr2.pop();
        var len2 = arr2.length;//或者用getSize
        for(let i = 0; i < len2; i++){
            this.push(arr2.pop());
        }
        return tar;
    }
    arr1.enqueue(6);
    arr1.dequeue();
    console.log(arr1);
	```

3. jsonp函数实现

	``` javascript
    function jsonp(url,data, callbackName){
    	let callbackName = callbackName || "callback";
		let data = data || {};
		if(~url.indexOf('?')){
			url = `${url}&callback=${callbackName}`;
		}else{
			url = `${url}?callback=${callbackName}`;
		}
		Object.keys(data).forEach((prop) => {
			url = `${url}&${prop}=${data[prop]}`;
		})
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
    
4. 实现find函数
	- `find(obj, str)`
	- `obj={a:{b:{c:1}}}`
	- `find(obj, ‘a.b.c’)`返回**1**,`find(obj, ‘a.b.c’)`返回**undefined**
	
	```javascipt
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
	
5. 斐波那契数列
	- 递归方式

	```javascript
	function fbnq(n){
	    if(n==1||n==2){
	        return 1;
	    }
	    return fbnq(n-1) + fbnq(n-2);
	}
	fbnq(10);
	```
	
	- 非递归

	```
	function fbnq(n){
		var arr = [0, 0, 1];
		for(var i = 1; i < n; i++){
			arr[0] = arr[1];
			arr[1] = arr[2];
			arr[2] = arr[0] + arr[1];
		}
		return arr[2];
	}
	fbnq(10);
	```

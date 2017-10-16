# JS二叉树

## 生成一棵二叉树

- 把下面数组生成一个二叉树：`let nodes = [8,3,6,4,9,11,2,5,7];`

- 结构如下

![](https://github.com/lj614418910/blog/blob/master/images/tree1.png)

- 生成的二叉树对象如下

	```
	let tree = {
	    key: 8,
	    left: {
	        key: 3,
	        left: {
	            key: 2,
	            left: null,
	            right: null
	        },
	        right: {
	            key: 6,
	            left: {
	                key: 4,
	                left: null,
	                right: {
	                    key: 5,
	                    left: null,
	                    right: null
	                }
	            },
	            right: {
	                key: 7,
	                left: null,
	                right: null
	            }
	        }
	    }
	    right: {
	        key: 9,
	        left: null,
	        right: {
	            key: 11,
	            left: null,
	            right: null
	        }
	    }
	}	
	```

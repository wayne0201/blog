# JS二叉树

## 生成一棵二叉树

### 二叉树实现原理

- 把第一位当做根节点，比根节点小的数放在左子树上，比根节点大的数放到右子树上，以此类推。

- 把下面数组生成一个二叉树：`let nodes = [8,3,6,4,9,11,2,5,7];`

- 结构如下

![](https://github.com/lj614418910/blog/blob/master/images/tree1.png)

- 生成的二叉树对象如下

	``` javascript
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

### 生成二叉树实现代码

``` javascript
class BinaryTree{
	constructor() {
		this.root = null;//树根
		this.Node = key => {
			//生成一个新子树
			let _obj = Object.create(null, {});
			_obj.key = key;
			_obj.left = null;
			_obj.right = null;
			return _obj;
		}
	}
	insertNode (node, newNode){
		//如何插入子树
		if (newNode.key < node.key) {
			if (node.left === null) {
				node.left = newNode;
			} else {
				this.insertNode(node.left, newNode)
			}
		} else {
			if (node.right === null) {
				node.right = newNode;
			} else {
				this.insertNode(node.right, newNode)
			}
		}
	}
	insert(key) {
		//插入子树
		let newNode = this.Node(key);
		if (this.root === null) {
			this.root = newNode;
		} else {
			this.insertNode(this.root, newNode);
		}
	}
	showTree () {
		//显示二叉树
		return this.root;
	}
}


let nodes = [8,3,6,4,9,11,2,5,7];
let binaryTree = new BinaryTree();

nodes.forEach(key => {
    binaryTree.insert(key);
})
let tree = binaryTree.showTree();
```



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
//API:
//insert添加一个子树，传值Number
//bulkInsert批量添加子树，传值Array
//showTree返回二叉树对象
class BinaryTree {
	constructor(tree = []) {
		this.root = null;//树根
		this.Node = key => {
			//生成一个新的子树
			let _obj = Object.create(null, {});
			_obj.key = key;
			_obj.left = null;
			_obj.right = null;
			return _obj;
		}
		//初始化二叉树
		if (typeof tree === 'number') {
			this.insert(tree);
		} else if (Array.isArray(tree)) {
			this.bulkInsert(tree)
		} else {
			console.error('请输入Number类型或者Array类型的参数')
		}
	}
	insert(key) {
		//添加一个新子树
		let newNode = this.Node(key);
		let _insertNode = (node, newNode) => {
			//判断新二叉树的值和原有节点的值
			if (newNode.key < node.key) {
				if (node.left === null) {
					//判断左节点是否为空
					node.left = newNode;
				} else {
					_insertNode(node.left, newNode)
				}
			} else {
				if (node.right === null) {
					//判断右节点是否为空
					node.right = newNode;
				} else {
					_insertNode(node.right, newNode)
				}
			}
		}
		if (this.root === null) {
			//如果没有根节点，那么把传入的值当根节点
			this.root = newNode;
		} else {
			//如果有根节点，那么把传入的值插到二叉树上
			_insertNode(this.root, newNode);
		}
	}
	bulkInsert (nodes) {
		nodes.forEach(key => {
			//遍历数组，插入子树
			this.insert(key);
		})
	}
	showTree () {
		//返回二叉树对象
		return this.root;
	}
}


let nodes = [8,3,6,4,9,11,2,5,7];
let binaryTree = new BinaryTree(nodes);
let tree = binaryTree.showTree();
```

## 先序遍历、中序遍历、后序遍历

- 先序遍历
	- 根左右
- 中序遍历
	- 左根右 
- 后序遍历
	- 左右根 

``` javascript
class BinaryTree {
	......//生成树的代码
	inOrderTraverse (fn) {
		//中序遍历，传入一个回调函数
		let inOrderTraverseNode = (node, callback) => {
			if (node !== null) {
				inOrderTraverseNode(node.left, callback);
				callback(node.key);
				inOrderTraverseNode(node.right, callback);
			}
		}
		inOrderTraverseNode(this.root, fn)
	}

	preOrderTraverse (fn) {
		//先序遍历，传入一个回调函数
		let preOrderTraverseNode = (node, callback) => {
			if (node !== null) {
				callback(node.key);
				preOrderTraverseNode(node.left, callback);
				preOrderTraverseNode(node.right, callback);
			}
		}
		preOrderTraverseNode(this.root, fn)
	}

	postOrderTraverse (fn) {
		//先序遍历，传入一个回调函数
		let postOrderTraverseNode = (node, callback) => {
			if (node !== null) {
				postOrderTraverseNode(node.left, callback);
				postOrderTraverseNode(node.right, callback);
				callback(node.key);
			}
		}
		postOrderTraverseNode(this.root, fn)
	}
}

let nodes = [8,3,6,4,9,11,2,5,7];
let binaryTree = new BinaryTree(nodes);
let	arr1 = [],
	arr2 = [],
	arr3 = []

binaryTree.inOrderTraverse(key => {
	arr1.push(key);//中序遍历[2, 3, 4, 5, 6, 7, 8, 9, 11]
})
binaryTree.preOrderTraverse(key => {
	arr2.push(key);//先序遍历[8, 3, 2, 6, 4, 5, 7, 9, 11]
})
binaryTree.postOrderTraverse(key => {
	arr3.push(key);//后序遍历[2, 5, 4, 7, 6, 3, 11, 9, 8]
})
```


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
let arr1 = [],
    arr2 = [],
    arr3 = [];

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

## 二叉树节点查找

- 查找最大值、最小值

``` javascript
class BinaryTree {
    ......//生成树的代码与三种遍历代码
    min() {
        let node = this.root;
        if (node) {
            //循环遍历左子树
            while (node && node.left !== null) {
                node = node.left;
            }
            return node.key;
        }
    }

    max() {
        let node = this.root;
        if (node) {
            //循环遍历右子树
            while (node && node.right !== null) {
                node = node.right;
            }
            return node.key;
        }
    }
}

let nodes = [8,3,6,4,9,11,2,5,7];
let binaryTree = new BinaryTree(nodes);

let min = binaryTree.min();//2
let max = binaryTree.max();//11

```

- 查找二叉树中节点是否存在

``` javascript
class BinaryTree {
    ......//生成树的代码与三种遍历代码
    search(key) {
        let searchNode = (node, key) => {
            if (node === null) {
                return false;
            }
            if (key < node.key) {
                //当查找的值小于当前节点的值，则递归其左子树
                return searchNode(node.left, key);
            } else if (key > node.key){
                //当查找的值大于当前节点的值，则递归其右子树
                return searchNode(node.right, key);
            } else {
                return true;
            }
        }
        return searchNode(this.root, key)
    }
}

let nodes = [8,3,6,4,9,11,2,5,7];
let binaryTree = new BinaryTree(nodes);

let inTree1 = binaryTree.showTree(2);//true
let inTree2 = binaryTree.showTree(1);//false
```

## 二叉树节点删除

- 需要判断这个节点是否还有子树，有三种情况，无子树，有单子树，有双子树。

``` javascript
class BinaryTree {
    ......//生成树的代码与三种遍历代码
    remove(key) {
        let findMinNode = (node, key) => {
            //查找最小子节点
            let node = node || this.root;
            if (node) {
                while (node && node.left !== null) {
                    node = node.left;
                }
                return node;
            }
            return null;
        }
        let removeNode = (node, key) => {
            if (node === null) {
                //如果为null，返回null
                return null
            }

            if (key < node.key) {
                //如果值小于当前节点值，递归其左子树
                node.left = removeNode(node.left, key);
                return node;
            } else if (key > node.key) {
                //如果值大于当前节点值，递归其右子树
                node.right = removeNode(node.right, key);
                return node;
            } else {
                //如果值等于当前节点值，做该节点删除操作
                if (node.left === null && node.right === null) {
                    //如果该节点没有左右子树，直接使其为null
                    node = null;
                    return node;
                }
                if (node.left === null) {
                    //如果该节点只有右子树，则将该节点指向它的右子树
                    node = node.right;
                    return node;
                } else if (node.right === null) {
                    //如果该节点只有左子树，则将该节点指向它的左子树
                    node = node.left;
                    return node;
                } 

                if (node.left !== null && node.right !== null) {
                    //如果该节点有左右子树，则把找到它的右子树的最小子节点，并使该节点的值等于它的右子树的最小子节点的值，然后删除它的右子树的最小子节点
                    let aux = findMinNode(node.right);
                    node.key = aux;
                    node.right = removeNode(node.right, aux.key);
                    return node;        
                }
            }
        }
        this.root = removeNode(this.root, key)
    }
}

let nodes = [8,3,6,4,9,11,2,5,7];
let binaryTree = new BinaryTree(nodes);
```

## JS二叉树完整代码

``` javascript
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

    inOrderTraverse (fn) {
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
        let postOrderTraverseNode = (node, callback) => {
            if (node !== null) {
                postOrderTraverseNode(node.left, callback);
                postOrderTraverseNode(node.right, callback);
                callback(node.key);
            }
        }
        postOrderTraverseNode(this.root, fn)
    }

    min() {
        let node = this.root;
        if (node) {
            while (node && node.left !== null) {
                node = node.left;
            }
            return node.key;
        }
    }

    max() {
        let node = this.root;
        if (node) {
            while (node && node.right !== null) {
                node = node.right;
            }
            return node.key;
        }
    }



    search(key) {
        let searchNode = (node, key) => {
            if (node === null) {
                return false;
            }
            if (key < node.key) {
                return searchNode(node.left, key);
            } else if (key > node.key){
                return searchNode(node.right, key);
            } else {
                return true;
            }
        }
        return searchNode(this.root, key)
    }

    remove(key) {
        let findMinNode = (node, key) => {
            let node = node || this.root;
            if (node) {
                while (node && node.left !== null) {
                    node = node.left;
                }
                return node;
            }
            return null;
        }
        let removeNode = (node, key) => {
            if (node === null) {
                return null
            }

            if (key < node.key) {
                node.left = removeNode(node.left, key);
                return node;
            } else if (key > node.key) {
                node.right = removeNode(node.right, key);
                return node;
            } else {
                if (node.left === null && node.right === null) {
                    node = null;
                    return node;
                }
                if (node.left === null) {
                    node = node.right;
                    return node;
                } else if (node.right === null) {
                    node = node.left;
                    return node;
                } 

                if (node.left !== null && node.right !== null) {
                    let aux = findMinNode(node.right);
                    node.key = aux;
                    node.right = removeNode(node.right, aux.key);
                    return node;        
                }
            }
        }
        this.root = removeNode(this.root, key)
    }
}

let nodes = [8,3,6,4,9,11,2,5,7];
let binaryTree = new BinaryTree(nodes);
```

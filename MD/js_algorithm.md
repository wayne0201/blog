#JS排序算法

## 冒泡排序

#### 算法原理

1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
2. 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。
3. 针对所有的元素重复以上的步骤，除了最后一个。
4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

#### JS实现

```
let bubbleSort = (arr = []) => { 
	//冒泡排序
    let len = arr.length,
        temp; 
    for (let i = 0; i < len; i++) {
        for (let j = 0; j < len - 1 - i; j++) {
            if (arr[j] > arr[j+1]) {        //相邻元素两两对比
                temp = arr[j+1];        //元素交换
                arr[j+1] = arr[j];
                arr[j] = temp;
            }
        }
    }
    return arr
}
```

## 选择排序

#### 算法原理
- 每一次从待排序的数据元素中选出最小（或最大）的一个元素，存放在序列的起始位置，直到全部待排序的数据元素排完。

#### JS实现

```
let selectionSort = (arr = []) => {  
	//选择排序
    let len = this.length,
        temp,
        minIndex;
    for(let i = 0; i < len - 1; i ++){
        minIndex =  i;
        for(let j = i + 1; j < len; j++){
            if(arr[j] < arr[minIndex]){
                minIndex = j;
            }
        }
        temp = arr[minIndex];
        arr[minIndex] = arr[i];
        arr[i] = temp;
    }
    return arr
}
```

## 插入排序

#### JS实现

```
let insertionSort = (arr = []) => { 
	//插入排序
    let len = this.length,
        temp; 
    for (let i = 1; i < len; i++) {
        for(let j = i; j > 0 && arr[j] < arr[j-1]; j--){
            temp = arr[j];
            arr[j] = arr[j-1];
            arr[j-1] = temp;
           
        }
    }
    return arr
}
```

## 快速排序

#### js实现

```
let qSort = (arr) => {
  let len = arr.length;
  if(!len){
    return arr;
  }
  let left = [];
  let right = [];
  let mid = arr.splice(Math.floor(len / 2), 1)[0];
  arr.map(v => {
    if(v > mid){
      right.push(v);
    }else{
      left.push(v);
    }
  })
  return qSort(left).concat([mid], qSort(right));
}
```
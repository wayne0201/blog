# JS排序算法

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
#### 算法原理
- 通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

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
    arr.forEach(v => {
        if(v > mid){
            right.push(v);
        }else{
            left.push(v);
        }
    })
    return [...qSort(left), mid, ...qSort(right)];;
}
```

## 归并排序
#### 算法原理
- 采用分治法（Divide and Conquer）的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为二路归并。

#### js实现

```
let merge = (left, right) => {
    let result = [];
    while (left.length>0 && right.length>0) {
        if (left[0] < right[0]) {
            result.push(left.shift());
        } else {
            result.push(right.shift());
        }
    }
    return [...result, ...left, ...right];
}

let mergeSort = (arr) => {  //采用自上而下的递归方法
    let len = arr.length;
    if(len == 1) {
        return arr;
    }
    let middle = Math.floor(len / 2),
        left = arr.slice(0, middle),
	right = arr.slice(middle);
    return merge(mergeSort(left), mergeSort(right));
}

console.log(mergeSort(arr));
```



+++

title = '一些常见排序的梳理'
date = 2024-04-06T11:52:10+08:00
author = "Yotom"
description = "七种必备的经典排序"
tags = [
    "排序",
    "双指针",

]
categories = [
    "学习记录",
    "算法", 
]

+++

# 排序

排序的算法是在面试中经常提到的，且因为部分排序涉及到常见的数据结构，所以这是求职中必备的内容，这里进行一些梳理。

---

## 冒泡排序

### 概念

冒泡排序基本是编程中比较简单的排序，很多初学者第一次接触到的排序算法就是它。

冒泡排序的思想是重复走访要排序的数列，依次比较两种元素，次序部队则进行交换，一直重复这样的过程，直到没有需要交换的元素。

### 算法原理

下图是个无序数列：1，5，4，2，6，3。

按照冒泡排序的思想我们要把相邻的元素两两比较，根据大小来交换元素的位置。

<div style="text-align:center">
    <img src="/img/bubble_1.png" alt='b1' style="max-width: 75%; height: auto;">
</div>

首先开始第一轮

1. 比较1和5，1比5小，顺序正确，位置不变
2. 比较5和4，5比4大，顺序错误，交换位置

<div style="text-align:center">
    <img src="/img/bubble_2.png" alt='b2' style="max-width: 75%; height: auto;">
</div>

3. 比较5和2，5比2大，顺序不对，交换位置

<div style="text-align:center">
    <img src="/img/bubble_3.png" alt='b3' style="max-width: 75%; height: auto;">
</div>

依次类推，经过第一轮比较后，6作为最大的元素排在了序列最右侧：

<div style="text-align:center">
    <img src="/img/bubble_4.png" alt='b4' style="max-width: 75%; height: auto;">
</div>

接着开始第二轮，得到的结果应该是把5放在了6的左侧……

然后是第三轮，结果是4放在了5的左侧……

这样以此类推，最后就得到了一个有序的数列：

<div style="text-align:center">
    <img src="/img/bubble_5.png" alt='b5' style="max-width: 75%; height: auto;">
</div>

### 算法特点

**时间复杂度**：冒泡排序每一轮要遍历所有需要排序的元素，时间复杂度为O(N^2)。

**空间复杂度**：冒泡排序过程中需要一个临时变量进行两两交换，额外空间为1。

**稳定性**：冒泡排序在排序过程中，相同元素的前后顺序并不改变，所以是一种稳定的排序。

### 算法实现

```c++
void swap(int *a, int *b){
    int t = *a;
    *a = *b;
    *b = t;
}

void bubbleSort(int arr[], int n){
    for(int i = 0; i < n-1; i++){
        for(int j = 0; j < n-i-1; j++)
        if(arr[j] > arr[j+1]) swap(arr[j], arr[j+1]);
	}
}
```

---

## 选择排序

### 概念

选择排序是和冒泡排序一样的简单排序算法，思路与冒泡排序也类似，每轮选择一个最大或者最小的元素放在首位，直到所有的元素排完。

### 算法原理

与之前一样，下图是个无序数列：1，5，4，2，6，3。

按照选择排序的思想我们要把最小的元素找到并移至首位。

<div style="text-align:center">
    <img src="/img/bubble_1.png" alt='b1' style="max-width: 75%; height: auto;">
</div>

首先开始第一轮

1. 比较1和5，1比5小，最小元素是1

2. 比较1和4，1比4小，最小元素是1

   ……………………………………………

3. 经过一轮比较后，没有比1小的元素，所以1放在首位，位置不变

<div style="text-align:center">
    <img src="/img/selection_1.png" alt='s1' style="max-width: 75%; height: auto;">
</div>

然后开始第二轮

1. 比较5和4，4比5小，最小元素是4

2. 比较5和2，2比5小，最小元素是2

   ……………………………………………

3. 经过一轮比较后，发现最小元素是2，比5小，所以交换位置

<div style="text-align:center">
    <img src="/img/selection_2.png" alt='s2' style="max-width: 75%; height: auto;">
</div>

这样以此类推，最后得到有序数列：

<div style="text-align:center">
    <img src="/img/bubble_5.png" alt='b5' style="max-width: 75%; height: auto;">
</div>

### 算法特点

**时间复杂度**：选择排序每一轮要遍历所有需要排序的元素，共遍历n-1轮，时间复杂度为O(N^2)。

**空间复杂度**：选择排序过程中需要一个临时变量进行两两交换，额外空间为1。

**稳定性**：选择排序在排序过程中，相同元素的前后顺序有可能会改变，所以是一种不稳定的排序。

<div style="text-align:center">
    <img src="/img/selection_3.png" alt='s3' style="max-width: 75%; height: auto;">
</div>

### 算法实现

```c++
void swap(int *a, int *b){
    int t = *a;
    *a = *b;
    *b = t;
}
    
void selectionSort(int arr[], int n){
    for(int i = 0; i < n-1; i++){
        int minIndex = i;
        for(int j = i+1; j < n; j++)
            if(arr[j] < arr[minIndex]) minIndex = j;
        if(i != minIndex) swap(arr[i], arr[minIndex]);
    }
}
```

---

## 插入排序

### 概念

插入排序又称斗地主排序（戏称），是一种我们习惯性上使用，但平时很少察觉的排序。原理是将未排序的数据，挨个插入到有序数列的相应位置。

### 算法原理

与之前一样，下图是个无序数列：1，5，4，2，6，3。

按照插入排序的思想我们要找将无序数列插入到有序数列中的相应位置

<div style="text-align:center">
    <img src="/img/bubble_1.png" alt='b1' style="max-width: 75%; height: auto;">
</div>
首先是第一轮：

1. 5和1比较，1小于5，无需换位

<div style="text-align:center">
    <img src="/img/insert_1.png" alt='i1' style="max-width: 75%; height: auto;">
</div>

得到了1、5这个有序数列，接着开始第二轮：

1. 4和5比较，4小于5，再看有序数列的下一个，和1比较
2. 4和1比较，4大于1，发现4的值在1、5之间
3. 将4和5换位，完成插入

<div style="text-align:center">
    <img src="/img/insert_2.png" alt='i2' style="max-width: 75%; height: auto;">
</div>

按照这样以此类推，最终得到有序数列：

<div style="text-align:center">
    <img src="/img/bubble_5.png" alt='b5' style="max-width: 75%; height: auto;">
</div>

### 算法特点

**时间复杂度**：插入排序要进行n-1轮，每轮对比最坏的情况就是1……n-1全部对比一遍，故时间复杂度为O(N^2)。

**空间复杂度**：插入排序过程中需要一个临时变量进行两两交换，额外空间为1。

**稳定性**：插入排序将无序数列插入有序数列的过程中，不改变相同元素的前后位置，是一种稳定的排序的算法。

### 算法实现

```c++
void insertSort(int arr[], int n){
    for(int i = 0; i < n-1; i++){
        int end = i, t = arr[end+1];
        while(end>=0){
            if(t < arr[end]){
                arr[end+1] = arr[end];
                end--;
            }else break;
        }
        a[end+1] = t;
    }
}
```

---

## 希尔排序

### 概念

希尔排序是什么？在我眼中就是分类的插入排序，当然你可以反过来说插入排序是希儿增量为1的希尔排序，他们的主体思路是一致的。

### 算法原理

这是一个无序数列：1、5、8、4、7、2、6、3，我们要将它按从小到大排序。按照希尔排序的思想，我们先把数列进行分组排序

<div style="text-align:center">
    <img src="/img/shell_1.png" alt='sh1' style="max-width: 75%; height: auto;">
</div>

首先，我们选择序列长度的一半4，作为增量进行分组

<div style="text-align:center">
    <img src="/img/shell_2.png" alt='sh2' style="max-width: 75%; height: auto;">
</div>

如果所示，1和7一组，5和2一组，8和6一组，4和3一组，共四组

然后，我们对每一组进行插入排序，排序后序列如下

<div style="text-align:center">
    <img src="/img/shell_3.png" alt='sh3' style="max-width: 75%; height: auto;">
</div>

经过这一轮排序，序列有序了很多，接着我们进一步缩小增量，增量缩小为原来的一半，也就是2，再进行分组排序

<div style="text-align:center">
    <img src="/img/shell_4.png" alt='sh4' style="max-width: 75%; height: auto;">
</div>

如图所示，1、6、7、8一组，2、3、5、4一组，共两组

我们再对每一组进行插入排序，排序后序列如下

<div style="text-align:center">
    <img src="/img/shell_5.png" alt='sh5' style="max-width: 75%; height: auto;">
</div>

最后，我们进一步把增量缩小为原来一半，也就是1，这相当于直接在序列上进行插入排序，排序结果如下

<div style="text-align:center">
    <img src="/img/shell_6.png" alt='sh6' style="max-width: 75%; height: auto;">
</div>

至此所有的元素都是有序的
### 算法特点

**时间复杂度**：

希尔排序算法利用分组粗调的方式减少了插入排序算法的工作量，使得算法的平均复杂度低于O(N^2)

但某些极端的情况下，希尔排序算法的时间复杂度仍然是O(N^2)，甚至比插入排序算法更慢

为了降低希尔排序算法的时间复杂度，提出了更严谨的算法增量

+ Hibbard增量，序列为：1、3、7、15…，通项公式为2^k-1， 最坏的时间复杂度为O(n^(3/2))

+ Sedgewick增量，序列为：1、5、19、41、109…，通项公式为 9 * 4^k - 9 * 2^k + 1 或者 4^k - 3 * 2^k + 1，最坏的时间复杂度为O（n^(4/3)）

**空间复杂度**：

希尔排序算法排序过程中需要一个临时变量存储插入元素，所需要的额外空间为1，因此空间复杂度为O(1)

**稳定性**：

希尔排序算法会进行分组排序，在分组排序的过程中有可能改变相同元素的前后位置，因此是一种不稳定的排序算法

<div style="text-align:center">
    <img src="/img/shell_7.png" alt='sh7' style="max-width: 75%; height: auto;">
</div>

### 算法实现

```c++
void shellSort(int arr[], int n){
    int gap = n;
    while(gap > 1){
        gap /= 2;
        for(int i = 0; i < n-gap; i++){
            int end = i, t = arr[end+gap];
            while(end>=0){
                if(arr[end]>t){
                    arr[end+gap] = arr[end];
                    end -= gap;
                }else break;
            }
            a[end+gap] = t;
        }
    }
}
```

---

## 快速排序

### 概念

快速排序（Quick Sort）是从冒泡排序演变而来的，实际上是在冒泡排序基础上的递归分治法。快速排序在每一轮挑选一个基准元素，并让其他比它大的元素移动到数列一边，比它小的元素移动到数列的另一边，从而把数列拆解成了两个部分。

### 算法原理

这是一个无序数列：4、5、8、1、7、2、6、3，我们要将它按从小到大排序。按照快速排序的思想，我们先选择一个基准元素，进行排序

<div style="text-align:center">
    <img src="/img/quick_1.png" alt='q1' style="max-width: 75%; height: auto;">
</div>

我们选取4为我们的基准元素，并设置基准元素的位置为index，设置两个指针left和right，分别指向最左和最右两个元素

<div style="text-align:center">
    <img src="/img/quick_2.png" alt='q2' style="max-width: 75%; height: auto;">
</div>

接着，从right指针开始，把指针所指向的元素和基准元素做比较，如果比基准元素大，则right指针向左移动，如果比基准元素小，则把right所指向的元素填入index中

3和4比较，3比4小，将3填入index中，原来3的位置成为了新的index，同时left右移一位

<div style="text-align:center">
    <img src="/img/quick_3.png" alt='q3' style="max-width: 75%; height: auto;">
</div>

然后，我们切换left指针进行比较，如果left指向的元素小于基准元素，则left指针向右移动，如果元素大于基准元素，则把left指向的元素填入index中

5和4比较，5比4大，将5填入index中，原来5的位置成为了新的index，同时right左移一位

<div style="text-align:center">
    <img src="/img/quick_4.png" alt='q4' style="max-width: 75%; height: auto;">
</div>

接下来，我们再切换到right指针进行比较，6和4比较，6比4大，right指针左移一位

<div style="text-align:center">
    <img src="/img/quick_5.png" alt='q5' style="max-width: 75%; height: auto;">
</div>

2和4比较，2比4小，将2填入index中，原来2的位置成为新的index，left右移一位

<div style="text-align:center">
    <img src="/img/quick_6.png" alt='q6' style="max-width: 75%; height: auto;">
</div>

随着left右移，right左移，最终left和right重合

<div style="text-align:center">
    <img src="/img/quick_7.png" alt='q7' style="max-width: 75%; height: auto;">
</div>

此时，我们将基准元素填入index中，这时，基准元素左边的都比基准元素小，右边的都比基准元素大，这一轮交换结束

<div style="text-align:center">
    <img src="/img/quick_8.png" alt='q8' style="max-width: 75%; height: auto;">
</div>

### 算法特点

**时间复杂度**
快速排序算法在分治法的思想下，原数列在每一轮被拆分成两部分，每一部分在下一轮又分别被拆分成两部分，直到不可再分为止，平均情况下需要logn轮，因此快速排序算法的平均时间复杂度是O(nlogn)

在极端情况下，快速排序算法每一轮只确定基准元素的位置，时间复杂度为O(N^2)

**空间复杂度**
快速排序算法排序过程中只是使用数组原本的空间进行排序，因此空间复杂度为O(1)

**稳定性**
快速排序算法在排序过程中，可能使相同元素的前后顺序发生改变，所以快速排序是一种不稳定排序算法

### 算法实现

思路版

```c++
void quickSort(int arr[], int l, int r){
    if(l >= r) return ;
    int base = arr[l];
    int u = l, v = r;
    while(u < v){
        while(u < v && arr[v] >= base) v--;
        arr[u] = arr[v];
        while(u < v && arr[u] <= base) u++;
        arr[v] = arr[u];
    }
    arr[u] = base;
    quickSort(arr, l, u-1), quickSort(arr, u+1, r);
}
```

优化版

```c++
void swap(int *a, int *b){
    int t = *a;
    *a = *b;
    *b = t;
}

void quickSort(int arr[], int l, int r){
    if(l >= r) return;
    int base = arr[(l+r)/2];
    int u = l-1, v = r+1;
    while(u < v){
        u++;while(arr[u] < base);
        v--;while(arr[v] > base)
        if(u < v) swap(arr[u], arr[v]);
    }
    quickSort(arr, l, v), quickSort(arr, v+1, r);
}
```

---

## 归并排序

### 概念

归并排序（Merge sort）是建立在归并操作上的一种有效的排序算法，归并排序对序列的元素进行逐层折半分组，然后从最小分组开始比较排序，合并成一个大的分组，逐层进行，最终所有的元素都是有序的。

### 算法思路

这是一个无序数列：4、5、8、1、7、2、6、3，我们要将它按从小到大排序。按照归并排序的思想，我们要把序列逐层进行拆分

<div style="text-align:center">
    <img src="/img/merge_1.png" alt='m1' style="max-width: 75%; height: auto;">
</div>

拆分如下：

<div style="text-align:center">
    <img src="/img/merge_2.png" alt='m2' style="max-width: 75%; height: auto;">
</div>

接着两两有序合并，即可得到有序数列

<div style="text-align:center">
    <img src="/img/merge_3.png" alt='m3' style="max-width: 75%; height: auto;">
</div>

### 算法特点

**时间复杂度**
归并排序算法每次将序列折半分组，共需要logn轮，因此归并排序算法的时间复杂度是O(nlogn)

**空间复杂度**
归并排序算法排序过程中需要额外的一个序列去存储排序后的结果，所占空间是n，因此空间复杂度为O(n)

**稳定性**
归并排序算法在排序过程中，相同元素的前后顺序并没有改变，所以归并排序是一种稳定排序算法

### 算法实现

```c++
void mergeSort(int arr[], int l, int r){
    if(l >= r) return;
    int m = (l+r)/2;
    mergeSort(arr, l, m), mergeSort(arr, m+1, r);
    const int d = r-l+1;
    int t[d], u = l, v = m+1, k = 0;
    while(u <= m && v <= r){
        if(arr[u] < arr[v]) t[k++] = arr[u++];
        else t[k++] = arr[v++];
    }
    while(u<=m)t[k++] = arr[u++];
   	while(v<=r)t[k++] = arr[v++];
    
    for(u = l, v = 0; u <= r; u++, v++) arr[u] = t[v];
}
```

---

## 堆排序

### 概念

堆排序是利用二叉堆（完全二叉树）的概念来排序的选择排序算法，分为两种：

+ 升序排序：利用最大堆进行排序
+ 降序排序：利用最小堆进行排序

### 算法原理
#### bilibili视频

{{< bilibili BV1AF411G7cA >}}
#### 图文
以最大堆为例进行演示

<div style="text-align:center">
    <img src="/img/heap_1.png" alt='h1' style="max-width: 75%; height: auto;">
</div>

首先删除堆顶元素10（即最大元素），然后将3补充到堆顶，删掉的元素10放在原来3的位置。

<div style="text-align:center">
    <img src="/img/heap_2.png" alt='h2' style="max-width: 75%; height: auto;">
</div>

根据二叉堆的自我调节，第二大的元素9会成为新的堆顶

<div style="text-align:center">
    <img src="/img/heap_3.png" alt='h3' style="max-width: 75%; height: auto;">
</div>

删除元素9，元素8成为最大堆堆顶

<div style="text-align:center">
    <img src="/img/heap_4.png" alt='h4' style="max-width: 75%; height: auto;">
</div>

删除元素8，元素7成为最大堆堆顶

<div style="text-align:center">
    <img src="/img/heap_5.png" alt='h5' style="max-width: 75%; height: auto;">
</div>

依次删除最大元素，直到所有元素被删除

<div style="text-align:center">
    <img src="/img/heap_6.png" alt='h6' style="max-width: 75%; height: auto;">
</div>

此时，被删除的元素组成了一个由小到大的有序数列

<div style="text-align:center">
    <img src="/img/heap_7.png" alt='h7' style="max-width: 75%; height: auto;">
</div>

### 算法特点

**时间复杂度**
下沉调整的时间复杂度等同于堆的高度O(logn)，构建二叉堆执行下沉调整次数是n/2，循环删除进行下沉调整次数是n-1，时间复杂度约为O(nlogn)

**空间复杂度**
堆排序算法排序过程中需要一个临时变量进行两两交换，所需要的额外空间为1，因此空间复杂度为O(1)

**稳定性**
堆排序算法在排序过程中，相同元素的前后顺序有可能发生改变，所以堆排序是一种不稳定排序算法

### 算法实现

```c++
void heapify(std::vector<int>& arr, int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    
    if (left < n && arr[left] > arr[largest])
        largest = left;

    if (right < n && arr[right] > arr[largest])
        largest = right;

    if (largest != i) {
        std::swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}
void heapSort(std::vector<int>& arr) {
    int n = arr.size();

    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);

    for (int i = n - 1; i > 0; i--) {
        std::swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}

```

---


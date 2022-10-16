# Tree

There are many types of trees:

## **1. Full Binary Tree**

 either zero children or two children. 



![types of binary tree - full binary tree](/Users/blake/Desktop/JavaNotes/Algorithm/pictures/Picture2-1.jpg)

## **2. Complete Binary Tree**

 ==**all the** **tree levels**== are filled entirely with nodes, ==**except the lowest level of the tree**==. Also, in the last or the lowest level of this binary tree, every node should possibly reside on the left side. 

![types of binary tree - complete binary tree](/Users/blake/Desktop/JavaNotes/Algorithm/pictures/Picture3.jpg)
![img](/Users/blake/Desktop/JavaNotes/Algorithm/pictures/Picture4.jpg)





## **3. Perfect Binary Tree**

A binary tree is said to be ‘perfect’ if all the internal nodes have ==**strictly two children**==, and every external or leaf node is at the same level or same depth within a tree. A perfect [binary tree](https://www.upgrad.com/blog/binary-tree-in-data-structure/) having height ‘h’ has 2h – 1 node. Here is the structure of a perfect binary tree:

![types of binary tree - perfect tree](/Users/blake/Desktop/JavaNotes/Algorithm/pictures/Picture5.jpg)

![img](/Users/blake/Desktop/JavaNotes/Algorithm/pictures/Picture6.jpg)

## **4. Balanced Binary Tree**

the tree height is O(logN), where ‘N’ is the number of nodes. In a balanced binary tree, the height of the left and the right subtrees of each node should vary by at most one. An AVL Tree and a Red-Black Tree are some common examples of data structure that can generate a balanced binary search tree. Here is an example of a balanced binary tree:

![types of binary tree - balanced binary tree](/Users/blake/Desktop/JavaNotes/Algorithm/pictures/Picture7.jpg)

## **5. Degenerate Binary Tree** 

 every internal node has only a single child. Such trees are similar to a linked list performance-wise. 

![degenarate binary tree](/Users/blake/Desktop/JavaNotes/Algorithm/pictures/Picture8.jpg)

# Max Heap & Min Heap

A [Heap](https://www.geeksforgeeks.org/binary-heap/) is a special [Tree-based data structure](https://www.geeksforgeeks.org/binary-tree-data-structure/) in which the tree is a [complete binary tree](https://www.geeksforgeeks.org/binary-tree-set-3-types-of-binary-tree/). 



## Defination

**Min Heap:**

==Priority queue is min heap.==, but priority queue is not very flexible.

```java
PriorityQueue<Integer> minHeap = new PriorityQueue(); //系统默认是小根堆
```



In a [Min-Heap](https://www.geeksforgeeks.org/min-heap-in-java/) the key present at the root node must be less than or equal among the keys present at all of its children. 

![MinHeap](/Users/blake/Desktop/JavaNotes/Algorithm/pictures/MinHeap.png)

Max Heap:

![MaxHeap](/Users/blake/Desktop/JavaNotes/Algorithm/pictures/MaxHeap.png)

Vice Versa



## Example 1: heap sort(In sorting.md)

youtube: https://www.youtube.com/watch?v=2DmK_H7IdTo

left child: 2i+1

right child:2i+2

parent:  (i-1)/2

```java
// 堆排序
public int[] heapSort(int[] nums){
    if (nums == null || nums.length<=1) {
        return nums;
    }
    // O(NlogN)
    // for (int i=0; i<nums.length;i++) { //O(N)
    //     heapInsert(nums, i); //O(logN)
    // }
    //优化：Floyd建堆法 O(N)
    for (int i=nums.length-1; i>=0; i--) {
        heapify(nums, i, nums.length);
    }  
    int heapsize = nums.length;
    swap1(nums, 0, --heapsize); //将第一位和最后一位进行交换，heapsize-1
    while (heapsize >0) { // O(N)
        heapify(nums, 0, heapsize); // O(logN)
        swap1(nums, 0, --heapsize);  // O(1)
    }
    return nums;
}


// heapinsert insert a number to a max heap, and maintain it
public void heapInsert(int[] nums, int index) { //在index位置插入新数
    while (nums[index] > nums[(index-1)/2]) {
        swap1(nums, index, (index-1)/2);
        index = (index-1)/2;
    }
}

public void heapify(int[] nums, int index, int heapsize) { //从index位置开始heapify，即index处的数变小后重新整理数组恢复为大根堆结构的过程
    int left = 2*index+1;
    while (left< heapsize) { //下方还有孩子的时候
        //储存子结点中更大的数的下标 (有右比大，没右左大)
        int larger = ((left+1<heapsize) && (nums[left+1]>nums[left])) ? (left+1) : left;
        //父节点和较大孩子之间进行比较
        int largest = nums[index]>nums[larger] ? index : larger; 
        if (largest == index) { //若父节点已经比子节点都大，则大根堆结构已完成
            break;
        }
        swap1(nums, index, largest);
        index = largest;
        left = 2*index+1;
    }
}
```



## Example 2: sort an array, move distance less than k

已知一个几乎有序的数组（几乎有序是指，如果把数组顺序排号的话，每个元素移动的距离不超过k，并且k相对于数组来说比较小）。选择合适的算法对数组排序。

思路：
建立一个heapSize为k+1的小根堆，数组中最小的数一定在这个小根堆中；重复操作弹出最小值、小根堆范围在数组中往后挪1位并heapify（滑动窗口），直至heapSize==1；
该方法复杂度为O(Nlog(k+1))；考虑k时相对数组来说很小的数，可以渐进为O(N)

此题可以用java中内置的PriorityQueue作小根堆，因为只需要弹出和添数的操作。
注1：系统内置的堆结构，改中间的值不支持自动调整；只支持弹出和添数。
注2：由数组维持的堆结构，添数时考虑扩容情况（满时新开辟一个length*2的空间，并把原来的全部复制一遍，1->2->4->8…）的时间复杂度仍是O(logN)级别。分析：扩容的次数为logN，乘上N个数复制的总操作是NlogN，但平均到添加每个数每个数就是O(logN)。



```java
public void sortedArrDistanceLessK(int[] arr, int k) {
	PriorityQueue<Integer> minHeap = new PriorityQueue(); //系统默认是小根堆
	int index=0;
	for (; index<=Math.min(arr.length-1, k); index++) {
		minHeap.add(arr[index]);
	}
	int i=0;
	for (; index<arr.length; i++, index++) {
		arr[i]=minHeap.poll();
		minHeap.add(arr[index]);
	}
	while (!minHeap.isEmpty()) {
		arr[i]=minHeap.poll();
	}
}
————————————————
版权声明：本文为CSDN博主「qq_42949310」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_42949310/article/details/125124531
```
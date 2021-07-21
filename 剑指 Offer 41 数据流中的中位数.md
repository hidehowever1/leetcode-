# 数据流中的中位数

> 如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

void addNum(int num) - 从数据流中添加一个整数到数据结构中。
double findMedian() - 返回目前所有元素的中位数。



>>  ### 思路解析：
>>> 针对本题，根据以上思路，可以将数据流保存在一个列表中，并在添加元素时 保持数组有序 。此方法的时间复杂度为 O(N)O(N) ，其中包括： 查找元素插入位置 O(\log N)O(logN) （二分查找）、向数组某位置插入元素 O(N)O(N) （插入位置之后的元素都需要向后移动一位）。

借助 堆 可进一步优化时间复杂度。

建立一个 小顶堆 AA 和 大顶堆 BB ，各保存列表的一半元素，且规定：

AA 保存 较大 的一半，长度为 \frac{N}{2} 
2
N
​
 （ NN 为偶数）或 \frac{N+1}{2} 
2
N+1
​
 （ NN 为奇数）；
BB 保存 较小 的一半，长度为 \frac{N}{2} 
2
N
​
 （ NN 为偶数）或 \frac{N-1}{2} 
2
N−1
​
 （ NN 为奇数）；
随后，中位数可仅根据 A, BA,B 的堆顶元素计算得到。



算法流程：
设元素总数为 N = m + nN=m+n ，其中 mm 和 nn 分别为 AA 和 BB 中的元素个数。

addNum(num) 函数：

当 m = nm=n（即 NN 为 偶数）：需向 AA 添加一个元素。实现方法：将新元素 numnum 插入至 BB ，再将 BB 堆顶元素插入至 AA ；
当 m \ne nm 

​
 =n（即 NN 为 奇数）：需向 BB 添加一个元素。实现方法：将新元素 numnum 插入至 AA ，再将 AA 堆顶元素插入至 BB ；
假设插入数字 numnum 遇到情况 1. 。由于 numnum 可能属于 “较小的一半” （即属于 BB ），因此不能将 numsnums 直接插入至 AA 。而应先将 numnum 插入至 BB ，再将 BB 堆顶元素插入至 AA 。这样就可以始终保持 AA 保存较大一半、 BB 保存较小一半。

findMedian() 函数：

当 m = nm=n（ NN 为 偶数）：则中位数为 (( AA 的堆顶元素 + BB 的堆顶元素 )/2)/2。
当 m \ne nm 

​
 =n（ NN 为 奇数）：则中位数为 AA 的堆顶元素。


>>> #### 代码
```
from heapq import *
class MedianFinder:
    def __init__(self):
        self.A = [] # 小顶堆，保存较大的一半
        self.B = [] # 大顶堆，保存较小的一半

    def addNum(self, num: int) -> None:
        if len(self.A)!=len(self.B):
            heappush(self.B,-heappushpop(self.A,num))
        else:
            heappush(self.A,-heappushpop(self.B,-num))

    def findMedian(self) -> float:
        return self.A[0] if len(self.A) != len(self.B) else (self.A[0] - self.B[0]) / 2.0



# Your MedianFinder object will be instantiated and called as such:
# obj = MedianFinder()
# obj.addNum(num)
# param_2 = obj.findMedian()
```

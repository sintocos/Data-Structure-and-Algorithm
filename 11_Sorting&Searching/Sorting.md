### 基本排序算法


**算法分类**

![avatar](https://img2018.cnblogs.com/blog/849589/201903/849589-20190306165258970-1789860540.png)



**算法复杂度**



![avatar](https://images2018.cnblogs.com/blog/849589/201804/849589-20180402133438219-1946132192.png)

设需要排序的列表是：

```python
list_original = [1, 3, 4, 5, 6, 3, 4, 5, 7, 0, 9, 33,8,10,2,11,17]
```

#### 一、冒泡排序

1.比较相邻的两个元素，如果第一个比第二个大，就交换。

2.比较每一对相邻元素，这样在最后的那个元素就应该是最大的元素

3.针对所有元素重复以上步骤，除了最后一个元素。

4.重复1-3步骤，直到排序完成

```python
def bubble_sort(input_list):
    length = len(input_list)
    for i in range(length - 1):
        for j in range(length - 1 - i):
            if input_list[j] > input_list[j + 1]:  # 如果前者比后者大，则交换
                input_list[j], input_list[j + 1] = input_list[j + 1], input_list[j]
    return input_list

print('bubble_sort:',bubble_sort(list_original))
```




#### 二、选择排序

1.初始状态为R[1..n],有序区为空

2.第i趟排序(i=1,2,3,..,n-1)开始时，当前有序区和无序区分别为R[1,..,i-1]和R[i,..,n],该趟排序从当前的无序区中选出关键字最小的记录R[k],

将其与无序区中的第i个记录R交换，使得R[1,..,i]和R[i+1,..,n]分别变为记录个数增加一个的新有序区和记录个数减少一个的新无序区。

3.n-1趟结束时，数组有序化了

简单来说，就是在无序区域中不断选择最小的数，然后放到有序区域的最后面，有序区越来越大，无序区域越来越小，从而实现从小到大排序

```python
def selection_sort(input_list):
    length = len(input_list)
    for i in range(length - 1):
        min_index = i
        for j in range(i + 1, length):
            if input_list[j] < input_list[min_index]:  # 寻找无序区中最小的值
                min_index = j                          # 将最小的值进行索引
        input_list[i], input_list[min_index] = input_list[min_index], input_list[i]
    return input_list

print('selection_sort:',selection_sort(list_original))
```





#### 三、插入排序
1.从第一个元素开始，认为该元素已经排好了序

2.取出下一个元素，在已经排序的序列中从后往前扫描

3.如果该元素(已排序)大于新元素，将该元素移到下一位置

4.重复步骤3，直到找到已排序的元素小于或者等于新元素的位置

5.将新元素插入到该位置后

6.重复步骤2~5

插入排序在实现上通常采用in-place操作，即只需用到O(1)的额外空间，因而在从后往前扫描过程中，需要反复把已排序元素逐渐向后挪位，为最新元素提供插入空间

```python
def insertion_sort(input_list):
    length = len(input_list)
    for i in range(1,length-1):
        pre_index = i-1
        current = input_list[i]
        while pre_index >= 0 and input_list[pre_index] > current:
            input_list[pre_index+1] = input_list[pre_index]
            pre_index -= 1
        input_list[pre_index+1] = current
    return input_list

print('insertion_sort:',insertion_sort(list_original))
```




#### 四、希尔排序
亦称为递减增量排序算法。将整个待排序的序列分割成为若干子序列分别进行直接插入排序，具体算法描述为：

1.选择一个增量序列t1,t2,...,tk,

2.按增量序列个数k,对序列进行k趟排序

3.每趟排序，根据对应的增量ti,将待排序的学列分割成若干长度为m的子序列，分别对各子表进行直接插入排序。

增量为1时，将整个序列作为一个表来处理，表的长度即为整个序列的长度

```python
def shell_sort(input_list):
    length = len(input_list)
    gaps,gap = [],1
    while gap<length/3:
        gaps += [gap*3+1]
        gap *= 3
    for gap in gaps:
        i = gap
        while i < length:
            temp = input_list[i]
            j = i
            while j >= gap and input_list[j - gap] > temp:
                input_list[j] = input_list[j - gap]
                j -= gap
            input_list[j] = temp
            i += 1
    return input_list

print('shell_sort:',shell_sort(list_original))
```




#### 五、归并排序
建立在归并操作上的算法，采用了分治法(Divide and Conquer),将已经有序的子序列进行合并，得到完全有序的序列。

1.将长度为n的输入序列分成两个长度为n/2的子序列

2.对这两个子序列分别采用归并排序

3.将两个排序好的子序列合并成也给最终的有序序列

```python
def merge_sort(input_list):
    length = len(input_list)
    if length>1:
        middle = length//2
        left_half = merge_sort(input_list[:middle])
        right_half = merge_sort(input_list[middle:])
        i,j,k = 0,0,0
        left_length = len(left_half)
        right_length = len(right_half)
        while i<left_length and j<right_length:
            if left_half[i] < right_half[j]:
                input_list[k] = left_half[i]
                i += 1
            else:
                input_list[k] = right_half[j]
                j += 1
            k += 1
        while i < left_length:
            input_list[k] = left_half[i]
            i += 1
            k += 1
        while j < right_length:
            input_list[k] = right_half[j]
            j += 1
            k += 1
    return input_list

print('merge_sort:',merge_sort(list_original))
```



#### 六、快速排序
快速排序的基本思想是通过一趟排序将待排记录分割成独立的两个部分，其中一部分记录的关键字比另一部分的关键字小，则可分别对这两部分记录继续进行排序

1.从序列中挑选一个元素，称为"基准"(pivot):

2.重新排序，所有元素比基准值小的放在基准的前面，所有元素比基准大的放在后面，相同的数可以放在任意一边。此操作称为分区(partition)操作

3.递归(recursive)地把小于基准值元素地子序列和大于基准值元素地子序列排序

```python
def quick_sort(input_list):
    length = len(input_list)
    if length <= 1:
        return input_list
    else:
        pivot = input_list[0]
        greater = [element for element in input_list[1:] if element>pivot]
        lesser = [element for element in input_list[1:] if element<=pivot]
        return quick_sort(lesser) + [pivot] + quick_sort(greater)

print('quick_sort:',quick_sort(list_original))
```

#### 七、堆排序

堆排序是指利用堆这种数据结构进行排序地算法，堆积是一个类似于完全二叉树地结构，并同时满足堆积的性质，即子节点的键值或索引总是小于(或大于)其父节点

1.将初始待排关键字序列(R1,R2,...,Rn)构建成大顶堆，此堆为初始的无序区

2.将堆顶元素R[1]与最后一个元素R[n]交换，此时得到新的无序区(R1,R2,...,Rn-1)和新的有序区(Rn)，且满足R[1,2,...,n-1]<=R[n]

3.由于交换后新的堆顶R[1]可能违反堆的性质，因此需要对当前的无序区(R1,R2,...,Rn-1)调整为新堆，然后再次将R[1]与无序区最后一个元素交换，

得到新的无序区(R1,R2,...,Rn-2)和新的有序区(Rn-1,Rn)。不断重复此过程直到有序区的元素个数为n-1，则整个排序过程完成。

```python
def heap_helper(input_list,index,heap_size):
    largest = index
    left_index = 2 * index + 1
    right_index = 2 * index + 2
    if left_index < heap_size and input_list[left_index] > input_list[largest]:
        largest = left_index

    if right_index < heap_size and input_list[right_index] > input_list[largest]:
        largest = right_index

    if largest != index:
        input_list[largest], input_list[index] = input_list[index], input_list[largest]
        heap_helper(input_list, largest, heap_size)

def heap_sort(input_list):
    length = len(input_list)
    for i in range(length //2 -1,-1,-1):
        heap_helper(input_list,i,length)
    for i in range(length-1,0,-1):
        input_list[0],input_list[i] = input_list[i],input_list[0]
        heap_helper(input_list,0,i)
    return input_list

print('heap_sort:',heap_sort(list_original))
```



#### 八、计数排序



#### 九、桶排序



#### 十、基数排序



#### 十一、其他排序算法



### References

- https://www.cnblogs.com/onepixel/articles/7674659.html
- https://github.com/TheAlgorithms/Python
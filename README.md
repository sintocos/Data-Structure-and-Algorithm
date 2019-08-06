Data Structure and Algorithm
==

Table of Contents

- [1.General](#1general)
- [2.Sequence](#2sequence)
- [3.Array](#3array)
- [4.Binary](#4binary)
- [5.Dynamic Programming](#5dynamic-programming)
- [6.Graph](#6graph)
- [7.Interval](#7interval)
- [8.Linked List](#8linked-list)
- [9.Math](#9math)
- [10.Matrix](#10matrix)
- [11.Recursion](#11recursion)
- [12.String](#12string)
- [13.Tree](#13tree)
- [14.Trie](#14trie)
- [15.Heap](#15heap)
- [References](#references)



## 1.General

1. **明确假设**：任何在潜意识里面做的假设都要明确下来，因为很多问题故意设置一些特殊情形，不符合一般性的假设。
2. **验证输入**：检查无效/空/负/其他类型的输入。 永远不要假设您获得了有效参数。 或者和面试官确定你是否可以假设输入有效（通常是），这可以让你不用写输入验证的代码，节省时间。
3. **复杂度要求**：确定是否有时间/空间复杂度的要求。
4. [**差一错误**](https://zh.wikipedia.org/wiki/差一错误)：检查计数时边界条件是否判断失误，比如说本应该用"小于等于"最后却使用“小于”。
5. **同一类型**：在没有类型强制的语言中，检查串联起来的值是否是同一类型：int/str/list。
6. **代码测试**：写完代码后，写一些简单的输入来测试代码。
7. **输入预处理**：确定该算法是否要多次运行，比如在一个web服务器中。如果是，则输入可能是可以预处理的，这样的话可以通过预处理来提高效率。
8. **函数式编程**：结合使用函数式编程和命令式编程范式：
   - 尽可能写**纯函数**，纯函数更容易推理，可以帮助减少实现中的错误
   - 尽量**不要改变传递给函数的参数**。如果这些参数是通过引用传递的，更加要注意，除非非常确定。
   - 注意，**函数式编程的空间复杂度通常比较高**，因为non-mutation和对新对象的重复分配。而命令式编程一般更快，因为可以对现有对象进行操作。因此，需要在适当的情况下使用适当的函数式和命令式，在准确性和效率之间取得平衡。
9. **全局变量**：**避免依赖和改变全局变量**。全局变量一般用来设置或者引入状态。如果必须依赖全局变量，确保不要突然地改变它。
10. **时间空间权衡**：通常，为了提高程序的速度，我们可以：1.选择更合适的数据结构/算法；2.使用更多内存。后者是一个经典的空间与时间的权衡问题，但实际上，有一些情况并非只能以牺牲空间为代价来提高速度。另外，需要注意的是，程序运行速度通常存在理论上的限制（就时间复杂度而言）。比如，要求在未排序数组中找到最小/最大元素的问题不可能比O(N)更快。
11. **数据结构选择**：不同的数据结构适用于不同的问题，选择正确的数据结构是解决算法题目的关键。所以我们需要非常熟悉每种数据结构的优势以及各种操作的时间复杂度。
12. **数据结构增强**：可以对数据结构进行增强，以提高不同操作的时间效率。例如，hashmap可以与双向链表一起使用，这样可以在[LRU缓存](https://leetcode.com/problems/lru-cache/)问题中实现“get”和“put”操作的O(1)时间复杂度。
13. **使用hashmap**：它可能是算法问题中最常用的数据结构。如果在某一个问题中卡壳了，可以考虑使用hashmap，或者枚举所有可能的数据结构来考虑哪个比较有用。
14. **阐述想法**：如果代码写不出来，可以和面试官讲一下在非面试环境中(没有时间限制)你会做些什么来解决这个问题。例如，写一个正则表达式来解析这个字符串，而不是使用`split（）`，因为它可能无法涵盖所有情况。

## 2.Sequence

数组和字符串被认为是序列（字符串是一系列字符）。这里有一些处理数组和字符串的小技巧。

1. **重复值**：序列中是否存在重复值，是否会影响最终结果。
2. **越界检查**：检查序列是否越界。
3. **考虑使用索引而不是切片/连接**：不要在序列中随意使用切片或连接。通常，切片和连接序列都需要O(n)的时间。尽可能考虑一下是否可以使用开始和结束索引来划分子数组/子字符串。
4. **反向遍历**：有时需要选择从右侧而不是从左侧遍历序列。
5. [**滑动窗口**](https://discuss.leetcode.com/topic/30941/here-is-a-10-line-template-that-can-solve-most-substring-problems)：该方法适用于大多数子串/子阵列问题。
6. **双指针/索引**：当给定两个要处理的序列时，通常每个序列都有一个索引来遍历或比较。例如，合并两个已排序的数组。

**注意特殊情况：**

 - 空序列。
 - 序列只有一个或者两个元素。
 - 有重复元素的序列。

## 3.Array

1. **是否有序**：数组是完全排好序的还是只有一部分排好序？ 如果是，则应该可以进行某种形式的二分查找。 这通常意味着面试官正在寻找比O(N)更快的解决方案。
2. **是否可排序**：有时先对数组进行排序可以大大简化问题。在尝试排序之前，请确保不需要保留数组元素的原来的顺序。
3. **子数组的和/积问题**：若涉及子数组的求和或乘法，考虑**散列，前缀和，前缀积，后缀和，后缀积**等预计算。
4. **O(1)复杂度**：如果给出一个序列并且要求O(1)空间，则可以将该**数组本身用作哈希表**。例如，如果数组的值都在1到N之中，其中N是数组的长度，则用值作为键可以建立一个哈希表，从而可以O(1)确定一个值存在与否。参考剑指Offer第三题：数组中重复的数字。

**题目**

- [Two Sum](https://leetcode.com/problems/two-sum/)
- [Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)
- [Contains Duplicate III](https://leetcode.com/problems/contains-duplicate-iii/)
- [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
- [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
- [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)
- [Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)
- [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
- [3Sum](https://leetcode.com/problems/3sum/)
- [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

## 4.Binary

**学习链接**

- [Bits, Bytes, Building With Binary](https://medium.com/basecs/bits-bytes-building-with-binary-13cb4289aafa)

有时会涉及**二进制表示**和**按位运算**的问题，必须完全熟悉如何将所选编程语言中的数字从十进制形式转换为二进制形式(反之亦然)。一些基本的技巧如下：

1. 确定第K位是1： `num & (1 << k) != 0`.
2. 设置第K位为1： `num |= (1 << k)`.
3. 设置第K位为0:    `num &= ~(1 << k)`.
4. 反转第K位： `num ^= (1 << k)`.
5. 检查是不是2的次方： `num & (num - 1) == 0`.
6. 异或性质：对于任何数a，都有: 1. **a^a==0**；2. **a^0==a**.

**特殊情况**可能需要：

- 检查上溢/下溢。
- 注意负数。

**题目**

- [Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)
- [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)
- [Counting Bits](https://leetcode.com/problems/counting-bits/)
- [Missing Number](https://leetcode.com/problems/missing-number/)
- [Reverse Bits](https://leetcode.com/problems/reverse-bits/)

## 5.Dynamic Programming

**学习链接**

- [Demystifying Dynamic Programming](https://medium.freecodecamp.org/demystifying-dynamic-programming-3efafb8d4296)
- [Dynamic Programming – 7 Steps to Solve any DP Interview Problem](https://dev.to/nikolaotasevic/dynamic-programming--7-steps-to-solve-any-dp-interview-problem-3870)

1. 动态规划（DP）通常用于求解优化问题。 学习DP的最好方法就是练习，练习可以帮助我们确定这个问题是否可以用DP解决。
2. 有时不需要将整个DP表存储在内存中，存储矩阵的最后两个值或最后两行就足够了。

**题目**

- 0/1 Knapsack
- [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
- [Coin Change](https://leetcode.com/problems/coin-change/)
- [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)
- [Longest Common Subsequence]()
- [Word Break Problem](https://leetcode.com/problems/word-break/)
- [Combination Sum](https://leetcode.com/problems/combination-sum-iv/)
- [House Robber](https://leetcode.com/problems/house-robber/) 
- [House Robber II](https://leetcode.com/problems/house-robber-ii/)
- [Decode Ways](https://leetcode.com/problems/decode-ways/)
- [Unique Paths](https://leetcode.com/problems/unique-paths/)
- [Jump Game](https://leetcode.com/problems/jump-game/)

## 6.Graph

**学习链接**

- [From Theory To Practice: Representing Graphs](https://medium.com/basecs/from-theory-to-practice-representing-graphs-cfd782c5be38)
- [Deep Dive Through A Graph: DFS Traversal](https://medium.com/basecs/deep-dive-through-a-graph-dfs-traversal-8177df5d0f13)
- [Going Broad In A Graph: BFS Traversal](https://medium.com/basecs/going-broad-in-a-graph-bfs-traversal-959bd1a09255)

1. 熟悉图的各种表示方法，图的各种搜索算法及其时间和空间复杂度。

2. 有时会给一个元素为边的列表，需要从这个列表构建一个自己的图然后遍历。常见的图的表示方法有：

   邻接矩阵、邻接列表、hashmaps的hashmap。

3. 一个树状图很可能是一个允许存在环的图，这时简单的递归解决方案是不起作用的。 在这种情况下，需要对环进行处理，并且需要设置一个集合记录遍历时访问过的节点。

**图搜索算法**：

 -  **常见**： 广度优先搜索(Breadth-first Search),  深度优先搜索(Depth-first Search)
 -  **不常见**：拓扑排序，Dijkstra算法
 -  **罕见**：Bellman-Ford算法，Floyd-Warshall算法，Prim算法，Kruskal算法

一般来说，图通常表示为一个2-D矩阵，因此，需要非常熟悉遍历二维矩阵，使用递归遍历时，需要确保下一个点的存在性。有关在矩阵上进行深度优先搜索的更多方法，可以参考[这里](https://discuss.leetcode.com/topic/66065/python-dfs-bests-85-tips-for-all-dfs-in-matrix-question/)。

**特殊情况**

- 空图
- 图只有一个或两个节点
- 非连通图
- 存在环的图

**题目**

- [Clone Graph](https://leetcode.com/problems/clone-graph/)
- [Course Schedule](https://leetcode.com/problems/course-schedule/)
- [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)
- [Number of Islands](https://leetcode.com/problems/number-of-islands/)
- [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)
- [Alien Dictionary (Leetcode Premium)](https://leetcode.com/problems/alien-dictionary/)
- [Graph Valid Tree (Leetcode Premium)](https://leetcode.com/problems/graph-valid-tree/)
- [Number of Connected Components in an Undirected Graph (Leetcode Premium)](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)

## 7.Interval

区间问题(Interval question)就是求多个区间交集并集之类的问题。最典型的就是给一个数组，这个数组里面的每一个单元/元素是一个二元组，二元组里面的两个数一个代表开始值，另一个代表结束值，构成一个区间。 区间问题被认为是数组类问题的一部分，但它们涉及了一些常用的技术，因此在这里单独提及。对于不熟悉区间问题的人来说，区间问题可能有点困难，因为考虑区间重叠时比较麻烦。`[[1,2]，[4,7]]`就是一个区间数组。

1. 需要注意的是，应当向面试官确认“[1,2]”和“[2,3]”是否要视为重叠区间。

2. 一类常见的区间问题就是按每个区间的起始值对区间数组进行排序。

3. 需要熟悉如何检查两个区间是否重叠，以及如何合并两个重叠的区间：

```python
def is_overlap(a, b):
  return a[0] < b[1] and b[0] < a[1]

def merge_overlapping_intervals(a, b):
  return [min(a[0], b[0]), max(a[1], b[1])]
```

**特殊情形**

- 只有一个区间。
 - 没有重叠的区间。
 - 一个区间全部在另一个区间内部。
 - 重复的区间。

**题目**

- [Insert Interval](https://leetcode.com/problems/insert-interval/)
- [Merge Intervals](https://leetcode.com/problems/merge-intervals/)
- [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)
- [Meeting Rooms (Leetcode Premium)](https://leetcode.com/problems/meeting-rooms/) 
- [Meeting Rooms II (Leetcode Premium)](https://leetcode.com/problems/meeting-rooms-ii/)

## 8.Linked List

1. 与数组一样，链表用于表示数据序列。链表的好处是，**从链表中的任何位置插入和删除都是O(1)**，而在数组中，必须移动后续的元素。

2. 在头部或者尾部**添加虚拟节点**可能有助于处理许多必须在头部或尾部执行操作的边缘情况。虚拟节点的存在本质上确保了操作永远不会在头部或尾部执行，从而省却了编写条件检查来**处理空指针**的许多麻烦。一定要记得在最后删除这些虚拟节点。

3. 有时链表问题可以在不需要额外存储的情况下解决。尝试从**链表反向**问题中借鉴思路。

4. 对于链表中的删除，可以修改节点值或修改节点的指针。有时候需要保留对前一个元素的引用。

5. 对于链表划分，可以考虑创建两个单独的链表然后将它们重新连接在一起。

6. 链表问题与数组问题有相似之处，可以从数组中借鉴思路。

7. 对于链表问题，双指针方法比较常见，比如：
   - 获取倒数第K个节点：用两个指针，其中一个指针比另一个指针早K个节点。当前面的指针到达终点时，第二个指针到达了倒数第K个节点。
   - 检测是否存在环：用两个指针，一个步长为1，另一个步长为2。如果两个指针相遇，说明存在环。此外，根据这种方法也可以计算环的起始点位置。
   - 获取中间节点：用两个指针，其中一个指针的步长是另外一个指针的步长的两倍。快指针到达终点时，慢指针刚好在中间节点。

熟悉以下的常规方法，许多链表问题需要使用其中一个或多个方法:

  - 计算链表中的节点数
  - 原地反转链表
  - 使用快/慢指针查找链表的中间节点
  - 合并两个序列

**特殊情形**

- 只有一个节点
- 只有两个节点
- 链表有环

**题目**

- [Reverse a Linked List](https://leetcode.com/problems/reverse-linked-list/)
- [Detect Cycle in a Linked List](https://leetcode.com/problems/linked-list-cycle/)
- [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
- [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
- [Remove Nth Node From End Of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
- [Reorder List](https://leetcode.com/problems/reorder-list/)

## 9.Math

1. 如果代码涉及除法或取模，请注意**除或模为0**的情形。

2. 当一个问题涉及到一个数字的倍数时，考虑取模。
3. 如果使用的是Java/C++之类的类型语言，注意检查**上溢/下溢**。或者和面试官确认一下是否要考虑溢出。

4. **注意浮点数和负数**，面试时可能因为压力而忽略掉这些明显的情况。

5. 如果问题要求实现诸如幂、平方根或除法之类的运算，并且希望它比O(n)更快，考虑**二进制搜索**。
6. 当比较两对点之间的欧氏距离时，使用 dx<sup>2</sup> + dy<sup>2</sup>就足够了，没有必要求平方根。
7. 要确定两个圆是否重叠，请检查圆的两个中心之间的距离是否小于它们的半径之和。
8. 熟悉等比数列求和公式、三角函数和差化积等基础数学知识，概率问题则需熟悉排列、组合、全概率公式、贝叶斯公式、期望、几何概型等，此外，需要回顾一下数学分析里面微分积分方法、常用求导公式等。

**一些常用公式**：

- 1到N的和： (n+1) * n/2。
- 等比数列求和 2<sup>0</sup> + 2<sup>1</sup> + 2<sup>2</sup> + 2<sup>3</sup> + ... 2<sup>n</sup> = 2<sup>n+1</sup> - 1。
- 抛硬币连续k面向上的次数期望：E=1/p +  (1/p)<sup>2</sup> + ... + (1/p)<sup>k</sup>，这里p=1/2。 
- N个元素的排列： N! / (N-K)!。
- N个元素的组合： N! / (K! * (N-K)!)。

**题目**

- [Pow(x, n)](https://leetcode.com/problems/powx-n/)
- [Sqrt(x)](https://leetcode.com/problems/sqrtx/)
- [Integer to English Words](https://leetcode.com/problems/integer-to-english-words/)

## 10.Matrix

1. 矩阵是二维数组。 涉及矩阵的问题通常与动态规划或图遍历有关。
2. 对于涉及遍历和动态规划的问题，一般是创建具有相同维度的矩阵的副本，该矩阵被初始化为空值以存储访问的状态或动态规划表。需要熟悉下面的常规方法：

```python
rows, cols = len(matrix), len(matrix[0])
copy = [[0 for _ in range(cols)] for _ in range(rows)]
```

3. 许多基于网格的游戏可以被建模为矩阵，例如Tic-Tac-Toe，Sudoku，Crossword，Connect 4，Battleship等。此外，一般还需要写代码验证游戏的获胜条件。对于像Tic-Tac-Toe，Connect 4和Crosswords这样的游戏，必须在垂直和水平方向都进行验证。一个简单的技巧就是编写代码验证矩阵的水平单元，然后转置矩阵再验证一遍。在Python里面对矩阵进行转置非常容易：

```python
transposed_matrix = zip(*matrix)
```

**特殊情况**

- 空矩阵，检查row或col是否为0。
- 1*1矩阵。
- 只有一行或只有一列的矩阵。

**题目**

- [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)
- [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)
- [Rotate Image](https://leetcode.com/problems/rotate-image/)
- [Word Search](https://leetcode.com/problems/word-search/)

## 11.Recursion

1. 递归对于变换/重排等非常有用，因为它会生成基于树的问题集以及所有的组合情况。注意一定要生成所有的情况，不要漏掉，另外要记得处理重复的情况或不符合条件的情况。 

2. 要始终记得定义一个**基本情形**或递归出口，以便结束递归。

3. 递归实际上隐式地使用了堆栈。 因此，可以使用堆栈来重写所有递归方法，将其变为迭代/循环。注意递归的层次过深会导致堆栈溢出(Python中的默认限制为1000)。 
4. 一般来说，递归容易理解，而迭代比较快。可以参考[这里的讨论](https://www.zhihu.com/question/20418254)来深入理解两者的联系和区别。
5.  递归永远不可能是O(1)空间复杂度，因为涉及堆栈，除非有[尾调优化](https://stackoverflow.com/questions/310974/what-is-tail-call-optimization)(TCO)。了解哪些语言支持TCO。了解尾递归(Tail Recursion)，Continuation Passing Style(CPS)。

**题目**

- [Subsets](https://leetcode.com/problems/subsets/) 
- [Subsets II](https://leetcode.com/problems/subsets-ii/)
- [Strobogrammatic Number II (Leetcode Premium)](https://leetcode.com/problems/strobogrammatic-number-ii/)

## 12.String

1. 字符串是序列的一种，前面的第2节Sequence也适用于字符串。

2. 注意输入字符串的字符集是什么以及是否区分大小写。通常来说是小写的a到z。

3. 如果比较字符串时不用在乎其内部字符的顺序(如易位构词anagram)时，可以考虑使用HashMap/Dictionary来作为计数的数据结构。 在Python里面内置Counter类，可以直接用Counter。

4. 如果要对字符计数，要注意计数器的空间复杂度并不是O(n)，而是O(1)。因为字符一般是26个小写英文字母。

5. 可以快速查找字符的数据结构有：1.字典树(前缀树)  2.后缀树

6. 常见的字符串算法有：1.Rabin Karp(用滚动hash快速查找子串)  2.KMP(用于快速查找子串)

**特殊情形**

- 字符串只由一个字符构成，比如b, aaaa这种的。

**无重复字符的字符串**

 - 使用一个26位掩码来确定字符串里面包含了哪个小写英文字母

```python
mask = 0
for c in set（word）：
   mask | =（1 <<（ord（c） -  ord（'a'）））
```

要确定两个字符串是否具有公共字符，可以对两个位掩码执行＆。 如果结果为非零，`mask_a＆mask_b> 0`，则两个字符串具有公共字符。

**易位构词**

易位构词(Anagram)就是对字符串里面的元素进行重排得到新字符串或者新词，要求只使用原来的字母一次。通常比较困难的是那些单词之间没有空格的字符串。

要确定两个字符串是否是Anagram，有以下方法：

 - 对两个字符串进行排序，看看产生的字符串是否相同，需要O(nlgn)时间和O(lgb)空间。
 - 将每个字符映射到一个特定的素数上，然后将字符串中每个字符映射得到的数字全部相乘得到积，看两个字符串各自的积是否相同，因为Anagram应该具有相同的积(素因子分解唯一)，需要O(n)时间和O(1)空间。
 - 用hashmap/字典对字符串里面的字符进行频率计数，同样需要O(n)时间和O(1)空间。

**回文**

1. 回文是一个单词/短语/数字或者其他字符序列，这个序列从前往后读，与从后往前读是一样的。比如madam，12321，racecar等。

2. 判断一个字符串是否为回文字符串的方法有以下几种：

 - 反转字符串，看是否相等，其实就是用回文的定义来检查。
 - 设置开头和结尾两个指针，两个指针向中间移动直到它们相遇。在任何时间点，两个指针所访问的字符都应该一样。

3. 回文中字符的顺序是关键，所以HashMaps在这里用处不大。

4. 如果要计算回文串的数量，一个常见的技巧就是让两个指针从中间往外移动。注意，回文的长度可以是偶数或奇数，所以对于每个待判定串的中间位置，需要检查两次：一次包含那个字符，一次不包含那个字符。

 - 对于子字符串，一旦出现不一样，就可以提前终止。
 - 对于子序列，要使用动态规划，因为有重合子问题，比如[这个问题](https://leetcode.com/problems/longest-palindromic-subsequence/)。

**题目**

- [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
- [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)
- [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/description/)
- [Valid Anagram](https://leetcode.com/problems/valid-anagram)
- [Group Anagrams](https://leetcode.com/problems/group-anagrams/)
- [Valid Parentheses](https://leetcode.com/problems/valid-parentheses)
- [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
- [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)
- [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)
- [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)
- [Encode and Decode Strings (Leetcode Premium)](https://leetcode.com/problems/encode-and-decode-strings/)

## 13.Tree

**学习链接**

- [Leaf It Up To Binary Trees](https://medium.com/basecs/leaf-it-up-to-binary-trees-11001aaf746d)

1. 树是**无向**且**连通**的**非循环图**。

2. 递归是树的常用方法。如果子树问题可用于解决整个问题时，考虑用递归。

3. 使用递归时，要记得检查基本情形，尤其是节点为“null”。

4. 要求逐层遍历树时，用广度优先搜索。

5. 有时递归函数需要返回两个值。

6. 如果问题涉及沿途访问的节点的总和，要考虑节点为负数的情况。

7. 熟悉前序、中序和后序遍历，一般用递归实现。 但是有的时候用递归实现了遍历，面试官还会要求写一下用迭代来遍历。

**特殊情况**

- 树是空的
- 树只有一个或两个节点
- 很偏的树(比如根节点左边有N个点，而根节点右边甚至每个节点的右边都只有一个点)

**二叉树**

1. 中序遍历+前序遍历或者中序遍历+后序遍历都可以唯一地确定一棵树，但仅仅中序或者前序+后序不行。
2. 熟悉由中序+前序生成后序遍历，或者由中序+后序生成前序遍历序列。

**二叉搜索树** 

1. 熟悉中序遍历BST(二叉搜索树)，这样可以按顺序得到所有元素。

2. 熟悉BST的性质，并知道如何验证一个二叉树是BST。 

3. 涉及BST的问题一般要求比O(n)更快的解决方法。

**题目**

- [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
- [Same Tree](https://leetcode.com/problems/same-tree/)
- [Invert/Flip Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
- [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
- [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)
- [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)
- [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
- [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
- [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
- [Lowest Common Ancestor of BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

## 14.Trie

**学习链接**

- [Trying to Understand Tries](https://medium.com/basecs/trying-to-understand-tries-3ec6bede0014)
- [Implement Trie (Prefix Tree)](https://leetcode.com/articles/implement-trie-prefix-tree/)

1. Trie(字典树或前缀树)是一种特殊的树，一般用于搜索和存储字符串。 
2. Trie有许多实际的应用，比如搜索和自动补全，需要熟悉这些应用，这样就会知道什么时候应该用Trie。

3. 碰到由单词构成的列表，可以将其预处理成Trie，这样可以在n个单词中快速找到长度为k的单词。搜索的时间复杂度由O(n)变为O(k)。

4. 熟悉如何从头开始实现一个`Trie`类，同时要熟悉其`add`，`remove`和`search`方法。

**题目**

- [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree)
- [Add and Search Word](https://leetcode.com/problems/add-and-search-word-data-structure-design)
- [Word Search II](https://leetcode.com/problems/word-search-ii/)

## 15.Heap

**学习链接**

- [Learning to Love Heaps](https://medium.com/basecs/learning-to-love-heaps-cef2b273a238)

1. 如果问题是关于最大或者最小的K个元素，可以考虑使用堆，例如 [频率最多的K个元素](https://leetcode.com/problems/top-k-frequent-elements/).。

2. 如果是Top K问题，使用大小为K的最小堆。遍历每个元素，将其加入堆中，当堆的大小超过K时，删除最小元素，这可以保证始终拥有K个最大元素。

**题目**

- [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
- [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
- [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## References

- https://github.com/yangshun/tech-interview-handbook
- http://blog.triplebyte.com/how-to-pass-a-programming-interview
- https://quip.com/q41AA3OmoZbC
- http://www.geeksforgeeks.org/must-do-coding-questions-for-companies-like-amazon-microsoft-adobe/
- https://medium.com/basecs
- https://wizardforcel.gitbooks.io/the-art-of-programming-by-july/content/
- [https://www.weiweiblog.cn/category/%e7%ae%97%e6%b3%95%e5%ad%a6%e4%b9%a0/](https://www.weiweiblog.cn/category/算法学习/)



其他待补充链接

1. Hackerrank interview preparation, 
3. 剑指Offer, 
4. Leetcode分类刷题顺序https://cspiration.com/leetcodeClassification

5. NumPy能力大评估：70道测试题https://www.jiqizhixin.com/articles/030201
6. Python面试 https://github.com/taizilongxu/interview_python
7. 2019各公司面试题目 https://github.com/0voice/interview_internal_reference
8. 写一个简单的解释器: lsbasi
9. Python写一个Python解释器 qingyunha.github.io/taotao
10. github上的CS_Notes
11. 编程之法 面试和算法心得https://wizardforcel.gitbooks.io/the-art-of-programming-by-july/content/

12. 脑图：玩转AI面试

13. 自旋锁、阻塞锁、可重入锁、悲观锁、乐观锁、读写锁、偏向所、轻量级锁、重量级锁、锁膨胀、对象锁和类锁https://blog.csdn.net/a314773862/article/details/54095819

14. python自测100题 https://zhuanlan.zhihu.com/p/57991045
15. BAT机器学习面试1000题 https://zhuanlan.zhihu.com/c_140166199
16. 《Java编程的逻辑》文章列表https://www.cnblogs.com/swiftma/p/5631311.html
17. Python Coding Interview 编程面试 https://realpython.com/python-coding-interview-tips/
18. Python魔术方法指南 https://pycoders-weekly-chinese.readthedocs.io/en/latest/issue6/a-guide-to-pythons-magic-methods.html
19. Python按位运算基础 http://yshblog.com/blog/92
20. 数据结构和算法https://alleniverson.gitbooks.io/data-structure-and-algorithms/content/
21. C++ Primer 笔记 https://github.com/chuenlungwang/cppprimer-note
22. 海量数据处理方法总结和面试题 https://blog.csdn.net/v_JULY_v/article/details/6279498
23. Linux IO模式及 select、poll、epoll详解https://segmentfault.com/a/1190000003063859
24. 推荐系统方面的博客 https://www.jianshu.com/nb/21403842
25. 特征工程https://www.cnblogs.com/jasonfreak/p/5448385.html
26. 算法珠玑 Leetcode C++版https://soulmachine.gitbooks.io/algorithm-essentials/content/cpp/
27. AI算法工程师手册 http://www.huaxiaozhuan.com/
28. HTTP常见知识点https://juejin.im/post/5d0de954e51d4556be5b3a6f#heading-17
29. 每周一练-数据结构与算法 https://juejin.im/user/586fc337a22b9d0058807d53/posts
30. Http服务器tinyhttpd的详细注释版https://github.com/cbsheng/tinyhttpd

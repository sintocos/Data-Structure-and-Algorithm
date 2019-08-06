## **Array**

[TOC]

### 1. [Two Sum](https://leetcode.com/problems/two-sum/) [easy]

**题目**：给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例: 给定 nums = [2, 7, 11, 15], target = 9。因为 nums[0] + nums[1] = 2 + 7 = 9，所以返回 [0, 1]

**分析**：一种思路是先排序，然后设置两个指针begin和end，如果nums[begin]+nums[end]大于target，则end-=1，否则begin+=1，直到nums[begin]+nums[end]等于target。时间复杂度为O(NlogN)，空间复杂度为O(1)。

另一种思路是直接hash成字典，然后判断target-num[i]是否已经在字典中。时间复杂度和空间复杂度均为O(N)。

注意题目中已说明有且仅有唯一解。

**代码**：

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        num_dict = {}
        for i in range(len(nums)):
            if target - nums[i] in num_dict:
                return [num_dict[target - nums[i]], i]
            else:
                num_dict[nums[i]] = i
```



### 2. [Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/) [hard]

**题目**：给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。

注意: 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

示例 1:输入: [3,3,5,0,0,3,1,4]，输出: 6
解释: 在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。

示例 2:输入: [1,2,3,4,5]，输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。 注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。

示例 3:输入: [7,6,4,3,1] ，输出: 0 
解释: 在这个情况下, 没有交易完成, 所以最大利润为 0。

**分析**：由于最多可以进行两笔交易，考虑划分区间，分别计算两个区间上的最大利润，然后遍历划分点即可，其本质是动态规划。另外[Best Time to Buy and Sell Stock I](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-i/)是本问题的子问题，而[Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)可以将价格看成折线，直接将所有上升的差值加起来即可，因为要先买后卖。

更全面更通用的方法是从状态考虑，构建状态转移方程，不过比较复杂，参考[一个通用方法团灭 6 道股票问题](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/solution/yi-ge-tong-yong-fang-fa-tuan-mie-6-dao-gu-piao-wen/)。

**代码**：

```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        n = len(prices)
        if n < 2:
            return 0

        dp1, dp2 = [0] * n, [0] * n
        buy1, sell2, res = prices[0], prices[-1], 0

        for i in range(1, n):  # 第一个区间，从前往后遍历
            buy1 = min(buy1, prices[i])
            dp1[i] = max(dp1[i - 1], prices[i] - buy1)

        for i in range(n - 2, -1, -1):  # 第二个区间，反向遍历
            sell2 = max(sell2, prices[i])  
            dp2[i] = max(dp2[i + 1], sell2 - prices[i])

        for profit1, profit2 in zip(dp1, dp2):  # 获取最优值
            res = max(res, profit1 + profit2)
        return res
```



### 3. [Contains Duplicate III](https://leetcode.com/problems/contains-duplicate-iii/) [medium]

**题目**：给定一个整数数组，判断数组中是否有两个不同的索引 i 和 j，使得 nums [i] 和 nums [j] 的差的绝对值最大为 t，并且 i 和 j 之间的差的绝对值最大为 ķ。

示例 1: 输入: nums = [1,2,3,1], k = 3, t = 0，输出: true

示例 2: 输入: nums = [1,0,1,1], k = 1, t = 2，输出: true

示例 3: 输入: nums = [1,5,9,1,5,9], k = 2, t = 3，输出: false

**分析**：这个问题实际上是给定了两个约束条件，一个是序号的范围，一个是值的范围。我们需要解决的问题是：

1. 对于给定的元素 x, 在窗口中是否有存在区间 [x−t,x+t] 内的元素？
2. 我们能在常量时间内完成以上判断嘛？

由于区间是定长的，所有联想到桶排序。桶排序是一种把元素分散到不同桶中的排序算法。把元素分到有序的桶中或者区间中，然后每个桶再独立地用不同的排序算法进行排序，最后按照桶的顺序收集所有的元素就可以得到一个有序的数组了。

在这个问题中，可以设计一些桶，让他们分别包含区间 [0,t], [t+1, 2t+1], ......，然后把桶当做窗口。对于每一个数，只需要检查其所属的那个桶和相邻的两个桶中的元素就可以了，因为候选者只有可能在这些桶中。从而可以在常量时间内进行窗口搜索。

还有一件值得注意的事，这个问题和桶排序的不同之处在于每次我们的桶里只需要包含最多一个元素就可以了，因为如果任意一个桶中包含了两个元素，那么这也就是意味着这两个元素是足够接近的 了，这时候就直接得到答案了。因此，我们只需使用一个标签为桶序号的散列表就可以了。在这里，我们设置k个桶来保证|i-j|<=k。

时间复杂度：O(n)，空间复杂度：O(min(n,k))

另外，对于[Contains Duplicate I](https://leetcode.com/problems/contains-duplicate/)只需要返回len(nums) != len(set(nums))，或者设置一个hash表/set，遍历nums，如果在里面返回True，不在则往set里面添加元素，或者先排序然后判断相邻元素是否相等。

而对于[Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)，则是用散列表来维护一个k大小的滑动窗口。

**代码**：

```python
class Solution(object):
    def containsNearbyAlmostDuplicate(self, nums, k, t):
        """
        :type nums: List[int]
        :type k: int
        :type t: int
        :rtype: bool
        """
        buckets = {}
        for i, v in enumerate(nums):
        # t == 0 is a special case where we only have to check the bucket that v is in.
            bucketNum, offset = (v / t, 1) if t else (v, 0)
            for idx in range(bucketNum - offset, bucketNum + offset + 1):
                if idx in buckets and abs(buckets[idx] - nums[i]) <= t:
                    return True

            buckets[bucketNum] = nums[i]
            if len(buckets) > k:
                # Remove the bucket which is too far away. Beware of zero t.
                del buckets[nums[i - k] / t if t else nums[i - k]]

        return False
```



### 4. [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) [medium]

**题目**：给定长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。

示例: 输入: [1,2,3,4]，输出: [24,12,8,6]。

说明: 请不要使用除法，且在 O(n) 时间复杂度内完成此题。进一步思考，你可以在常数空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组不被视为额外空间。）

**分析**：暴力方法是直接乘得到每个output[i]，但是其中有些乘积的信息被浪费了，其时间复杂度为O(N^2)。考虑构建矩阵如下：

|   output[1]   |      1      |   nums[2]   |   nums[3]   |   nums[4]   |   nums[5]   |
| :-----------: | :---------: | :---------: | :---------: | :---------: | :---------: |
| **output[2]** | **nums[1]** |    **1**    | **nums[3]** | **nums[4]** | **nums[5]** |
| **output[3]** | **nums[1]** | **nums[2]** |    **1**    | **nums[4]** | **nums[5]** |
| **output[4]** | **nums[1]** | **nums[2]** | **nums[3]** |    **1**    | **nums[5]** |
| **output[5]** | **nums[1]** | **nums[2]** | **nums[3]** | **nums[4]** |    **1**    |

即每一行的第一个元素都等于这一行剩下所有元素的乘积，从而先计算矩阵的下三角，再计算矩阵的上三角，每一次乘积的信息都用到了而没被浪费，其时间复杂度是O(N)。

**代码**：

```python 
class Solution(object):
    def productExceptSelf(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        n = len(nums)
        res = [1] * n

        for i in range(n - 1):
            res[i + 1] = res[i] * nums[i]

        tmp = 1
        for i in range(n - 2, -1, -1):
            tmp *= nums[i + 1]
            res[i] *= tmp
        return res
```



### 5. [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) [easy]

**题目**：给一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少含一个元素），返回其最大和。

示例: 输入: [-2,1,-3,4,-1,2,1,-5,4]，输出: 6。解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

说明：如果你已经实现复杂度为 O(*n*) 的解法，尝试使用更为精妙的分治法求解。

**分析**：最直接的想法是采用动态规划，设置一个DP数组，记录当前的最大和，其状态转移方程是DP[i] = max(DP[i-1]+nums[i], nums[i])，简化一下，可以达到时间复杂度为O(n) ，空间复杂度为O(1)，是最优的。

另外一种方法是分治，对于任何一个array来说，有三种可能： 

1. 最大子序列和落在它的左边； 
2. 最大子序列和落在它的右边； 
3. 最大子序列和横跨左右两边。

对于第1,2情况，利用二分法就很容易得到，base case 是如果只有一个数字了，那么就返回。

对于第3种情况，先从右到左计算左边的最大子序和，再从左到右计算右边的最大子序和，再相加。

而且对于左半部分或者右半部分，上述结论也成立，利用这特性，可以写出相应的递归，递归结束的条件是当只有一个元素时，判断这个元素是否大于零，大于零便返回这个数，否则返回零。 

然后求出左边最大值，右边最大值和横跨两边的最大值，返回这三个值中的最大值。

这种分治递归法时间复杂度为O(NlogN)，空间复杂度为 O(logN)。

**代码**：

```python
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        maxEndingHere = maxSoFar = nums[0]
        for i in range(1, len(nums)):
            maxEndingHere = max(maxEndingHere + nums[i], nums[i])
            maxSoFar = max(maxEndingHere, maxSoFar)
        return maxSoFar
```



### 6. [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/) [medium]

**题目**：给定一个整数数组 nums ，找出一个序列中乘积最大的连续子序列（该序列至少包含一个数）。

示例 1: 输入: [2,3,-2,4]，输出: 6。解释: 子数组 [2,3] 有最大乘积 6。
示例 2: 输入: [-2,0,-1]，输出: 0。解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。

**分析**：最直接的想法就是动态规划。由于存在负整数，所以当前最大的在随后可能变成最小的，而当前最小的可能在随后变成最大的，因此要维护两个数组，一个存储到当前位置的最小的子数组乘积，一个存储最大的子数组乘积。可以用另外一个变量存储要输出的最大乘积，从而这两个数组可以简化成两个变量。

此外，由于0的存在，要判断是否要重新开始乘，因此要加入对nums[i]的比较。

f(k) = max(f(k-1) * A[k], A[k], g(k-1) * A[k]), g(k) = min(g(k-1) * A[k], A[k], f(k-1) * A[k])。

算法的时间复杂度是O(N)，空间复杂度是O(1)。

**代码**：

```python
class Solution(object):
    def maxProduct(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        tmp_max = tmp_min = res = nums[0]
        for i in range(1, len(nums)):
            prior_max, prior_min = tmp_max, tmp_min  # 记录上一次的值，用于更新
            tmp_max = max(max(prior_max * nums[i], nums[i]), prior_min * nums[i])
            tmp_min = min(min(prior_min * nums[i], nums[i]), prior_max * nums[i])
            res = max(res, tmp_max)
        return res
```



### 7. [Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/) [hard]

**题目**：假设按照升序排序的数组在预先未知的某个点上进行了旋转。例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] 。请找出其中最小的元素。注意数组中可能存在重复的元素。

示例 1：输入: [1,3,5]，输出: 1。

示例 2：输入: [2,2,2,0,1]，输出: 0。

**分析**：暴力的方法是遍历。考虑到升序排序，可以用二分搜索，关键是要注意那个变化点。

另外对于[Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)，其已经说明无重复元素，更为简单。

两个题目用二分搜索，时间复杂度都是O(logN)，空间复杂度都是O(1)。

**代码**：

```python
class Solution(object):
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        left = 0
        right = len(nums) - 1
        while left < right and nums[left] >= nums[right]:
            mid = (left + right) // 2
            if nums[mid] > nums[right]:
                left = mid + 1
            elif nums[mid] < nums[left]:
                right = mid
            else:
                # nums[left] = nums[right] = nums[mid]的情形，该行亦可改为left += 1
                right -= 1
        return nums[left]
```



### 8. [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) [medium]

**题目**：假设按照升序排序的数组在预先未知的某个点上进行了旋转。( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。你的算法时间复杂度必须是 O(logN) 级别。

示例 1: 输入: nums = [4,5,6,7,0,1,2], target = 0，输出: 4。
示例 2: 输入: nums = [4,5,6,7,0,1,2], target = 3，输出: -1。

**分析**：注意到原数组为有限制的有序数组且要求O(logN)，很容易想到二分查找。一种直接的方法是先用二分查找找到旋转的下标 `rotation_index` ，也就是数组中最小的元素。然后再在选中的数组区域中再次使用二分查找。

但是还有另外一种方法更加简洁。考虑以下三种情况：

1. nums[0] <= nums[i]，那么在0-i区间内没有发生旋转，即nums[0] 到 nums[i] 为有序数组。所以当 nums[0] <= target <= nums[i] 时可以在0-i范围内查找；

2. nums[i] < nums[0]，那么在0−i区间的某个点处发生了旋转，那么i+1 到最后一个数字的区间为有序数组，并且所有的数字都是小于nums[0] 且大于 nums[i]。所以当target不属于 nums[0] 到 nums[i] 时（target <= nums[i] < nums[0] 或者 nums[i] < nums[0] <= target），可以在0−i 区间内查找。
   上述三种情况可以总结如下：

    nums[0] <= target <= nums[i]
               target <= nums[i] < nums[0]
                         nums[i] < nums[0] <= target

以上这些情况都要在0-i区间内搜索，即将mid赋值给right。反之，若不是这些情况，则需要在i+1到最后这个区间内搜索，即将mid+1赋值给left。

接下来就要判断当前状态是否是上述的情况。所以要进行三项判断：(nums[0] <= target)，(target <= nums[i]) ，(nums[i] < nums[0])，这三项中有一项还是两项为真（明显这三项不可能均为真或均为假，因为这三项可能已经包含了所有情况）。两项为真时，说明当前状态是上述三种情况之一，在0-i之间搜索。一项为真时，则相反。

现在只需要区别出这三项中有两项为真还是只有一项为真。可以使用 “异或” 操作可以轻松的得到上述结果（两项为真时异或结果为假，一项为真时异或结果为真）。即：

1. 三项判断的异或的结果为假时，说明有两项判断为真，说明当前状态是上述三种情况之一，说明要在0-i内搜索，从而将i赋值给right。
2. 三项判断的异或的结果为真时，说明有一项判断为真，说明当前状态不在上述三种情况中，说明要在i+1到最后这个区间内搜索，从而将i+1赋值给left。

之后我们通过二分查找不断缩小 target 可能位于的区间，直到 left==right，此时如果 nums[left]==target 则找到了，如果不等则说明该数组里没有此项。[参考链接](https://leetcode-cn.com/problems/two-sum/solution/ji-jian-solution-by-lukelee/)。

**代码**：

```python
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        length = len(nums)
        left, right = 0, length - 1
        while left < right:
            mid = (left + right) // 2
            if ((nums[0] <= target) ^ (target <= nums[mid]) ^ (nums[mid] < nums[0])):
                left = mid + 1
            else:
                right = mid
            print(left, mid, right)

        return left if (left == right and nums[left] == target) else -1

```



### 9. [3Sum](https://leetcode.com/problems/3sum/) [medium]

**题目**：给定一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？找出所有满足条件且不重复的三元组。

说明：答案中不可以包含重复的三元组。例如, 给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：[  [-1, 0, 1],  [-1, -1, 2] ]。

**分析**：3Sum可以转化成一个2Sum的题，先从数组中选一个数，并将目标数减去这个数，得到一个新目标数。然后再在剩下的数中找一对和是这个新目标数的数，其实就转化为2Sum了

首先对数组进行排序，排序后固定一个数nums[i]，再使用左右指针指向nums[i]后面区间的两端，分别为nums[L]和nums[R]，计算三个数的和sum，判断其是否为0，满足则添加进结果集。如果nums[i]大于 0，由于已经排序了，nums[L]和nums[R]必然大于nums[i]，所以三数之和必然大于0，直接结束循环。

为了避免得到重复结果，不仅要保证2Sum找的范围要是在最先选定的那个数之后的，而且要跳过重复元素。

1. 如果nums[i] == nums[i-1]，则说明该数字重复，会导致结果重复，所以跳过。
2. 当sum == 0时，如果nums[L] == nums[L+1]，会导致结果重复，所以跳过，L+=1。
3. 当sum == 0时，如果nums[R]== nums[R-1]，会导致结果重复，所以跳过，R-=1。

时间复杂度：O(N^2)，空间复杂度为O(1)。

此外，对于相类似的[3Sum Closest](https://leetcode.com/problems/3sum-closest/)，也可以采用这种双指针方法，只是不需要去重，只需要判断tmp_sum和res哪个更靠近target，然后更新res即可，复杂度和3Sum一致。

对于[4Sum](https://leetcode.com/problems/4sum/)，也是类似的方法，遍历前两个，然后转化为2Sum，用双指针求解。时间复杂度为O(N^3)，只是要注意去重。如果考虑使用Python中的itertools，也可以做到O(N^2)，参考[这种方法](https://leetcode-cn.com/problems/4sum/solution/5-xing-python-on2-by-qqqun902025048/)。

**代码**：

```python
class Solution(object):
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        nums.sort()
        length = len(nums)
        res = []

        for i in range(length - 2):
            if nums[i] > 0:
                break
            if i > 0 and nums[i] == nums[i - 1]:  # 去重
                continue

            L, R = i + 1, length - 1
            while L < R:
                tmp_sum = nums[i] + nums[L] + nums[R]
                if tmp_sum == 0:
                    res.append([nums[i], nums[L], nums[R]])
                    while L < R and nums[L] == nums[L + 1]:  # 去重
                        L += 1
                    while L < R and nums[R] == nums[R - 1]:  # 去重
                        R -= 1
                    L += 1
                    R -= 1
                elif tmp_sum > 0:
                    R -= 1
                else:
                    L += 1
        return res

```



### 10. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) [medium]

**题目**：给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

示例: 输入: [1,8,6,2,5,4,8,3,7]，输出: 49。

说明：你不能倾斜容器，且 *n* 的值至少为 2。

**分析**：对于这种区间问题，第一个容易想到的就是双指针法。以i,j表示前后指针，H[i]表示位置i处的高度。从而S(i,j) = min(H[i],H[j]) * (j - i)是(i,j)对应的水容器的面积。设置两指针i,j分别位于容器壁两端，逐渐向中间收缩并记录最大值。每次选定H[i], H[j]中**较小**的所对应的指针，向中间收缩，这是因为：

如果将指向较长线段的指针向中间移动，矩形区域的面积将受限于较短的线段而不会获得任何增加。但是，在同样的条件下，移动指向较短线段的指针尽管造成了矩形宽度的减小，但却可能会有助于面积的增大。因为移动较短线段的指针可能会得到一条相对较长的线段，这可以克服由宽度减小而引起的面积减小。

该方法的正确性证明可以参考[这篇文章](https://leetcode-cn.com/problems/container-with-most-water/solution/shuang-zhi-zhen-fa-zheng-que-xing-zheng-ming-by-r3/)。时间复杂度是O(N)，空间复杂度是O(1)。

此外，若要找面积最小的水槽，直接找高度H最小的板子，并且宽度为1即可。

**代码**：

```python
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        length = len(height)
        left, right = 0, length - 1
        res = 0

        while left < right:
            res = max(res, min(height[left], height[right]) * (right - left))
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1
        return res
```


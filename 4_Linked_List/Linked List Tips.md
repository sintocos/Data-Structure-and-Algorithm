## **Linked List Tips**

[TOC]

链表是面试过程中经常被问到的，这里把剑指offer 和 LeetCode 中的相关题目做一个汇总，方便复习。

#### 1. 在 O(1) 时间删除链表节点

**题目描述**：给定单向链表的头指针和一个节点指针，定义一个函数在O(1)时间删除该节点。
**解题思路**：常规的做法是从链表的头结点开始遍历，找到需要删除的节点的前驱节点，把它的 next 指向要删除节点的下一个节点，平均时间复杂度为O(n)，不满足题目要求。
那是不是一定要得到被删除的节点的前一个节点呢？其实不用的。我们可以很方面地得到要删除节点的下一个节点，如果我们把下一个节点的内容复制到要删除的节点上覆盖原有的内容，再把下一个节点删除，那就相当于把当前要删除的节点删除了。举个栗子，我们要删除的节点i，先把i的下一个节点j的内容复制到i，然后把i的指针指向节点j的下一个节点。此时再删除节点j，其效果刚好是把节点i给删除了。
要注意两种情况：

1. 如果链表中只有一个节点，即头节点等于要删除的节点，此时我们在删除节点之后，还需要把链表的头节点设置为NULL。
2. 如果要删除的节点位于链表的尾部，那么它就没有下一个节点，这时我们就要从链表的头节点开始，顺序遍历得到该节点的前序节点，并完成删除操作。

**参考代码**

```java
public static ListNode deleteNode(ListNode head, ListNode toBeDeleted) {  
    // 如果输入参数有空值就返回表头结点  
    if (head == null || toBeDeleted == null) {  
        return head;  
    }  
    // 如果删除的是头结点，直接返回头结点的下一个结点  
    if (head == toBeDeleted) {  
        return head.next;  
    }  
      // 下面的情况链表至少有两个结点  
    // 在多个节点的情况下，如果删除的是最后一个元素  
    if (toBeDeleted.next == null) {  
        // 找待删除元素的前驱  
        ListNode tmp = head;  
        while (tmp.next != toBeDeleted) {  
            tmp = tmp.next;  
        }  
        // 删除待结点  
        tmp.next = null;  
    }  
    // 在多个节点的情况下，如果删除的是某个中间结点  
    else {  
        // 将下一个结点的值输入当前待删除的结点  
        toBeDeleted.value = toBeDeleted.next.value;  
        // 待删除的结点的下一个指向原先待删除引号的下下个结点，即将待删除的下一个结点删除  
        toBeDeleted.next = toBeDeleted.next.next;  
    }  
    // 返回删除节点后的链表头结点  
    return head;  
}  
```

#### 2. 翻转单链表

**题目描述**：输出一个单链表的逆序反转后的链表。
**解题思路**：用三个临时指针 prev、cur、next 在链表上循环一遍即可。

> [[剑指offer\] 从尾到头打印链表](https://weiweiblog.cn/printlistfromtailtohead/)
> [[剑指offer\] 反转链表](https://weiweiblog.cn/reverselist/)

#### 3. 翻转部分单链表：

**题目描述**：给定一个单向链表的头结点head,以及两个整数from和to,在单链表上把第from个节点和第to个节点这一部分进行反转

> 举例：1->2->3->4->5->null, from = 2, to = 4
> 结果：1->4->3->2->5->null

```java
public ListNode reverseBetween(ListNode head, int m, int n) {
    if (head == null) return null;
    if (head.next == null) return head;
    int i = 1;
    ListNode reversedNewHead = null;// 反转部分链表反转后的头结点
    ListNode reversedTail = null;// 反转部分链表反转后的尾结点
    ListNode oldHead = head;// 原链表的头结点
    ListNode reversePreNode = null;// 反转部分链表反转前其头结点的前一个结点
    ListNode reverseNextNode = null;
    while (head != null) {
        if (i > n) {
            break;
        }
        if (i == m - 1) {
            reversePreNode = head;
        }
        if (i >= m && i <= n) {
            if (i == m) {
                reversedTail = head;
            }
            reverseNextNode = head.next;
            head.next = reversedNewHead;
            reversedNewHead = head;
            head = reverseNextNode;
        } else {
            head = head.next;
        }
        i++;
    }
    reversedTail.next = reverseNextNode;
    if (reversePreNode != null) {
        reversePreNode.next = reversedNewHead;
        return oldHead;
    } else {
        return reversedNewHead;
    }
}
```

#### 4. 旋转单链表

**题目描述**：给定一个单链表，设计一个算法实现链表向右旋转 K 个位置。
举例： 给定 1->2->3->4->5->6->NULL, K=3
则4->5->6->1->2->3->NULL
**解题思路**：

- **方法一** 双指针，快指针先走k步，然后两个指针一起走，当快指针走到末尾时，慢指针的下一个位置是新的顺序的头结点，这样就可以旋转链表了。
- **方法二** 先遍历整个链表获得链表长度n，然后此时把链表头和尾链接起来，在往后走n – k % n个节点就到达新链表的头结点前一个点，这时断开链表即可。

方法二代码：

```java
public class Solution { {
    public ListNode rotateRight(ListNode head, int k) {
        if (!head) return null;
        int n = 1;
        ListNode cur = head;
        while (cur.next) {
            ++n;
            cur = cur.next;
        }
        cur.next = head;
        int m = n - k % n;
        for (int i = 0; i < m; ++i) {
            cur = cur.next;
        }
        ListNode newhead = cur.next;
        cur.next = NULL;
        return newhead;
    }
};
```

#### 5. 删除单链表倒数第 n 个节点

**题目描述**：删除单链表倒数第 n 个节点，1 <= n <= length，尽量在一次遍历中完成。
**解题思路**：双指针法，找到倒数第 n+1 个节点，将它的 next 指向倒数第 n-1个节点。

> [[剑指offer\] 链表中倒数第k个结点](https://weiweiblog.cn/findkthtotail/)

#### 6. 求单链表的中间节点

**题目描述**：求单链表的中间节点，如果链表的长度为偶数，返回中间两个节点的任意一个，若为奇数，则返回中间节点。
**解题思路**：快慢指针，慢的走一步，快的走两步，当快指针到达尾节点时，慢指针移动到中间节点。

```java
// 遍历一次，找出单链表的中间节点
public ListNode findMiddleNode(ListNode head) {
    if (null == head) {
        return;
    }
    ListNode slow = head;
    ListNode fast = head;

    while (null != fast && null != fast.next) {
        fast = fast.next.next;
        slow = slow.next;
    }
    return slow;
}
```

#### 7. 链表划分

**题目描述**： 给定一个单链表和数值x，划分链表使得所有小于x的节点排在大于等于x的节点之前。

```java
public class Solution {
    /**
     * @param head: The first node of linked list.
     * @param x: an integer
     * @return: a ListNode 
     */
    public ListNode partition(ListNode head, int x) {
        // write your code here
        if(head == null) return null;
        ListNode leftDummy = new ListNode(0);
        ListNode rightDummy = new ListNode(0);
        ListNode left = leftDummy, right = rightDummy;

        while (head != null) {
            if (head.val < x) {
                left.next = head;
                left = head;
            } else {
                right.next = head;
                right = head;
            }
            head = head.next;
        }

        right.next = null;
        left.next = rightDummy.next;
        return leftDummy.next;
    }
}
```

#### 8. 链表求和

**题目描述**：你有两个用链表代表的整数，其中每个节点包含一个数字。数字存储按照在原来整数中相反的顺序，使得第一个数字位于链表的开头。写出一个函数将两个整数相加，用链表形式返回和。
**解题思路**：做个大循环，对每一位进行操作：

> 当前位：(A[i]+B[i])%10
> 进位：（A[i]+B[i]）/10

```java
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode c1 = l1;
        ListNode c2 = l2;
        ListNode sentinel = new ListNode(0);
        ListNode d = sentinel;
        int sum = 0;
        while (c1 != null || c2 != null) {
            sum /= 10;
            if (c1 != null) {
                sum += c1.val;
                c1 = c1.next;
            }
            if (c2 != null) {
                sum += c2.val;
                c2 = c2.next;
            }
            d.next = new ListNode(sum % 10);
            d = d.next;
        }
        if (sum / 10 == 1)
            d.next = new ListNode(1);
        return sentinel.next;
    }
}
```

#### 9. 单链表排序

**题目描述**：在O(nlogn)时间内对链表进行排序。
**快速排序**：

```java
public ListNode sortList(ListNode head) {
    //采用快速排序
   quickSort(head, null);
   return head;
}
public static void quickSort(ListNode head, ListNode end) {
    if (head != end) {
        ListNode node = partion(head, end);
        quickSort(head, node);
        quickSort(node.next, end);
    }
}

public static ListNode partion(ListNode head, ListNode end) {
    ListNode p1 = head, p2 = head.next;

    //走到末尾才停
    while (p2 != end) {

        //大于key值时，p1向前走一步，交换p1与p2的值
        if (p2.val < head.val) {
            p1 = p1.next;

            int temp = p1.val;
            p1.val = p2.val;
            p2.val = temp;
        }
        p2 = p2.next;
    }

    //当有序时，不交换p1和key值
    if (p1 != head) {
        int temp = p1.val;
        p1.val = head.val;
        head.val = temp;
    }
    return p1;
}
```

**归并排序**：

```java
public ListNode sortList(ListNode head) {
    //采用归并排序
    if (head == null || head.next == null) {
        return head;
    }
    //获取中间结点
    ListNode mid = getMid(head);
    ListNode right = mid.next;
    mid.next = null;
    //合并
    return mergeSort(sortList(head), sortList(right));
}

/**
 * 获取链表的中间结点,偶数时取中间第一个
 *
 * @param head
 * @return
 */
private ListNode getMid(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    //快慢指针
    ListNode slow = head, quick = head;
    //快2步，慢一步
    while (quick.next != null && quick.next.next != null) {
        slow = slow.next;
        quick = quick.next.next;
    }
    return slow;
}

/**
 *
 * 归并两个有序的链表
 *
 * @param head1
 * @param head2
 * @return
 */
private ListNode mergeSort(ListNode head1, ListNode head2) {
    ListNode p1 = head1, p2 = head2, head;
   //得到头节点的指向
    if (head1.val < head2.val) {
        head = head1;
        p1 = p1.next;
    } else {
        head = head2;
        p2 = p2.next;
    }

    ListNode p = head;
    //比较链表中的值
    while (p1 != null && p2 != null) {

        if (p1.val <= p2.val) {
            p.next = p1;
            p1 = p1.next;
            p = p.next;
        } else {
            p.next = p2;
            p2 = p2.next;
            p = p.next;
        }
    }
    //第二条链表空了
    if (p1 != null) {
        p.next = p1;
    }
    //第一条链表空了
    if (p2 != null) {
        p.next = p2;
    }
    return head;
}
```

#### 10. 合并两个排序的链表

**题目描述**：输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

> [[剑指offer\] 合并两个排序的链表](https://weiweiblog.cn/mergelinklist/)

#### 11. 复杂链表的复制

**题目描述**：输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

> [[剑指offer\] 复杂链表的复制](https://weiweiblog.cn/clonelink/)

#### 12. 删除链表中重复的结点

**题目描述**：在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

> [[剑指offer\] 删除链表中重复的结点](https://weiweiblog.cn/deleteduplication/)

#### 13. 判断单链表是否存在环

**题目描述**：判断一个单链表是否有环
分析：快慢指针，慢指针每次移动一步，快指针每次移动两步，如果存在环，那么两个指针一定会在环内相遇。

#### 14. 单链表是否有环扩展：找到环的入口点

**题目描述**：判断单链表是否有环，如果有，找到环的入口点
**解题思路**：在第 5 题两个指针相遇后，让其中一个指针回到链表的头部，另一个指针在原地，同时往前每次走一步，当它们再次相遇时，就是在环路的入口点。

> [[剑指offer\] 链表中环的入口结点](https://weiweiblog.cn/entrynodeofloop/)

#### 15. 判断两个无环单链表是否相交

**题目描述**：给出两个无环单链表
**解题思路**：

- **方法一** 最直接的方法是判断 A 链表的每个节点是否在 B 链表中，但是这种方法的时间复杂度为 O(Length(A) * Length(B))。
- **方法二** 转化为环的问题。把 B 链表接在 A 链表后面，如果得到的链表有环，则说明两个链表相交。可以之前讨论过的快慢指针来判断是否有环，但是这里还有更简单的方法。如果 B 链表和 A 链表相交，把 B 链表接在 A 链表后面时，B 链表的所有节点都在环内，所以此时只需要遍历 B 链表，看是否会回到起点就可以判断是否相交。这个方法需要先遍历一次 A 链表，找到尾节点，然后还要遍历一次 B 链表，判断是否形成环，时间复杂度为 O(Length(A) + Length(B))。
- **方法三** 除了转化为环的问题，还可以利用“如果两个链表相交于某一节点，那么之后的节点都是共有的”这个特点，如果两个链表相交，那么最后一个节点一定是共有的。所以可以得出另外一种解法，先遍历 A 链表，记住尾节点，然后遍历 B 链表，比较两个链表的尾节点，如果相同则相交，不同则不相交。时间复杂度为 O(Length(A) + Length(B))，空间复杂度为 O(1)，思路比解法 2 更简单。

方法三的代码：

```java
public boolean isIntersect(ListNode headA, ListNode headB) {
    if (null == headA || null == headB) {
        return false;
    }
    if (headA == headB) {
        return true;
    }
    while (null != headA.next) {
        headA = headA.next;
    }
    while (null != headB.next) {
        headB = headB.next;
    }
    return headA == headB;
}
```

#### 16. 两个链表相交扩展：求两个无环单链表的第一个相交点

**题目描述**：找到两个无环单链表第一个相交点，如果不相交返回空，要求在线性时间复杂度和常量空间复杂度内完成。
**解题思路**：

- **方法一** 如果两个链表存在公共结点，那么它们从公共结点开始一直到链表的结尾都是一样的，因此我们只需要从链表的结尾开始，往前搜索，找到最后一个相同的结点即可。但是题目给出的单向链表，我们只能从前向后搜索，这时，我们就可以借助栈来完成。先把两个链表依次装到两个栈中，然后比较两个栈的栈顶结点是否相同，如果相同则出栈，如果不同，那最后相同的结点就是我们要的返回值。
- **方法二** 先找出2个链表的长度，然后让长的先走两个链表的长度差，然后再一起走，直到找到第一个公共结点。
- **方法三** 由于2个链表都没有环，我们可以把第二个链表接在第一个链表后面，这样就把问题转化为求环的入口节点问题。
- **方法四** 两个指针p1和p2分别指向链表A和链表B，它们同时向前走，当走到尾节点时，转向另一个链表，比如p1走到链表 A 的尾节点时，下一步就走到链表B，p2走到链表 B 的尾节点时，下一步就走到链表 A，当p1==p2 时，就是链表的相交点

方法四的代码：

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (null == headA || null == headB) {
        return null;
    }
    if (headA == headB) {
        return headA;
    }

    ListNode p1 = headA;
    ListNode p2 = headB;
    while (p1 != p2) {
        // 遍历完所在链表后从另外一个链表再开始
        // 当 p1 和 p2 都换到另一个链表时，它们对齐了：
        // （1）如果链表相交，p1 == p2 时为第一个相交点
        // （2）如果链表不相交，p1 和 p2 同时移动到末尾，p1 = p2 = null，然后退出循环
        p1 = (null == p1) ? headB : p1.next;
        p2 = (null == p2) ? headA : p2.next;
    }
    return p1;
}
```

> [[剑指offer\] 两个链表的第一个公共结点](https://weiweiblog.cn/findfirstcommonnode/)

#### 17. 两个链表相交扩展：判断两个有环单链表是否相交

**题目描述**：上面的问题是针对无环链表的，如果是链表有环呢？
**解题思路**：如果两个有环单链表相交，那么它们一定共有一个环，即环上的任意一个节点都存在于两个链表上。因此可以先用之前快慢指针的方式找到两个链表中位于环内的两个节点，如果相交的话，两个节点在一个环内，那么移动其中一个节点，在一次循环内肯定可以与另外一个节点相遇。

#### 18. Reference

- https://www.weiweiblog.cn/linkedlist_summary/
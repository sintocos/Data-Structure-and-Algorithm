## **Binary Tree Tips**

[TOC]

### 0. 几个概念

完全二叉树：若二叉树的高度是h，除第h层之外，其他（1~h-1）层的节点数都达到了最大个数，并且第h层的节点都连续的集中在最左边。想到点什么没？实际上，完全二叉树和堆联系比较紧密哈~~~

满二叉树：除最后一层外，每一层上的所有节点都有两个子节点，最后一层都是叶子节点。

哈夫曼树：给定n个权值作为n的叶子结点，构造一棵二叉树，若带权路径长度达到最小，称这样的二叉树为最优二叉树，也称为哈夫曼树(Huffman tree)。

二叉排序树：又称二叉查找树（Binary Search Tree），亦称二叉搜索树。二叉排序树或者是一棵空树，或者是具有下列性质的二叉树：

- 若左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- 若右子树不空，则右子树上所有结点的值均大于或等于它的根结点的值；
- 左、右子树也分别为二叉排序树；
- 没有键值相等的节点

二分查找的时间复杂度是O(log(n))，最坏情况下的时间复杂度是O(n)（相当于顺序查找）

平衡二叉树：又称 AVL 树。平衡二叉树是二叉搜索树的进化版，所谓平衡二叉树指的是，左右两个子树的高度差的绝对值不超过 1。

红黑树：红黑树是每个节点都带颜色的树，节点颜色或是红色或是黑色，红黑树是一种查找树。红黑树有一个重要的性质，从根节点到叶子节点的最长的路径不多于最短的路径的长度的两倍。对于红黑树，插入，删除，查找的复杂度都是O（log N）。

### 1. 求二叉树中的节点个数

递归解法：
（1）如果二叉树为空，节点个数为0
（2）如果不为空，二叉树节点个数 = 左子树节点个数 + 右子树节点个数 + 1
参考代码如下：

```java
public static int getNodeNumRec(TreeNode root) {
        if (root == null) {
            return 0;
        }             
        return getNodeNumRec(root.left) + getNodeNumRec(root.right) + 1;
}
```

### 2. 求二叉树的最大层数(最大深度)

> 剑指offer：[二叉树的深度](https://weiweiblog.cn/treedepth/)
> 递归解法：
> （1）如果二叉树为空，二叉树的深度为0
> （2）如果二叉树不为空，二叉树的深度 = max(左子树深度， 右子树深度) + 1
> 参考代码如下：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null)
            return 0;
        return Math.max(maxDepth(root.left), maxDepth(root.right))+1;
    }
}
```

#### 2.1 二叉树的最小深度

> LeetCode：[Minimum Depth of Binary Tree](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/description/)
> 给定一个二叉树，找出其最小深度。
> 最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

![img](https://ws3.sinaimg.cn/large/0069RVTdgy1fuwr1dxogtj31e80ek0u8.jpg)

```java
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null)
            return 0;
        int left = minDepth(root.left);
        int right = minDepth(root.right);
        return (left == 0 || right == 0) ? left + right + 1 : Math.min(left, right) + 1;
    }
}
```

### 3. 先序遍历/前序遍历

> LeetCode：[Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)
> 给定二叉树，返回其节点值的前序遍历。

![img](https://ws4.sinaimg.cn/large/0069RVTdgy1fuv7z3zzmpj31e20dmgmr.jpg)
`根 - 左 - 右`

#### 递归

```java
ArrayList<Integer> preOrderReverse(TreeNode root){
    ArrayList<Integer> result = new ArrayList<Integer>();
    preOrder(root, result);
    return result; 
}
void preOrder(TreeNode root,ArrayList<Integer> result){
    if(root == null){
        return;
    }
    result.add(root.val);
    preOrder(root.left, result);
    preOrder(root.right, result);
}
```

#### 非递归

法一：

```java
import java.util.Stack;
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        LinkedList<Integer> res = new LinkedList<>();
        if(root == null)
            return res;
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode node = stack.pop();
            res.add(node.val);
            if(node.right != null){
                stack.push(node.right);
            }
            if(node.left != null){
                stack.push(node.left);
            }
        }
        return res;
    }
}
```

法二：

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if(root == null)
            return res;
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode cur = root;
        while(cur != null || !stack.isEmpty()){
            if(cur!=null){
                res.add(cur.val);
                stack.push(cur);
                cur = cur.left;
            }else{
                cur = stack.pop();
                cur = cur.right;
            }
        }
        return res;
    }
}
```

### 4. 中序遍历

> LeetCode：[Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)
> 给定二叉树，返回其节点值的中序遍历。

![img](https://ws4.sinaimg.cn/large/0069RVTdgy1fuv7x8fbwkj31e60dyq43.jpg)
`左 - 根 - 右`

#### 递归

```java
void inOrder(TreeNode root,ArrayList<Integer> result){
    if(root == null){
        return;
    }
    preOrder(root.left, result);
    result.add(root.val);
    preOrder(root.right, result);
}
```

#### 非递归

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if(root == null)
            return res;

        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode cur = root;
        while(!stack.isEmpty() || cur != null){
            if(cur != null){
                stack.push(cur);
                cur = cur.left;
            }else{
                cur = stack.pop();
                res.add(cur.val);
                cur = cur.right;
            }
        }
        return res;
    }
}
```

### 5. 后序遍历

> Leetcode：[Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)
> 给定二叉树，返回其节点值的后序遍历。

![img](https://ws1.sinaimg.cn/large/0069RVTdgy1fuv7s7s13wj31e20dogmr.jpg)
`左 - 右 - 根`

#### 递归

```java
void inOrder(TreeNode root,ArrayList<Integer> result){
    if(root == null){
        return;
    }
    preOrder(root.left, result);
    preOrder(root.right, result);
    result.add(root.val);
}
```

#### 非递归

方法一：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        LinkedList<Integer> ans = new LinkedList<>();
        Stack<TreeNode> stack = new Stack<>();
        if (root == null) return ans;

        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            //采用逆序插入的方式
            ans.addFirst(cur.val);
            if (cur.left != null) {
                stack.push(cur.left);
            }
            if (cur.right != null) {
                stack.push(cur.right);
            } 
        }
        return ans;
    }
}
```

方法二：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();

        if(root == null)
            return res;

        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode cur = root;
        TreeNode visited = null;
        while(!stack.isEmpty() || cur != null){
            if(cur != null){
                stack.push(cur);
                cur = cur.left;
            }else{
                cur = stack.peek();
                if(cur.right != null && cur.right != visited){
                    cur = cur.right;
                }else{
                    res.add(cur.val);
                    visited = cur;
                    stack.pop();
                    cur = null;
                }
            }
        }
        return res;
    }
}
```

方法三(推荐)：

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        LinkedList<Integer> result = new LinkedList<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode p = root;
        while(!stack.isEmpty() || p != null) {
            if(p != null) {
                stack.push(p);
                result.addFirst(p.val);  // Reverse the process of preorder
                p = p.right;             // Reverse the process of preorder
            } else {
                TreeNode node = stack.pop();
                p = node.left;           // Reverse the process of preorder
            }
        }
        return result;
    }
}
```

### 6. 分层遍历

> LeetCode：[Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)
> 剑指offer：[从上往下打印二叉树](https://weiweiblog.cn/printfromtoptobottom/)
> 剑指offer：[把二叉树打印成多行](https://weiweiblog.cn/print/)
> 给定二叉树，返回其节点值的级别顺序遍历。

![img](https://ws2.sinaimg.cn/large/0069RVTdgy1fuv7qgpmmhj31ee0jiq4s.jpg)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(root == null)
            return res;

        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        TreeNode cur = null;
        queue.add(root);

        while(!queue.isEmpty()){
            ArrayList<Integer> level = new ArrayList<Integer>();
            int l = queue.size();
            for(int i=0; i<l;i++){
                cur = queue.poll();
                level.add(cur.val);
                if(cur.left != null)
                    queue.add(cur.left);
                if(cur.right != null)
                    queue.add(cur.right);
            }
            res.add(level);
        }
        return res;
    }
}
```

#### 6.1 自下而上分层遍历

> LeetCode：[Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/description/)
> 给定二叉树，返回其节点值的自下而上级别顺序遍历。

```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> res = new LinkedList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        if(root == null)
            return res;
        queue.add(root);
        while(!queue.isEmpty()){
            int count = queue.size();
            List<Integer> temp = new LinkedList<>();
            for(int i=0; i<count; i++){
                TreeNode node = queue.poll();
                temp.add(node.val);
                if(node.left != null)
                    queue.add(node.left);
                if(node.right != null)
                    queue.add(node.right);
            }
            // 每次都添加到第一个位置
            res.add(0, temp);
        }
        return res;
    }
}
```

#### 6.2 按之字形顺序打印二叉树

> 剑指offer：[按之字形顺序打印二叉树](https://weiweiblog.cn/printz/)
> 请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

设两个栈，s2存放奇数层，s1存放偶数层
遍历s2节点的同时按照左子树、右子树的顺序加入s1，
遍历s1节点的同时按照右子树、左子树的顺序加入s2

```java
import java.util.ArrayList;
import java.util.Stack;
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;
    public TreeNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer> > res = new ArrayList<ArrayList<Integer> >();
        Stack<TreeNode> s1 = new Stack<TreeNode>();
        Stack<TreeNode> s2 = new Stack<TreeNode>();
        int flag = 1;
        if(pRoot == null)
            return res;
        s2.push(pRoot);
        ArrayList<Integer> temp = new ArrayList<Integer>();
        while(!s1.isEmpty() || !s2.isEmpty()){
            if(flag % 2 != 0){
                while(!s2.isEmpty()){
                    TreeNode node = s2.pop();
                    temp.add(node.val);
                    if(node.left != null){
                        s1.push(node.left);
                    }
                    if(node.right != null){
                        s1.push(node.right);
                    }
                }
            }
            if(flag % 2 == 0){
                while(!s1.isEmpty()){
                    TreeNode node = s1.pop();
                    temp.add(node.val);
                    if(node.right != null){
                        s2.push(node.right);
                    }
                    if(node.left != null){
                        s2.push(node.left);
                    }
                }
            }
            res.add(new ArrayList<Integer>(temp));
            temp.clear();
            flag ++;
        }
        return res;
    }

}
```

### 7. 求二叉树第K层的节点个数

```java
void get_k_level_number(TreeNode root, int k){
    if(root == null || k <=0){
        return 0;
    }
    if(root != null && k == 1){
        return 1;
    }
    return get_k_level_number(root.left, k-1) + get_k_level_number(root.right, k-1);
}
```

### 8. 求二叉树第K层的叶子节点个数

```java
void get_k_level_leaf_number(TreeNode root, int k){
    if(root == null || k <=0){
        return 0;
    }
    if(root != null && k == 1){
        if(root.left == null && root.right == null)
            return 1;
        else
            return 0;
    }
    return get_k_level_number(root.left, k-1) + get_k_level_number(root.right, k-1);
}
```

### 9. 判断两棵二叉树是否结构相同

> LeetCode：[Same Tree](https://leetcode.com/problems/same-tree/description/)
> 给定两个二叉树，编写一个函数来检查它们是否相同。

递归解法：
（1）如果两棵二叉树都为空，返回真
（2）如果两棵二叉树一棵为空，另一棵不为空，返回假
（3）如果两棵二叉树都不为空，如果对应的左子树和右子树都同构返回真，其他返回假

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p == null && q == null)
            return true;
        if(p == null || q == null)
            return false;
        if(p.val == q.val)
            return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
        return false;
    }   
}
```

### 10. 判断二叉树是不是平衡二叉树

> LeetCode：[Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/description/)
> 给定二叉树，确定它是否是高度平衡的。
> 对于此问题，高度平衡二叉树定义为： 一个二叉树，其中每个节点的两个子树的深度差不相差超过1。

递归解法：
（1）如果二叉树为空，返回真
（2）如果二叉树不为空，如果左子树和右子树高度相差不大于1且左子树和右子树都是AVL树，返回真，其他返回假

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null)
            return true;
        return Math.abs(maxHigh(root.left) - maxHigh(root.right)) <= 1 
            && isBalanced(root.left) && isBalanced(root.right);
    }

    public int maxHigh(TreeNode root){
        if(root == null)
            return 0;
        return Math.max(maxHigh(root.left), maxHigh(root.right))+1;
    }
}
```

### 11. 求二叉树的镜像

> 剑指offer：[二叉树的镜像](https://www.weiweiblog.cn/mirrortree/)
> LeetCode：[Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)
> 反转二叉树

![img](https://ws3.sinaimg.cn/large/0069RVTdgy1fuv7ea8p94j31ei0jq0u4.jpg)

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null)
            return root;

        TreeNode node = root.left;
        root.left = invertTree(root.right);
        root.right = invertTree(node);

        return root;
    }
}
```

#### 11.1 对称二叉树

> 剑指offer：[[剑指offer\] 对称的二叉树](https://weiweiblog.cn/issymmetrical/)
> LeetCode：[Symmetric Tree](https://leetcode.com/problems/symmetric-tree/description/)
> 给定一个二叉树，检查它是否是镜像对称的。
> ![img](https://ws3.sinaimg.cn/large/0069RVTdgy1fuws1gopizj31eo0jwq4x.jpg)

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return root == null || isSymmetricHelper(root.left, root.right);
    }
    public boolean isSymmetricHelper(TreeNode left, TreeNode right){
        if(left == null && right == null)
            return true;
        if(left == null || right == null)
            return false;
        if(left.val != right.val)
            return false;
        return isSymmetricHelper(left.left, right.right) && isSymmetricHelper(left.right, right.left);
    }
}
```

### 12. 求二叉树中两个节点的最低公共祖先节点

> LeetCode：[Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)
> 给定二叉树，找到树中两个给定节点的最低共同祖先（LCA）。

![img](https://ws4.sinaimg.cn/large/0069RVTdgy1fuv818buisj31du0rgjvi.jpg)
递归解法：
（1）如果两个节点分别在根节点的左子树和右子树，则返回根节点
（2）如果两个节点都在左子树，则递归处理左子树；如果两个节点都在右子树，则递归处理右子树

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q)
            return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left != null && right != null)
            return root;
        return left == null ? right : left;
    }
}
```

#### 12.1 求二叉搜索树的最近公共祖先

> LeetCode：[Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)
> 给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
> 百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

![img](https://ws1.sinaimg.cn/large/0069RVTdly1fuw3xgeyd0j31dw0r4dk0.jpg)
注意二叉搜索树的特性：`左子树` < `根节点` < `右子树`

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root.val > p.val && root.val > q.val){
            return lowestCommonAncestor(root.left, p, q);
        }
        else if(root.val < p.val && root.val < q.val){
            return lowestCommonAncestor(root.right, p, q);
        }
        else{
            return root;
        }
    }
}
```

### 13. 求二叉树的直径

> LeetCode：[Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/description/)
> 给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。

![img](https://ws3.sinaimg.cn/large/0069RVTdly1fuw3a12zjaj31ek0e20ua.jpg)
递归解法：对于每个节点，它的最长路径等于左子树的最长路径+右子树的最长路径

```java
class Solution {
    private int path = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        diamHelper(root);
        return path;
    }

    private int diamHelper(TreeNode root){
        if(root == null)
            return 0;
        int left = diamHelper(root.left);
        int right = diamHelper(root.right);
        path = Math.max(path, left + right);
        return Math.max(left, right) + 1;
    }
}
```

### 14. 由前序遍历序列和中序遍历序列重建二叉树

> 剑指offer：[重建二叉树](https://www.weiweiblog.cn/reconstructbinarytree/)
> LeetCode：[Construct Binary Tree from Preorder and Inorder Traversal](https://www.weiweiblog.cn/20tree/)
> 根据一棵树的前序遍历与中序遍历构造二叉树。

![img](https://ws1.sinaimg.cn/large/0069RVTdly1fuw48smh0cj31dw0g6tai.jpg)

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder.length == 0 || inorder.length == 0)
            return null;
        return buildTreeHelper(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    public TreeNode buildTreeHelper(int[] preorder, int pre_start, int pre_end, 
                                    int[] inorder, int in_start, int in_end){
        if(pre_start > pre_end || in_start > in_end)
            return null;
        TreeNode root = new TreeNode(preorder[pre_start]);
        for(int i = in_start; i <= in_end; i++){
            if(inorder[i] == preorder[pre_start]){
                // 左子树的长度：i-is
                root.left = buildTreeHelper(preorder, pre_start + 1, pre_start + i - in_start, inorder, in_start, i - 1);
                root.right = buildTreeHelper(preorder, pre_start + i - in_start + 1, pre_end, inorder, i + 1, in_end);
            }
        }
        return root;
    }
}
```

#### 14.1 从中序与后序遍历序列构造二叉树

> LeetCode：[Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)
> 根据一棵树的中序遍历与后序遍历构造二叉树。

![img](https://ws1.sinaimg.cn/large/0069RVTdly1fuw5432j6cj31dy0goabw.jpg)
本题与“从前序与中序遍历序列构造二叉树”是一个套路。唯一的区别是，后序序列的最后一个节点是根节点，因此我们要从后序序列的最后一个节点入手，再去中序序列中找到这个节点。在这个节点左侧的属于根节点的左子树部分，右侧的属于根节点右子树部分。然后根据左右子树节点的数量，在后序序列中找到他们各自的后序序列。比左子树节点个数为5，那么在后序序列中前五个节点就是左子树节点，之后的节点除了最后一个节点都是右子树节点。

```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(inorder.length == 0 || postorder.length == 0)
            return null;
        return buildTreeHelper(inorder, 0, inorder.length - 1, postorder, 0, postorder.length - 1);
    }

    public TreeNode buildTreeHelper(int[] inorder, int in_start, int in_end, 
                               int[] postorder, int post_start, int post_end){
        if(in_start > in_end || post_start > post_end)
            return null;
        TreeNode root = new TreeNode(postorder[post_end]);
        for(int i = in_start; i <= in_end; i++){
            if(inorder[i] == postorder[post_end]){
                root.left = buildTreeHelper(inorder, in_start, i - 1, postorder, post_start, post_start + i - in_start - 1);
                root.right = buildTreeHelper(inorder, i + 1, in_end, postorder, post_start + i - in_start, post_end - 1);
            }
        }
        return root;
    }
}
```

> 提示：根据前序和后序遍历无法构造出唯一的二叉树

### 15. 判断二叉树是不是完全二叉树

完全二叉树是指最后一层左边是满的，右边可能慢也不能不满，然后其余层都是满的，根据这个特性，利用层遍历。如果我们当前遍历到了NULL结点，如果后续还有非NULL结点，说明是非完全二叉树。

```java
class Solution {
    public boolean _CheckCompleteTree(TreeNode root) {
        Queue<TreeNode> queue = LinkedList<>();
        if(root == null)
            return true;
        queue.add(root);
        while(!queue.isEmpty()){
            TreeNode node = queue.pop();
            if(node != null){
                if(flag == true)
                    return false;
                queue.add(node.left);
                queue.add(node.right);
            }else{
                flag = true;
            }
        }
        return true;    
    }
}
```

### 16. 树的子结构

> 剑指offer：[树的子结构](https://weiweiblog.cn/issubtree/)
> 输入两棵二叉树A，B，判断B是不是A的子结构。

```java
public class Solution {
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        if(root1 == null || root2 == null){
            return false;
        }
        return IsSubtree(root1, root2) || 
               HasSubtree(root1.left, root2) ||
               HasSubtree(root1.right, root2);
    }
    public boolean IsSubtree(TreeNode root1, TreeNode root2){
        //要先判断roo2, 不然{8,8,7,9,2,#,#,#,#,4,7},{8,9,2}这个测试用例通不过。
        if(root2 == null)
            return true;
        if(root1 == null)
            return false;
        if(root1.val == root2.val){
            return IsSubtree(root1.left, root2.left) && 
                IsSubtree(root1.right, root2.right);
        }else
            return false;
    }
}
```

### 17. 二叉树中和为某一值的路径

> 剑指offer：[二叉树中和为某一值的路径](https://www.weiweiblog.cn/findpath/)
> 输入一颗二叉树和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。

```java
public class Solution {
    ArrayList<ArrayList<Integer> > res = new ArrayList<ArrayList<Integer> >();
    ArrayList<Integer> temp = new ArrayList<Integer>();
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root, int target) {
        if(root == null)
            return res;
        target -= root.val;
        temp.add(root.val);
        if(target == 0 && root.left == null && root.right == null)
            res.add(new ArrayList<Integer>(temp));
        else{
            FindPath(root.left, target);
            FindPath(root.right, target);
        }
        temp.remove(temp.size()-1);
        return res;
    }        
}
```

### 18. 二叉树的下一个结点

> 剑指offer：[二叉树的下一个结点](https://weiweiblog.cn/getnext/)
> 给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

```java
public class Solution {
    public TreeLinkNode GetNext(TreeLinkNode pNode)
    {
        if(pNode == null){
            return null;
        }
        if(pNode.right != null){
            TreeLinkNode node = pNode.right;
            while(node.left != null){
                node = node.left;
            }
            return node;
        }
        while(pNode.next != null){
            TreeLinkNode root = pNode.next;
            if(pNode == root.left)
                return root;
            pNode = root;
        }
        return null;
    }
}
```

### 19. 序列化二叉树

> 剑指offer：[序列化二叉树](https://weiweiblog.cn/serialize/)
> LeetCode：[Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/description/)
> 请实现两个函数，分别用来序列化和反序列化二叉树

![img](https://ws1.sinaimg.cn/large/0069RVTdgy1fuwsmry0fsj31e20f4mym.jpg)

```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if(root == null)
            return "#,";
        StringBuffer res = new StringBuffer(root.val + ",");
        res.append(serialize(root.left));
        res.append(serialize(root.right));
        return res.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String [] d = data.split(",");
        Queue<String> queue = new LinkedList<>();
        for(int i = 0; i < d.length; i++){
            queue.offer(d[i]);
        }
        return pre(queue);
    }

    public TreeNode pre(Queue<String> queue){
        String val = queue.poll();
        if(val.equals("#"))
            return null;
        TreeNode node = new TreeNode(Integer.parseInt(val));
        node.left = pre(queue);
        node.right = pre(queue);
        return node;
    }
}
```

### 20. 二叉搜索树的第k个结点

> 剑指offer：[二叉搜索树的第k个结点](https://weiweiblog.cn/kthnode/)
> 给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）中，按结点数值大小顺序第三小结点的值为4。

![img](https://ws3.sinaimg.cn/large/0069RVTdgy1fuwt2r4qi6j31ec0q0jtp.jpg)
因为二叉搜索树按照中序遍历的顺序打印出来就是排好序的，所以，我们按照中序遍历找到第k个结点就是题目所求的结点。

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        if(root == null)
             return Integer.MIN_VALUE;
        Stack<TreeNode> stack = new Stack<>();
        int count = 0;
        TreeNode p = root;
        while(p != null || !stack.isEmpty()){
            if(p != null){
                stack.push(p);
                p = p.left;
            }else{
                TreeNode node = stack.pop();
                count ++;
                if(count == k)
                    return node.val;
                p = node.right;
            }
        }
        return Integer.MIN_VALUE;
    }
}
```

### 21. Reference

- https://www.weiweiblog.cn/20tree/
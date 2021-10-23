# 1650_LowestCommonAncestorofaBinaryTreeIII

Given two nodes of a binary tree `p` and `q`, return *their lowest common ancestor (LCA)*.

Each node will have a reference to its parent node. The definition for `Node` is below:

```
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
}
```

According to the **[definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor)**: "The lowest common ancestor of two nodes p and q in a tree T is the lowest node that has both p and q as descendants (where we allow **a node to be a descendant of itself**)."

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5 since a node can be a descendant of itself according to the LCA definition.
```

**Example 3:**

```
Input: root = [1,2], p = 1, q = 2
Output: 1
```

## 题目分析

此题要找**最近公共祖先**，其实根本用不上`Node`的`left`和`right`指针，所以这棵树本质就是一个**链表**，然后找到链表交点。

使用**两个节点进行遍历**，如果当前节点**不为空**，便将其**转到其父节点**，否则直接跳到**另一个节点的起始点**。此举是为了能够找到最近的公共节点，可参考Example2进行模拟。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
};
*/

class Solution {
    public Node lowestCommonAncestor(Node p, Node q) {
        Node node1 = p, node2  = q;
        while (node1 != node2) {
            node1 = node1 != null ? node1.parent : q;
            node2 = node2 != null ? node2.parent : p;
        }
        return node1;
    }
}
```

此题可参考[LeetCode160](https://leetcode.com/problems/intersection-of-two-linked-lists/)，十分类似。啊不，完全一样哈哈哈，不信你试试？！


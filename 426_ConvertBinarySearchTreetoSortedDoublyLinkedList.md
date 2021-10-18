# 426_ConvertBinarySearchTreetoSortedDoublyLinkedList

Convert a **Binary Search Tree** to a sorted **Circular Doubly-Linked List** in place.

You can think of the left and right pointers as synonymous to the predecessor and successor pointers in a doubly-linked list. For a circular doubly linked list, the predecessor of the first element is the last element, and the successor of the last element is the first element.

We want to do the transformation **in place**. After the transformation, the left pointer of the tree node should point to its predecessor, and the right pointer should point to its successor. You should return the pointer to the smallest element of the linked list.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png)

```
Input: root = [4,2,5,1,3]


Output: [1,2,3,4,5]

Explanation: The figure below shows the transformed BST. The solid line indicates the successor relationship, while the dashed line means the predecessor relationship.
```

**Example 2:**

```
Input: root = [2,1,3]
Output: [1,2,3]
```

**Example 3:**

```
Input: root = []
Output: []
Explanation: Input is an empty tree. Output is also an empty Linked List.
```

**Example 4:**

```
Input: root = [1]
Output: [1]
```

## 题目分析

该题采用**DFS**，**深度优先搜索**，**递归**的方式。本题采用**中序遍历**，因为中序遍历是`左->根->右`，而题目中所给的是**BST，二叉搜索树**，满足：左子树小于根节点，右子树大于根节点，中序遍历之后刚好是从最小到最大。

算法如下：

1. 将`first`和`last`节点初始化为`null` (全局变量)
2. 调用递归函数(中序遍历)：
   - 节点为空，返回
   - 先调用当前节点的左子树
   - 若`last`不为空，则将`last`与当前节点连接；空则初始化`first`节点，使之为`node`
   - 将`last`移到当前节点
   - 最后调用当前节点右子树
3. 最终将`first`与`last`相连，使链表成环。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/

class Solution {
    private Node first = null;
    private Node last = null;
    public Node treeToDoublyList(Node root) {
        if (root == null) return null;
        inOrder(root);
        first.left = last;
        last.right = first;
        return first;
    }
    private void inOrder(Node node) {
        if (node == null) return;
            inOrder(node.left);
            if (last != null) {
                last.right = node;
                node.left = last;
            } else {
                first = node;
            }
            last = node;
            inOrder(node.right);
    }
}
```

时间复杂度：O(N)

空间复杂度：O(N)

借此机会，回顾一下DFS与BFS：

比如对于本题，采用四种遍历方式，前中/后/序遍历+层序遍历：

![postorder](https://leetcode.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list/Figures/426/dfs_bfs.png)
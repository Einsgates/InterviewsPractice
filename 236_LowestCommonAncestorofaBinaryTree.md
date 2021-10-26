# 236_LowestCommonAncestorofaBinaryTree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

 

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
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

**Example 3:**

```
Input: root = [1,2], p = 1, q = 2
Output: 1
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[2, 105]`.
- `-109 <= Node.val <= 109`
- All `Node.val` are **unique**.
- `p != q`
- `p` and `q` will exist in the tree.

## 题目分析

公共祖先定义：节点 `root` 为节点`p`，`q`的某公共祖先，若`root.left` `root.right`都不是`p` `q` 的公共祖先，则 `root` 是“最近公共祖先”

![Picture1.png](https://pic.leetcode-cn.com/1599885247-rxcHcZ-Picture1.png)

若`root`为`p` `q`的最近公共祖先，则为以下情况：

1. `p` `q`在`root`子树且分居异侧
2. `p` = `root`，`q`在子树中
3. `q`= `root`，`p`在子树中

### 终止条件：

1. 当越过叶节点，返回`null`
2. `root`为`p`，`q`，返回`root`

### 递推：

1. 递归左子树
2. 递归右子树

### 返回值：

1. `left`，`right`均为空，返回`null`
2. 某个为空，返回另一个

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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || p == root || q == root) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left == null) return right;
        if (right == null) return left;
        return root;
    }
}
```

时间复杂度：O(N)

空间复杂度：O(N)
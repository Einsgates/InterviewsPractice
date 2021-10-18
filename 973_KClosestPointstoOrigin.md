# 973_KClosestPointstoOrigin

Given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y** plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the **X-Y** plane is the Euclidean distance:
$$
\sqrt{(x_1-x_2)^2 +(y_1-y_2)^2}
$$


You may return the answer in **any order**. The answer is **guaranteed** to be **unique** (except for the order that it is in).

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/03/closestplane1.jpg)

```
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].
```

**Example 2:**

```
Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]
Explanation: The answer [[-2,4],[3,3]] would also be accepted.
```

## 题目分析

#### 方法一：使用 `Arrays.sort()` 方法

```java
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        Arrays.sort(points, (p1, p2)->sum(p1)-sum(p2));
        return Arrays.copyOfRange(points, 0, k);
    }
    private int sum(int[] p) {
        return p[0]*p[0] + p[1]*p[1];
    }
}
```

时间复杂度：O(NlogN)

空间复杂度：O(logN)

#### 方法二：借助快排pivot

参考快速排序的思路，称之为**quick select**。

回顾快排：选取一个 `pivot`作为中枢，将之与其他元素进行比较。一轮迭代之后，使得 `pivot` 左边的元素都小于它，右边的元素都大于它。

依据此，每次迭代选取一个 `pivot` 并找到 `pivot` 应处的位置p。然后将p与K比较。如果p比K小，那么，意味着`pivot`左边的所有元素都是满足要求的尽管并不是完全升序；同理p>K...如果p==K，那么就找到了`K-th` 位置。因此，只需返回前K个元素，它们都比`pivot`小

```java
public int[][] kClosest(int[][] points, int K) {
    int len =  points.length, l = 0, r = len - 1;
    while (l <= r) {
        int mid = helper(points, l, r);
        if (mid == K) break;
        if (mid < K) {
            l = mid + 1;
        } else {
            r = mid - 1;
        }
    }
    return Arrays.copyOfRange(points, 0, K);
}

private int helper(int[][] A, int l, int r) {
    int[] pivot = A[l];
    while (l < r) {
        while (l < r && compare(A[r], pivot) >= 0) r--;
        A[l] = A[r];
        while (l < r && compare(A[l], pivot) <= 0) l++;
        A[r] = A[l];
    }
    A[l] = pivot;
    return l;
}

private int compare(int[] p1, int[] p2) {
    return p1[0] * p1[0] + p1[1] * p1[1] - p2[0] * p2[0] - p2[1] * p2[1];
}
```

注意：该算法平均时间复杂度：O(N)，但最坏情况O(N^2)，但整体来说还是最佳的算法。

时间复杂度：O(N)

空间复杂度：O(1)
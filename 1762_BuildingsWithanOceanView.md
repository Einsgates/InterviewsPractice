# 1762_BuildingsWithanOceanView

There are `n` buildings in a line. You are given an integer array `heights` of size `n` that represents the heights of the buildings in the line.

The ocean is to the right of the buildings. A building has an ocean view if the building can see the ocean without obstructions. Formally, a building has an ocean view if all the buildings to its right have a **smaller** height.

Return a list of indices (**0-indexed**) of buildings that have an ocean view, sorted in increasing order.

**Example 1:**

```
Input: heights = [4,2,3,1]
Output: [0,2,3]
```

**Example 2:**

```
Input: heights = [4,3,2,1]
Output: [0,1,2,3]
```

**Example 3:**

```
Input: heights = [1,3,2,4]
Output: [3]
```

**Example 4:**

```
Input: heights = [2,2,2,2]
Output: [3]
```

**Constraints:**

- `1 <= heights.length <= 10^5`
- `1 <= heights[i] <= 10^9`

## 题目分析

该题很容易想到使用两重`for`循环，但是时间复杂度为O(N^2)，而测试样例最大10^5，不可取。

因为**右边高楼会挡住左边低楼**，所以只要该楼左边的比它小，就**删除其左边的元素**。

采用一个单调栈，首先进行一个`for`循环：其中嵌套一个`while`循环，如果当前遍历元素比栈顶元素大，则将栈顶元素**出栈**，直到栈为空或者栈顶元素小于当前元素 (注意：这里栈中存储的都是元素的下标，也就是之后要返回的数组中的元素)。

```java
class Solution {
    public int[] findBuildings(int[] heights) {
        Stack<Integer> s = new Stack<>();
        int n = heights.length;
        for (int i = 0; i < n; i++) {
            while (!s.isEmpty() && heights[i] >= heights[s.peek()]) {
                s.pop();
            }
            s.push(i);
        }
        List<Integer> list = new ArrayList<>();
        while (!s.isEmpty()) {
            list.add(s.pop());
        }
        int size = list.size();
        int[] res = new int[size];
        for (int i = 0; i < size; i++) {
            res[i] = list.get(size - 1 - i);
        }
        return res;
    }
}
```

时间复杂度：O(N)

空间复杂度：O(N)
# 560_SubarraySumEqualsK

Given an array of integers `nums` and an integer `k`, return *the total number of continuous subarrays whose sum equals to `k`*.

 

**Example 1:**

```
Input: nums = [1,1,1], k = 2
Output: 2
```

**Example 2:**

```
Input: nums = [1,2,3], k = 3
Output: 2
```



## 题目分析

### 方法1：

建立一个数组存储**累计和**，要计算子数组大小(和)，只需用`sum[i]-sum[j]` 即可。累计和第一项为0，满足关系：`sum[i] = sum[i-1] + nums[i-1]`

该算法相较于Brute Force，快很多

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int[] sum = new int[nums.length+1];
        int cnt = 0;
        sum[0] = 0;
        for (int i = 1; i <= nums.length; i++) {
            sum[i] = sum[i - 1] + nums[i - 1];
        }
        for (int i = 0; i < sum.length; i++) {
            for (int j = i + 1; j < sum.length; j++) {
                if (sum[j] - sum[i] == k) cnt++;
            }
        }
        return cnt;
    }
}
```

时间复杂度：O(N^2)

空间复杂度：O(N)

### 方法2：

使用一个变量代替整个数组，仅使用常数级额外空间。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int cnt = 0;
        for (int i = 0; i < nums.length; i++) {
            int tmp = 0;
            for (int j = i; j < nums.length; j++) {
                tmp += nums[j];
                if (tmp == k) cnt++;
            }
        }
        return cnt;
    }
}
```

时间复杂度：O(N^2)

空间复杂度：O(1)

### 方法3：

使用**哈希表优化+前缀和**。上述方法局限性在于对于每个 `i`，需要用所有的`j`来判断是否符合条件，故应在此优化。

`preSum`为前`i`个的和，而要使`[j,,,i]`这个子数组的和为`k`，即满足以下条件：
$$
pre[i]-pre[j-1]=k \quad即\quad pre[j-1]==pre[i]-k 
$$
所以建立哈希表，以`preSum`为健，`pre[i]`出现的次数为值，从左到右更新计算，即可在常数时间找到。动画过程详见 [动画演示](https://leetcode-cn.com/problems/subarray-sum-equals-k/solution/he-wei-kde-zi-shu-zu-by-leetcode-solution/)

以下是代码实现:

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int preSum = 0, cnt = 0;
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        for (int i = 0; i < nums.length; i++) {
            preSum += nums[i];
            if (map.containsKey(preSum - k)) {
                cnt += map.get(preSum - k);
            }
            map.put(preSum, 1 + map.getOrDefault(preSum, 0));
        }
        return cnt;
    }
}
```

时间复杂度：O(N)

空间复杂度：O(N)
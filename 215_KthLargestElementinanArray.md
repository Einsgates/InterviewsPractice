# 215_KthLargestElementinanArray

Given an integer array `nums` and an integer `k`, return *the* `kth` *largest element in the array*.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

 

**Example 1:**

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

**Example 2:**

```
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
```

## 题目分析

### 方法1：

采用 Java 内置函数`Arrays.sort()`进行排序，然后选取第`k`大的。

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length - k];
    }
}
```

时间复杂度：O(NlogN)

空间复杂度：O(logN) 递归调用栈的高度

### 方法2：

参考快速排序中的`partition`部分函数，选取一个`pivot`并将其放到该放的位置上，使其左边元素均小于`pivot`，右边均大于其。

**算法描述**：

- 在数组中随机选取一个当作`pivot`，这里取`arr[high]`
- `i`初始化为`low-1`
- `for`循环，`j`从`low`到`high-1`
- 如果`arr[j] < pivot`，那么交换`arr[i]`和`arr[j]`
- 最终交换`pivot`的下标与`i+1`

具体实现过程见 [此视频](https://www.youtube.com/watch?v=PgBzjlCcFvc&feature=emb_imp_woyt) 十分详细。

```
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        int left = 0, right = len - 1;
        int target_idx = len - k;
        while (left < right) {
            int part = partition(nums, left, right);
            if (part < target_idx) {
                left = part + 1;
            } else if (part > target_idx) {
                right = part - 1;
            } else {
                break;
            }
        }
        return nums[target_idx];
    }
    private int partition(int[] arr, int low, int high) {
        int pivot = arr[high];
        int i = low - 1;
        for (int j = low; j < high; j++) {
            if (arr[j] < pivot) {
                i++;
                swap(arr, i, j);
            }
        }
        swap(arr, high, i + 1);
        return i + 1;
    }
    private void swap(int[] arr, int i, int j) {
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
}
```

但是该算法存在一个问题，如果该数组是一个**有序数组**，那么最坏时间复杂度会变成O(N^2)，所以改进版需要引入**随机化**。

时间复杂度：O(N) 

最坏时间复杂度：O(N^2)

空间复杂度：O(1)

### 改进：

对选取的pivot进行随机化，只需引入Random类，调用random.nextInt()方法即可。从low到high中随机选取一个元素的下标与high(准备当作pivot下标的序号)交换。

```java
import java.util.*;
class Solution {
    Random rand = new Random();
    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        int left = 0, right = len - 1;
        int target_idx = len - k;
        while (true) {
            int pi = partition(nums, left, right);
            if (pi < target_idx) {
                left = pi + 1;
            } else if (pi > target_idx) {
                right = pi - 1;
            } else {
                return nums[target_idx];
            }
        }
    }
    private int partition(int[] arr, int low, int high) {
        if (low < high) {
            int rand_idx = rand.nextInt(high - low) + low + 1;
            swap(arr, rand_idx, high);
        }
        int pivot = arr[high];
        int i = low - 1;
        for (int j = low; j < high; j++) {
            if (arr[j] < pivot) {
                i++;
                swap(arr, i, j);
            }
        }
        swap(arr, high, i + 1);
        return i + 1;
    }
    private void swap(int[] arr, int i, int j) {
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
}

```

时间复杂度：O(N)

空间复杂度：O(1) 常数空间

类似题目：[973](https://github.com/Einsgates/InterviewsPractice/blob/master/973_KClosestPointstoOrigin.md)


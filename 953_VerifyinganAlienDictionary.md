# 953_VerifyinganAlienDictionary

In an alien language, surprisingly, they also use English lowercase letters, but possibly in a different `order`. The `order` of the alphabet is some permutation of lowercase letters.

Given a sequence of `words` written in the alien language, and the `order` of the alphabet, return `true` if and only if the given `words` are sorted lexicographically in this alien language.

 

**Example 1:**

```
Input: words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
Output: true
Explanation: As 'h' comes before 'l' in this language, then the sequence is sorted.
```

**Example 2:**

```
Input: words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
Output: false
Explanation: As 'd' comes after 'l' in this language, then words[0] > words[1], hence the sequence is unsorted.
```

**Example 3:**

```
Input: words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
Output: false
Explanation: The first three characters "app" match, and the second string is shorter (in size.) According to lexicographical rules "apple" > "app", because 'l' > '∅', where '∅' is defined as the blank character which is less than any other character (More info).
```

## 题目分析

此题看似复杂，其实很简单，就是根据**给定的字母表顺序**，对字典进行判断，判断是否单词符合升序。

显然，应该用一个**哈希表**来存储字母表，`key-字母`，`value-值`。用哈希表而不是数组的原因在于`hashmap.get()`方法时间复杂度为O(1)。

使用一个辅助函数，根据既定的字母表来比较两个单词的顺序是否符合字典序(升序)。初始化两个指针，比较两个单词的首字母，之后将指针同步后移，直到两个中有任意一个为空，此时再比较**二者长度**即可获得答案。

```java
class Solution {
    public boolean isAlienSorted(String[] words, String order) {
        Map<Character, Integer> map = new HashMap<>();
        for (int i = 0; i < order.length(); i++) {
            map.put(order.charAt(i), i);
        }
        for (int i = 0; i < words.length - 1; i++) {
            if (!compareWords(words[i], words[i + 1], map)) {
                return false;
            }
        }
        return true;
    }
    private boolean compareWords(String s1, String s2, Map<Character, Integer> map) {
        int i = 0, j = 0;
        while (i < s1.length() && j < s2.length()) {
            if (map.get(s1.charAt(i)) < map.get(s2.charAt(j))) {
                return true;
            } else if (map.get(s1.charAt(i)) > map.get(s2.charAt(j))) {
                return false;
            } else {
                i++; j++;
            }
        }
        return s1.length() <= s2.length();
    }
}
```

N: `words.length`

M: `words[i].length()`

该算法时间复杂度为：O(M*N)

空间复杂度为：O(1)
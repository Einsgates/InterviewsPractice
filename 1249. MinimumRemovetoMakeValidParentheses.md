# 1249. Minimum Remove to Make Valid Parentheses

Given a string s of `'('` , `')'` and lowercase English characters.

Your task is to remove the minimum number of parentheses ( `'('` or `')'`, in any positions ) so that the resulting parentheses string is valid and return **any** valid string.

Formally, a parentheses string is valid if and only if:

- It is the empty string, contains only lowercase characters, or
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
- It can be written as (`A`), where `A` is a valid string.

**Example 1:**

```
Input: s = "lee(t(c)o)de)"
Output: "lee(t(c)o)de"
Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.
```

**Example 2:**

```
Input: s = "a)b(c)d"
Output: "ab(c)d"
```

**Example 3:**

```
Input: s = "))(("
Output: ""
```

**Example 4:**

```
Input: s = "(a(b(c)d)"
Output: "a(b(c)d)"
```

## 题目分析

该题可借助一个**栈**来进行匹配，同时建立一个**集合**来存储invalid 符号的下标。如果遇到`(`，将该符号的index压入栈，如果遇到**右括号且栈不为空**，弹出栈顶元素(左括号的索引)；若栈为空，则将当前索引(右括号的下标)存入一个set。

第一遍遍历完之后，先将栈中剩余元素存入set，之后再进行一遍遍历，将set不包含的元素存入 `StringBuilder`即可。

```java
class Solution {
    public String minRemoveToMakeValid(String s) {
        Stack<Integer> stack = new Stack<>();
        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(i);
            } else if (s.charAt(i) == ')') {
                if (!stack.isEmpty()) {
                    stack.pop();
                } else {
                    set.add(i);
                }
            }
        }
        while (!stack.isEmpty()) {
            set.add(stack.pop());
        }
        StringBuilder ss = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            if (!set.contains(i)) {
                ss.append(s.charAt(i));
            }
        }
        return ss.toString();
    }
}
```

时间复杂度：O(N)

空间复杂度O(N)
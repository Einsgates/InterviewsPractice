# 921_MinimumAddtoMakeParenthesesValid

A parentheses string is valid if and only if:

- It is the empty string,
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
- It can be written as `(A)`, where `A` is a valid string.

You are given a parentheses string `s`. In one move, you can insert a parenthesis at any position of the string.

- For example, if `s = "()))"`, you can insert an opening parenthesis to be `"(**(**)))"` or a closing parenthesis to be `"())**)**)"`.

Return *the minimum number of moves required to make* `s` *valid*.

 

**Example 1:**

```
Input: s = "())"
Output: 1
```

**Example 2:**

```
Input: s = "((("
Output: 3
```

**Example 3:**

```
Input: s = "()"
Output: 0
```

**Example 4:**

```
Input: s = "()))(("
Output: 4
```

## 题目分析

建立一个**栈**来进行操作，遇到**左括号就压栈**，右括号则当栈不为空时，找到栈中与之匹配的左括号，找不到就将`cnt`加1，最后将栈中剩余元素全部弹出并记录数量。

```java
class Solution {
    public int minAddToMakeValid(String s) {
        Stack<Character> stack = new Stack<>();
        int cnt = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(s.charAt(i));
            } else {
                if (!stack.isEmpty()) {
                    stack.pop();
                } else {
                    cnt++;
                }
            }
        }
        while (!stack.isEmpty()) {
            cnt++;
            stack.pop();
        }
        return cnt;
    }
}
```

时间复杂度：O(N)

空间复杂度：O(N)
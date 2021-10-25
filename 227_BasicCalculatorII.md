# 227_BasicCalculatorII

Given a string `s` which represents an expression, *evaluate this expression and return its value*. 

The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of `[-231, 231 - 1]`.

**Note:** You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

 

**Example 1:**

```
Input: s = "3+2*2"
Output: 7
```

**Example 2:**

```
Input: s = " 3/2 "
Output: 1
```

**Example 3:**

```
Input: s = " 3+5 / 2 "
Output: 5
```

 

**Constraints:**

- `1 <= s.length <= 3 * 105`
- `s` consists of integers and operators `('+', '-', '*', '/')` separated by some number of spaces.
- `s` represents **a valid expression**.
- All the integers in the expression are non-negative integers in the range `[0, 231 - 1]`.
- The answer is **guaranteed** to fit in a **32-bit integer**.

## 题目分析

使用一个**栈**来**存储数字**，用一个指针遍历整个字符串，遇到数字保存下来并且记录**完整数字**。遇到符合：

- 加减，将之前保存的数字根据正负压入栈中
- 乘除，用栈顶元素对之前保存的数字进行操作

注意，每次遇到符号( `+` `-` `*` `/` )都需要在操作完之后将之前保存的**数字清零**，然后operator保存当前指针内容。

```java
class Solution {
    public int calculate(String s) {
        int len = s.length();
        Stack<Integer> stack = new Stack<>();
        char operator = '+';
        int curr_num = 0;
        int sum = 0;
        for (int i = 0; i < len; i++) {
            char curr_char = s.charAt(i);
            if (Character.isDigit(curr_char)) {
                curr_num = curr_num * 10 + (curr_char - '0');
            }
            if (curr_char == '+' || curr_char == '-' || curr_char == '*' || curr_char == '/' || i == len - 1) {
                switch (operator) {
                    case '+': stack.push(curr_num);
                        break;
                    case '-': stack.push(-curr_num);
                        break;
                    case '*': stack.push(stack.pop() * curr_num);
                        break;
                    case '/': stack.push(stack.pop() / curr_num);
                        break;
                }
                curr_num = 0;
                operator = curr_char;
            }
        }
        while (!stack.isEmpty()) {
            sum += stack.pop();
        }
        return sum;
    }
}
```

时间复杂度：O(N)

空间复杂度：O(N)

### 优化

当然，也可以不用栈，用一个变量保存最后的值，使空间复杂度降低。

```java
class Solution {
    public int calculate(String s) {
        int len = s.length();
        // Stack<Integer> stack = new Stack<>();
        char operator = '+';
        int curr_num = 0, last_num = 0;
        int sum = 0;
        for (int i = 0; i < len; i++) {
            char curr_char = s.charAt(i);
            if (Character.isDigit(curr_char)) {
                curr_num = curr_num * 10 + (curr_char - '0');
            }
            if (curr_char == '+' || curr_char == '-' || curr_char == '*' || curr_char == '/' || i == len - 1) {
                switch (operator) {
                    case '+': sum += last_num;
                        last_num = curr_num;
                        break;
                    case '-': sum += last_num;
                        last_num = -curr_num;
                        break;
                    case '*': last_num *= curr_num;
                        break;
                    case '/': last_num /= curr_num;
                        break;
                }
                curr_num = 0;
                operator = curr_char;
            }
        }
        sum += last_num;
        return sum;
    }
}
```

时间复杂度：O(N)

空间复杂度：O(1)
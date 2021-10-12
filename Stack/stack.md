### 栈(Stack)

特点：后进先出

[394.字符串解码](https://leetcode-cn.com/problems/decode-string/)
```
class Solution:
    def decodeString(self, s: str) -> str:
        """
        思路：辅助栈，遍历字符串，当c为数字时，将数字转化成multi，用于倍数计算
        Time: O(N) 其中N为字符串的长度  Space: O(N)
        """
        stack, res, multi = [], "", 0
        for c in s:
            if c == '[':  # 入栈
                stack.append([multi, res])
                res, multi = "", 0
            elif c == ']':  # 出栈
                cur_multi, last_res = stack.pop()
                res = last_res + cur_multi * res
            elif '0' <= c <= '9':
                multi = multi * 10 + int(c)  # 可能有多位数字
            else:
                res += c
        return res


class Solution:
    def decodeString(self, s: str) -> str:
        """
        思路：递归遍历，当匹配 [ 时，递归函数调用开始；当匹配 ] 时，函数返回，调用结束；
        Time: O(N) 其中N为字符串的长度  Space: O(N)
        """
        def dfs(s, i):
            res, multi = "", 0
            while i < len(s):
                if '0' <= s[i] <= '9':
                    multi = multi*10 + int(s[i])
                elif s[i] == '[':
                    i, tmp = dfs(s, i+1)
                    res += multi * tmp
                    multi = 0
                elif s[i] == ']':
                    return i, res
                else:
                    res += s[i]
                i += 1
            return res
        return dfs(s, 0)
```
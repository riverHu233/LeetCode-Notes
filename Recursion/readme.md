#### 递归

[273.整数转英文表示](https://leetcode-cn.com/problems/integer-to-english-words/)
```
class Solution:
    def numberToWords(self, num: int) -> str:
        """
        思路：将所有的数字表示分为1~19，十位，百位，千位，百万，十亿等不同的英文表示，通过将数字拆分成1000以下的英文表示方法，
        递归调用转换函数，由于每一个单词之间需要留有空格，因此可以先以list的形式来保存英文表示，最后通过字符串拼接方法拼接起来。
        Time: O(N) 其中N为非负整数num的位数
        时间：65mins
        """
        to19 = "One Two Three Four Five Six Seven Eight Nine Ten Eleven Twelve Thirteen Fourteen Fifteen Sixteen Seventeen Eighteen Nineteen".split()
        tens = "Twenty Thirty Forty Fifty Sixty Seventy Eighty Ninety".split()
        thousands = {1000: "Thousand", 1000000: "Million", 1000000000: "Billion"}

        def toWords(n):
            if n< 20:
                return to19[n-1:n]  # slice op, return list
            if n<100:
                return [tens[n//10-2]] + toWords(n%10)
            if n<1000:
                return [to19[n//100 - 1]] + ["Hundred"] + toWords(n%100)
            else:
                for i in [1000000000, 1000000, 1000]:
                    if n // i >0:
                        return toWords(n//i) + [thousands[i]] + toWords(n%i)

        return ' '.join(toWords(num)) if num > 0  else "Zero"


class Solution:
    def __init__(self):
        self.lessThan20 = ["","One","Two","Three","Four","Five","Six","Seven","Eight","Nine","Ten","Eleven","Twelve",
        "Thirteen","Fourteen", "Fifteen","Sixteen", "Seventeen","Eighteen","Nineteen"]
        self.tens = ["","Ten","Twenty","Thirty","Forty","Fifty","Sixty","Seventy","Eighty","Ninety"]
        self.thousands = ["","Thousand","Million","Billion"]

    def numberToWords(self, num: int) -> str:
        if num == 0:
            return 'Zero'
        res = ""
        for i in range(len(self.thousands)):
            if num % 1000 != 0:
                res = self.helper(num%1000) + self.thousands[i] + " " + res
            num = num // 1000
        return res.strip()

    def helper(self, num):
        if num == 0:
            return ""
        elif num < 20:
            return self.lessThan20[num] + " "
        elif num < 100:
            return self.tens[num//10] + " " + self.helper(num%10)
        else:
            return self.lessThan20[num//100] + " Hundred " + self.helper(num%100)
```
### 字符串相加

[415. 字符串相加 - 力扣（LeetCode）](https://leetcode.cn/problems/add-strings/)

```cpp
class Solution {
public:
    string addStrings(string num1, string num2) {
        int end1 = num1.size()-1, end2 = num2.size()-1;

        int next = 0;

        string retStr;
        while (end1 >= 0 || end2 >= 0)
        {
            int x1 = 0;
            if (end1 >= 0)
            {
                x1 = num1[end1] - '0';
                --end1;
            }
                
            int x2 = 0;
            if (end2 >= 0)
            {
                x2 = num2[end2] - '0';
                --end2;
            }

            int retVal = x1 + x2 + next;

            if (retVal > 9) // 如果当前位大于9，那么进位设置为1，否则置为0
            {
                next = 1; 
                retVal -= 10;
            }
            else
            {
                next = 0;
            }

            // 头插
            //retStr.insert(retStr.begin(), retVal+'0');
            // 头插虽好，但是效率低，可以每次都尾插，最后逆置
            retStr += retVal+'0';

        }

        // 处理类似1+9的情况
        if (next == 1)
        {
            //retStr.insert(retStr.begin(), '1');
            retStr += '1';
        }

        reverse(retStr.begin(), retStr.end()); // 传迭代器

        return retStr;
    }
};
```


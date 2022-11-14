### 判断字符串是否为回文字符串

[125. 验证回文串 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-palindrome/)

```cpp
class Solution {
public:
    bool isLeterOrNumber(char ch)
    {
        if (ch >= '0' && ch <= '9')
            return true;

        if (ch >= 'A' && ch <= 'Z')
            return true;

        if (ch >= 'a' && ch <= 'z')
            return true;

        return false;
    }

    bool isPalindrome(string s) {
        int begin = 0, end = s.size() - 1;

        while (begin < end)
        {
            while (begin < end && !isLeterOrNumber(s[begin]))
                ++begin;

            while (begin < end && !isLeterOrNumber(s[end]))
                --end;

            if(tolower(s[begin]) != tolower(s[end]))
                return false;

            ++begin;
            --end;
        }
        return true;
    }
};
```

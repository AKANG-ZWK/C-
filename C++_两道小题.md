### 只反转字母

[917. 仅仅反转字母 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-only-letters/submissions/)

```cpp
class Solution {
public:
    bool IsLetter(char ch)
    {
        if (ch >= 'A' && ch <= 'Z' || ch >= 'a' && ch <= 'z')
        {
            return true;
        }

        return false;
    }

    string reverseOnlyLetters(string s) {
        size_t begin = 0;
        size_t end = s.size() - 1;
		
        // 类似快排里面的思想
        while (begin < end)
        {
            while (begin < end && !IsLetter(s[begin]))
            {
                ++begin;
            }

            while (begin < end && !IsLetter(s[end]))
            {
                --end;
            }

            swap(s[begin], s[end]);
            ++begin;
            --end;

        }

        return s;
    }
};
```

### 找出字符串中第一个没有重复的字母

[387. 字符串中的第一个唯一字符 - 力扣（LeetCode）](https://leetcode.cn/problems/first-unique-character-in-a-string/submissions/)



注意：**统计字母的方式和按照顺序判断字母个数的方式**

```cpp
class Solution {
public:
    int firstUniqChar(string s) {
        int countArray[26] = {0};
        // 统计次数
        // 将s中字母对应的countArray数组里面的位置++，实现对字母个数的统计
        for (size_t i = 0; i < s.size(); ++i)
        {
            countArray[s[i] - 'a']++;
        }

        // 从s中第一个字母开始检查个数
        for (int j = 0; j < s.size(); ++j)
        {
            if (countArray[s[j]-'a'] == 1)
            {
                return j;
            }
        }

        return -1;
    }
};
```
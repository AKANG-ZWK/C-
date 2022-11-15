### 计算字符串中最后一个单词的长度

[字符串最后一个单词的长度_牛客题霸_牛客网 (nowcoder.com)](https://www.nowcoder.com/practice/8c949ea5f36f422594b306a2300315da?tpId=37&&tqId=21224&rp=5&ru=/activity/oj&qru=/ta/huawei/question-ranking)

```cpp
#include <iostream>
using namespace std;

int main()
{
	string s;
	// cin >> s; // 遇到空格会停下
	getline(cin, s);
	
	// getline()的实现原理
	/////////////////////////////////////////////////////////////////////
	/*
	char ch = getchar();

	while (ch != '\n')
	{
		s += ch;
		ch = getchar();
	}

	cout << s << endl;
	*/
	///////////////////////////////////////////////////////////////////

	size_t pos = s.rfind(' ');

	if (pos == string::npos)
	{
		cout << s.size() << endl;
	}
	else
	{
		cout << s.size() - pos - 1;
	}

	return 0;
}
```

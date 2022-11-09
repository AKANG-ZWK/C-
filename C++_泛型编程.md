### 泛型编程

C++相比于C的提升在于支持泛型编程。举例来说，写一个交换函数，

```cpp
void Swap(int& left, int& right)
{
	int temp = left;
	left = right;
	right = temp;
}

void Swap(double& left, double& right)
{
	double temp = left;
	left = right;
	right = temp;
}

void Swap(char& left, char& right)
{
	char temp = left;
	left = right;
	right = temp;
}

// 。。。。。
```

C中有多少个变量类型，就需要写多少个交换函数，极为麻烦。于是C++中提供了模板

```cpp
// 泛型编程
// 模板
template<class T> // 或者template<typename T> // 模板参数列表 -- 参数类型
// template<class T1, class T2> 可以定义多个类型
void Swap(T& x1, T& x2) // 函数参数列表 -- 参数对象
{
	T x = x1;
	x1 = x2;
	x2 = x;
}

int main()
{
	int a = 0, b = 1;
	double c = 1.1, d = 2.22;

	cout << a << " " << b << " " << c << " " << d << endl;

	Swap(a, b);
	Swap(c, d);

	cout << a << " " << b << " " << c << " " << d << endl;

	return 0;
}
```

模板会根据函数形参来替换类型`T`
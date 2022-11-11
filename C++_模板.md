### 模板

> **函数模板概念**
> 函数模板代表了一个函数家族，该函数模板与类型无关，在使用时被参数化，根据实参类型产生函数的特类型版本。
>
> 
>
> **函数模板格式**
> `template<typename T1, typename T2,......,typename Tn>` 返回值类型 函数名(参数列表){}  
>
> **注意**：`typename`是用来定义模板参数关键字，也可以使用`class`(切记：不能使用struct代替class)  

```cpp
template<typename T>
void Swap( T& left, T& right)
{
	T temp = left;
	left = right;
	right = temp;
}
```

### 模板的本质

函数模板是一个**蓝图**，它本身并**不是函数**，是编译器产生特定具体类型函数的模具。所以其实模板就是将本来应该我们做的重复的事情交给了编译器。所以C语言中需要定义的函数都会由编译器来定义。



在编译器编译阶段，对于模板函数的使用，编译器需要**根据传入的实参类型来推演生成对应类型的函数以供调用**。比如：当用double类型使用函数模板时，编译器通过对实参类型的推演，**将T确定为double类型**，然后产生一份专门处理double类型的代码，对于字符类型也是如此  

### 模板显式实例化

```cpp
template<class T>
T Add(const T& left, const T& right)
{
	return left + right;
}

int main()
{
	int a1 = 10, a2 = 20;
	double d1 = 10.0, d2 = 20.0;
	cout << Add(a1, a2) << endl;

	cout << Add(d1, d2) << endl;

	// Add(a1, d2); // 类型不能被匹配到，所以报错
	// 两种解决方案
	cout << Add((double)a1, d2) << endl; 
	cout << Add<double>(a1, d2) << endl; // 显式实例化 ***

	return 0;
}
```

**注意：当普通函数和模板都存在时，优先调用普通函数。（因为模板最终其实还是要生成一个普通函数的）**

### 类模板

```cpp
// 类模板
// 未使用类模板 Stack -- 类型
// 使用类模板   Stack<T> -- 类型

template<class T>
class Stack
{
public:
	Stack(int capacity = 4)
		:_top(0)
		, _capacity(4)
	{
		_a = new T[capacity];
	}

	void Push()
	{}

	~Stack()
	{
		delete[] _a;
		_a = nullptr;
		_capacity = _top = 0;
	}

private:
	T* _a;
	int _top;
	int _capacity;

};

template<class T>
void Stack<T>::Push()
{
	//...
}

int main()
{
	Stack<int> st1; // int 
	Stack<double> st2; // double
	// C无法做到一个栈存int，一个栈存double；C++可以通过类模板实现
	
	return 0;
}
```

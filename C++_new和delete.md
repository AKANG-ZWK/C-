### new和delete - 操作符

**new动态申请得对象：** 申请空间+调用构造函数初始化

**delete释放对象时：** 调用析构函数清理对象中的资源+释放空间

```C++
class A
{
public:
	A()
	{
		cout << "A()" << endl;
	}
	
	~A()
	{
		cout << "~A()" << endl;
	}

private:
	int _a;

};

int main() 
{
	// 动态申请int和5个int数组
	int* p1 = (int*)malloc(sizeof(int));
	int* p2 = (int*)malloc(sizeof(int) * 5);

	int* p3 = new int(5); // 创建1个大小为int的空间，初始化为5 
    
	int* p4 = new int[5]; // 创建5个大小为int的空间，不做初始化

	free(p1);
	free(p2);
	delete p3;
	delete[] p4;

	// 动态申请单个A和5个A对象数组
	A* q1 = (A*)malloc(sizeof(A));
	A* q2 = (A*)malloc(sizeof(A)*5);

	A* q3 = new A;
	A* q4 = new A[5];

	free(q1);
	free(q2);

	delete q3;
	delete[] q4;

	// 对于内置类型，malloc、free 和 new、delete 没有本质区别，
	// 但是对于自定义类型，new、delete会调用构造函数、析构函数
	// new -- delete  和 new [] -- delete[] 一共要匹配使用
	
	return 0;
}
```



 C++提出new和delete主要是解决两个问题

1. 自定义类型对象自动申请的时候，初始化和清理的问题。new/delete会调用构造函数和析构函数
2. new失败了以后要求抛异常，这样才符合面向对象语言的出错处理机制，所以new不用检查是否开辟成功

ps：delete和free一般不会失败，如果失败了都是释放空间上存在越界或者释放指针位置不对。



**举例**

- CPP内存管理和C的不一样

```cpp
struct ListNode
{
	ListNode* _next;
	ListNode* _prev;
	int _val;

	ListNode(int val)
		:_next(nullptr)
		, _prev(nullptr)
		, _val(val)
	{
	}
};

int main()
{
	// C 做法
	struct ListNode* n1 = (struct ListNode*)malloc(sizeof(ListNode));
	n1->_next = nullptr;
	n1->_prev = nullptr;
	n1->_val = 0;

	// CPP 做法
	ListNode* n2 = new ListNode(0);
	// CPP拥有构造函数拥有初始化列表，初始化起来更加轻松，说明CPP更面向对象
	
	return 0;
}
```

- new和delete用法细节

```cpp
class Stack
{
public:
	Stack(int capacity = 4)
		:_top(0)
		, _capacity(4)
	{
		_a = new int[capacity];
	}

	~Stack()
	{
		delete[] _a;
		_a = nullptr;
		_capacity = _top = 0;
	}

private:
	int* _a;
	int _top;
	int _capacity;

};

int main()
{
	Stack st1; // CPP不用再自己初始化了，会自动调用构造函数

	Stack* pst2 = new Stack; // 调用构造函数： 开空间+构造函数初始化

	delete pst2;             // 调用析构函数： 清理对象中的资源 + 释放空间

	return 0;
}

```

**总结**：在C++中建议使用new和delete
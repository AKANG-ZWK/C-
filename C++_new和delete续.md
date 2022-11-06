### operator new与operator delete

> new和delete是用户进行动态内存申请和释放的操作符，operator new 和operator delete是系统提供的全局函数，new在底层调用operator new全局函数来申请空间，delete在底层通过operator delete全局函数来释放空间。  



opertator new 和 operator delete 就是对malloc和free的封装，operator new 和 operator delete中调用malloc申请内存，失败以后（malloc报错）改为抛异常处理错误，这样符合面向对象语言的处理错误的方式

`new Stack : call operator new + call Stack构造函数`



**new 和 delete 的实现原理**

- **内置类型**

  如果申请的是内置类型的空间，new和malloc，delete和free基本类似，不同的地方是：new/delete申请和释放的是单个元素的空间，new[]和delete[]申请的是连续空间，而且new在申请空间失败时会抛异常，malloc会返回NULL  

- **自定义类型**

  - new的原理

    1. 调用operator new函数申请空间

    2. 在申请的空间上执行构造函数，完成对象的构造

  - delete的原理

    1. 在空间上执行析构函数，完成对象中资源的清理工作
    2. 调用operator delete函数释放对象的空间

  - new T[N]的原理

    1. 调operator new[]函数，在operator new[]中实际调用operator new函数完成N个对象空间的申请
    2. 在申请的空间上执行N次构造函数

  - delete[]的原理

    1. 在释放的对象空间上执行N次析构函数，完成N个对象中资源的清理
    2. 调用operator delete[]释放空间，实际在operator delete[]中调用operator delete来释
       放空间  

### 定位new

> ==定位new表达式是在已分配的原始内存空间中调用构造函数初始化一个对象。==
>
> 
>
> **使用格式**：
>
> new (place_address) type或者new (place_address) type(initializer-list)place_address必须是一个指针，initializer-list是类型的初始化列表
>
> 
>
> **使用场景**：
>
> 定位new表达式在实际中一般是配合内存池使用。**因为内存池分配出的内存没有初始化，所以如果是自定义类型的对象，需要使用new的定义表达式进行显示调构造函数进行初始化。**  

```cpp
class A
{
public:
	A(int a = 0)
		: _a(a)
	{
		cout << "A():" << this << endl;
	}
	~A()
	{
		cout << "~A():" << this << endl;
	}
private:
	int _a;
};

// 定位new/replacement new
int main()
{
	// p1现在指向的只不过是与A对象相同大小的一段空间，还不能算是一个对象，因为构造函数没有执行
	A* p1 = (A*)malloc(sizeof(A));

	// 定位new/replace new
	new(p1)A(1); // 注意：如果A类的构造函数有参数时，此处需要传参
	p1->~A(); // 析构函数可以直接显式调用
	free(p1);

	A* p2 = (A*)operator new(sizeof(A));
	new(p2)A(10);
	p2->~A();
	operator delete(p2);

	return 0;
}
```

### malloc/free和new/delete的区别  

**共同点是**：都是从堆上申请空间，并且需要用户手动释放。

**不同的地方是**：

1. malloc和free是函数，new和delete是操作符
2. malloc申请的空间不会初始化，new可以初始化
3. malloc申请空间时，需要手动计算空间大小并传递，new只需在其后跟上空间的类型即可，如果是多个对象，[]中指定对象个数即可
4. malloc的返回值为void*, 在使用时必须强转，new不需要，因为new后跟的是空间的类型
5. malloc申请空间失败时，返回的是NULL，因此使用时必须判空，new不需要，但是new需要捕获异常
6. 申请自定义类型对象时，malloc/free只会开辟空间，不会调用构造函数与析构函数，而new在申请空间后会调用构造函数完成对象的初始化，delete在释放空间前会调用析构函数完成空间中资源的清理  

### 内存泄露

内存泄露 -- 动态申请的内存，不使用了又没有主动释放，就有可能造成内存泄露



==内存泄露的危害是什么？==
a. 出现内存泄露的进程正常结束，进程结束时这些内存会还给系统，不会造成什么大的伤害！
b. 出现内存泄露的进程非正常结束，比如僵尸进程，危害很大，系统会越来越慢，甚至卡死宕机。
c. 需要长期运行的程序，出现内存泄露，危害很大，系统会越来越慢，甚至卡死宕机。 -- 服务器程序
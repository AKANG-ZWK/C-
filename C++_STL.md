### 什么是STL

**STL(standard template libaray-标准模板库)**：是C++标准库的重要组成部分，不仅是一个可复用的组件库，而且是一个包罗**数据结构与算法**的软件框架 

### STL的版本

- **原始版本**

  Alexander Stepanov、Meng Lee 在惠普实验室完成的原始版本，本着开源精神，他们声明允许任何人任意运用、拷贝、修改、传播、商业使用这些代码，无需付费。唯一的条件就是也需要向原始版本一样做开源使用。 HP 版本--所有STL实现版本的始祖。

- **P. J. 版本**

  由P. J. Plauger开发，继承自HP版本，被Windows Visual C++采用，不能公开或修改，缺陷：可读性比较低，符号命名比较怪异。

- **RW版本**

  由Rouge Wage公司开发，继承自HP版本，被C+ + Builder 采用，不能公开或修改，可读性一般。

- **SGI版本**

  由Silicon Graphics Computer Systems，Inc公司开发，继承自HP版 本。被GCC(Linux)采用，可移植性好，可公开、修改甚至贩卖，从命名风格和编程 风格上看，阅读性非常高。**我们后面学习STL要阅读部分源代码，主要参考的就是这个版本**  

### STL六大组件

1. 仿函数
2. 算法
3. 迭代器
4. 空间配置器
5. 容器
6. 配接器



### STL的缺陷

1. STL库的更新太慢了。这个得严重吐槽，上一版靠谱是C++98，中间的C++03基本一些修订。C++11出来已经相隔了13年，STL才进一步更新。
2. STL现在都没有支持线程安全。并发环境下需要我们自己加锁。且锁的粒度是比较大的。
3. STL极度的追求效率，导致内部比较复杂。比如类型萃取，迭代器萃取。
4. STL的使用会有代码膨胀的问题，比如使用vector/vector/vector这样会生成多份代码，当然这是模板语
   法本身导致的。  



### 汉字如何存储的

```cpp
template<class T>
class basic_string
{
private:
	T* _str;
	// ...
};

// typedef basic_string<char> string;
/*
编码 - 值--符号映射关系--编码表
ascii编码表 -- 表示英文字符 utf-8 utf-16 utf-32
unicode -- 表示全世界文字的编码表

*/

int main()
{
	string s1("hello");

	cout << s1 << endl;

	char str2[] = "你好";

	return 0;
}

```

![image-20221026204614358](C:\Users\AKANG\Desktop\编程\Image\image-20221026204614358.png)

前两个字节表示“你”，三四字节表示“好”
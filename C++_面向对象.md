### struct在C++中

```C++
// C++兼容C里面结构体的用法，同时struct在C++中也升级成了类
struct Student
{
	// 为了区分和函数中的形参名，一般对成员变量名前面或后面加_，或者前面加m
	char _name[10];
	int _age;
	int _id;

	// 成员方法/函数
	void Init(const char* name, int age, int id)
	{
		strcpy(_name, name);
		_age = age;
		_id = id;
	}

	void Print()
	{
		cout << _name << endl;
		cout << _age << endl;
		cout << _id << endl;
	}

};

int main()
{
	struct Student s1; // 兼容C
	Student s2; // 升级到类，Student是类名，也是类型

	// 类的处理方式，不需要像C结构体对象一样每个成员变量赋值，可以用一个函数一次性赋值
	s1.Init("zhangsan", 18, 202108101);
	s1.Print();

	return 0;
}
```

### 类的定义

```c++
class classname
{
    // 成员变量
    
    // 成员函数
};
```

class为定义类的关键字，ClassName为类的名字，{}中为类的主体，注意类定义结束时后面分号不能省略。
类体中内容称为类的成员：类中的变量称为类的属性或成员变量; 类中的函数称为类的方法或者成员函数。  

类的两种定义方式：

1. 声明和定义全部放在类体中，需注意：成员函数如果在类中定义，编译器可能会将其当成内
   联函数处理。  
2. 类声明放在.h文件中，成员函数定义放在.cpp文件中，注意：成员函数名前需要加类名::  

### 类访问限定符

C++实现封装的方法是：用类将对象的属性与方法结合在一块，让对象更加完善，通过访问权限选择性的将其接口提供给外部的用户使用 。

类访问限定符： 

- public（公有）
- protect（保护）
- private（私有）

**访问限定符说明**

1. public修饰的成员在类外可以直接被访问
2. protected和private修饰的成员在类外不能直接被访问(此处protected和private是类似的)
3. 访问权限作用域从该访问限定符出现的位置开始直到下一个访问限定符出现时为止
4. 如果后面没有访问限定符，作用域就到 } 即类结束。
5. class的默认访问权限为private，struct为public(因为struct要兼容C)  

**注意**：访问限定符只在编译时有用，当数据映射到内存后，没有任何访问限定符上的区别  

```C++
// 面向对象三大特性：封装、继承、多态
// 封装 ： 1.数据和方法放在一起  2. 访问限定符
// struct 默认都是public , class 默认都是是private
// 一般定义的时候不要用默认的限定符
class Student
{
	// 成员变量
private:
	char _name[10];
	int _age;
	int _id;

	// 成员方法/函数
public:
	void Init(const char* name, int age, int id)
	{
		strcpy(_name, name);
		_age = age;
		_id = id;
	}

	void Print()
	{
		cout << _name << endl;
		cout << _age << endl;
		cout << _id << endl;
	}

};

int main()
{
	struct Student s1; // 兼容C
	Student s2; // 升级到类，Student是类名，也是类型

	// 类的处理方式，不需要像C结构体对象一样每个成员变量赋值，可以用一个函数一次性赋值
	s1.Init("zhangsan", 18, 202108101);
	s1.Print();

	return 0;
}
```

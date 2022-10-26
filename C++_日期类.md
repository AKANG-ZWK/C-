### 日期类

头文件

```c++
#pragma once
#include <iostream>
using namespace std;

class Date
{
	friend ostream& operator<<(ostream& out, const Date& d);
	friend istream& operator>>(istream& in, Date& d);

public:
	// 构造函数
	Date(int year = 1, int month = 1, int day = 1); // 最好只有声明时缺省
	
	void Print() const; 
	// 本来参数应该是 Date* const this,thi这个指针变量不能改变
	// 后面加上const之后，就变成了 const Date* const this, this和*this都会被保护
	// 所以定义的const Date d1就不存在权限放大，相当于权限缩小了

	int GetMonthDay(int year, int month) const;

	// 基础符号重载 
	// 一般只需要实现>和==,其他的都复用。不只是Date类可以这样子，所有类要实现比较，都可以用这种方式
	bool operator>(const Date& d) const;
	bool operator<(const Date& d) const;
	bool operator>=(const Date& d) const;
	bool operator<=(const Date& d) const;
	bool operator==(const Date& d) const;
	bool operator!=(const Date& d) const;

	// + +=
	Date& operator+=(int day); // d1 += 100
	Date operator+(int day) const; // d1 + 100

	// - -=
	Date& operator-=(int day); // d1 -= 100
	Date operator-(int day) const; // d1 - 100

	// ++d1
	Date& operator++();

	// d1++ 为了区分前置++和后置++，后置++增加了一个参数占位(只能是int)，构成函数重载
	Date operator++(int); 

	// --d1
	Date& operator--();

	// --d1
	Date operator--(int);

	// 日期相减
	int operator-(const Date& d) const;

	void PrintWeekDay() const;


private:
	int _year;
	int _month;
	int _day;
};

ostream& operator<<(ostream& out, const Date& d);
istream& operator>>(istream& in, Date& d);
```

函数定义

```c++
#include "Date.h"

int Date::GetMonthDay(int year, int month) const
{
	int monthDayArray[13] = { 0, 31,28,31,30,31,30,31,31,30,31,30,31 };

	int day = monthDayArray[month];

	// 四年一闰，百年不闰，四百年闰
	if (month == 2 && ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0)))
	{ 
		day += 1;
	}

	return day;
}

Date::Date(int year, int month, int day)
{
	_year = year;
	_month = month;
	_day = day;

	if (!(_year >= 0 
		&& (_month > 0 && _month < 13)
		&& (day > 0 && day <= GetMonthDay(year, month))))
	{
		cout << "非法日期->";
		// this->Print();
		Print();
	}
}

void Date::Print() const
{
	cout << _year << "-" << _month << "-" << _day << endl;
}

// >
bool Date::operator>(const Date& d) const
{
	if (_year > d._year)
	{
		return true;
	}
	else if (_year == d._year && _month > d._month)
	{
		return true;
	}
	else if (_year == d._year && _month == d._month && _day > d._day)
	{
		return true;
	}
	else
	{
		return false;
	}
}

// ==
bool Date::operator==(const Date& d) const
{
	return _year == d._year
		&& _month == d._month
		&& _day == d._day;
}

// >=
bool Date::operator>=(const Date& d) const
{
	return *this > d || *this == d;
}

// >
bool Date::operator<(const Date& d) const
{
	return !(*this >= d);
}

// <=
bool Date::operator<=(const Date& d) const
{
	return !(*this > d);
}

// !=
bool Date::operator!=(const Date& d) const
{
	return !(*this == d);
}

// +=
Date& Date::operator+=(int day)
{
	if (day < 0)
	{
		return *this -= -day;
	}

	_day += day;
	while (_day > GetMonthDay(_year, _month))
	{
		_day -= GetMonthDay(_year, _month);
		++_month;

		if (_month == 13)
		{
			_month = 1;
			_year++;
		}
	}

	return *this;
}

// +
Date Date::operator+(int day) const
{
	Date ret(*this);

	ret += day; // ret.operate+=(day);

	return ret;
}

Date& Date::operator-=(int day)
{
	if (day < 0)
	{
		return *this += -day;
	}

	/*while (_day <= day)
	{
		day -= _day;
		_month--;
		if (_month == 0)
		{
			_month = 12;
			_year--;
		}
		_day = GetMonthDay(_year, _month);
	}
	_day -= day;*/
	
	_day -= day;

	while (_day <= 0)
	{
		--_month;
		if (_month == 0)
		{
			--_year;
			_month = 12;
		}
		_day += GetMonthDay(_year, _month);
	}

	return *this;
}

Date Date::operator-(int day) const
{
	Date ret(*this);

	ret -= day;

	return ret;
}

// 前置++ ————自定义类型推荐使用前置++
Date& Date::operator++()
{
	*this += 1;

	return *this;
}

// 后置++
Date Date::operator++(int)
{
	Date ret(*this); // 后置++需要调用一次拷贝构造函数

	*this += 1;

	return ret;
}

// --d1
Date& Date::operator--()
{
	*this -= 1;

	return *this;
}

// --d1
Date Date::operator--(int)
{
	Date ret(*this);

	*this -= 1;

	return ret;

}

int Date::operator-(const Date& d) const
{
	Date max = *this;
	Date min = d;

	int flag = 1;
	if (*this < d)
	{
		max = d;
		min = *this;
		flag = -1;
	}

	int count = 0;
	while (min != max)
	{
		++min;
		++count;
	}

	return count * flag;
}

void Date::PrintWeekDay() const
{
	const char* arr[7] = { "星期一", "星期二", "星期三", "星期四", "星期五", "星期六", "星期日" };
	Date start(1900, 1, 1);
	int count = *this - start;
	cout << arr[count % 7] << endl;
}

ostream& operator<<(ostream& out, const Date& d)
{
	out << d._year << "/" << d._month << "/" << d._day << endl;
	return out;
}

istream& operator>>(istream& in, Date& d)
{
	cout << "请依次输入年月日，以空格隔开:> ";
	in >> d._year >> d._month >> d._day;

	return in;
}
```

测试程序

```c++
#include "Date.h"

void TestDate1()
{
	Date d1;
	Date d2(2022, 1, 1);

	Date d3(2000, 2, 29);
}

void TestDate2()
{
	Date d1(2022, 1, 16);

	Date d2 = d1 + 100;
	d2.Print();

	d1 += 100;
	d1.Print();

	d1++; // d1.operator(&d1, 1); 1没有实际意义，就是用来区分而已
	++d1; // d1.operator(&d1);


	d1 -= 20;
	d1.Print();
}

void TestDate3()
{
	Date d1(2022, 1, 17);

	Date ret1 = d1 - 10;
	ret1.Print();

	Date ret2 = d1 - 17;
	ret2.Print();

	Date ret3 = d1 - 30;
	ret3.Print();

	Date ret4 = d1 - 400;
	ret4.Print();

	Date ret5 = d1 - 1500;
	ret5.Print();

	Date ret6 = d1 - -100;
	ret6.Print();

	Date ret7 = d1 + -1500;
	ret7.Print();

}

void TestDate4()
{
	Date today(2022, 10, 1);
	Date offerDay(2022, 9, 1);

	cout << (offerDay - today) << endl;
	cout << (today - offerDay) << endl;

	today.PrintWeekDay();
	Date(2022, 10, 2).PrintWeekDay(); // 用匿名对象打印今天星期几
}

int main()
{
	//TestDate2();
	//TestDate3();
	//TestDate4();
	Date d1;
	d1.Print();

	const Date d2;
	d2.Print();

	Date d3(2022, 10, 2);
	Date d4(2022, 10, 3);

	// 如果将<<重载为成员们函数，那么的第一个参数肯定是对象，所以就出现了下面的调用方式
	// 因此我们应该将<<重载在类外面，为了避免不能访问私有成员的问题，采用友元函数来实现
	//d3.operator<<(cout);
	cout << d3 << d4;

	Date d5, d6;

	cin >> d5 >> d6;
	cout << d5 << d6 << endl;

	return 0;
}
```

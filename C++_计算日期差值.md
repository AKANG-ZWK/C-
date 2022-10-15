### 计算日期差值

[日期差值_牛客题霸_牛客网 (nowcoder.com)](https://www.nowcoder.com/practice/ccb7383c76fc48d2bbc27a2a6319631c?tpId=62&&tqId=29468&rp=1&ru=)



> 思路：先比较年份的大小，让小的年份一天一天来追赶大的年份，然后用一个变量记录下追赶了多少天就好。



因为这道题输入的第一行的年份肯定是小于第二个年份的，所以说不用比较年份的大小，如果想实现比较完美的日期计算器就需要判断。

```C++
#include <iostream>
using namespace std;


// 判断闰年
int isLeapYear(int year) {
    if (year % 400 == 0 || (year % 4 == 0 && year % 100 != 0))
        return 1;

    return 0;
}

// 输出每个月的天数
int MonthDay(int year, int month) {
    int day = 0;
    int monthDay[13] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

    day = monthDay[month];
    if (month == 2 && isLeapYear(year))
    {
        day+=1;
    }
    
    return day;
}


int SubDate(int year1, int month1, int day1, int year2, int month2, int day2 )
{
    int count = 0;
    while (year1 != year2)
    {
        if (month1 < 13)
        {
            count += MonthDay(year1, month1); // 如果小的年份是7月，那么从7月的天数开始加
        }
        
        month1++;

        if (month1 > 12)
        {
            month1 = 1;
            year1++;
        }
    }

    while (month1 != month2)
    {
        count += MonthDay(year1, month1);
        month1++;        
    }

    if (day1 > day2)
    {
        count -= (day1 - day2);
    }

    if (day1 < day2)
    {
        count += (day2 - day1);
    }

    count++;

    return count;
}

int main() {
    // 先拿到年月日
    int a, b;
    cin >> a >> b;

    int year1 = a / 10000;
    int year2 = b / 10000;
    int month1 = (a - year1 * 10000) / 100;
    int month2 = (b - year2 * 10000) / 100;
    int day1 = a % 100;
    int day2 = b % 100;

    int count = SubDate(year1, month1, day1, year2, month2, day2);
    
    cout << count << endl;

}
```

**比较完美的版本**

```C++
#include<iostream>
using namespace std;

// 判断闰年
int isLeapYear(int year) {
	if (year % 400 == 0 || (year % 4 == 0 && year % 100 != 0))
		return 1;

	return 0;
}

// 输出每个月的天数
int MonthDay(int year, int month) {
	int day = 0;
	int monthDay[13] = { 0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };

	day = monthDay[month];
	if (month == 2 && isLeapYear(year))
	{
		day += 1;
	}


	return day;
}

bool CompareDate(int year1, int month1, int day1, int year2, int month2, int day2)
{
	if (year1 < year2)
	{
		return true;
	}
	else if (year1 == year2 && month1 < month2)
	{
		return true;
	}
	else if (year1 == year2 && month1 == month2 && day1 < day2)
	{
		return true;
	}
	else
		return false;
}

void Swap(int& x, int& y)
{
	int tmp = x;
	x = y;
	y = tmp;
}


int SubDate(int year1, int month1, int day1, int year2, int month2, int day2)
{
	int count = 0;
	while (year1 != year2)
	{
		if (month1 < 13)
		{
			count += MonthDay(year1, month1);
		}
		month1++;

		if (month1 > 12)
		{
			month1 = 1;
			year1++;
		}
	}

	while (month1 != month2)
	{
		count += MonthDay(year1, month1);
		month1++;
	}

	if (day1 > day2)
	{
		count -= (day1 - day2);
	}

	if (day1 < day2)
	{
		count += (day2 - day1);
	}

	//count++; // 牛客网的题需要++

	return count;
}

int main() {
	// 先拿到年月日
	int a, b;
	cin >> a >> b;

	int flag = -1;
	int year1 = a / 10000;
	int year2 = b / 10000;
	int month1 = (a - year1 * 10000) / 100;
	int month2 = (b - year2 * 10000) / 100;
	int day1 = a % 100;
	int day2 = b % 100;

	if (CompareDate(year1, month1, day1, year2, month2, day2) == false)
	{
		int tmp = year1;
		Swap(year1, year2);
		Swap(month1, month2);
		Swap(day1, day2);
		flag = -1;
	}

	int count = SubDate(year1, month1, day1, year2, month2, day2);

	cout << flag*count << endl;

	return 0;
}
```
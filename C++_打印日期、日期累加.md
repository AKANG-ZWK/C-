### 打印日期

[打印日期_牛客题霸_牛客网 (nowcoder.com)](https://www.nowcoder.com/practice/b1f7a77416194fd3abd63737cdfcf82b?tpId=69&&tqId=29669&rp=1&ru=/activity/oj&qru=/ta/hust-kaoyan/question-ranking)



```c++
#include <iostream>
#include <stdio.h>
using namespace std;

class Date
{
public:
    Date(int year = 0, int month = 0, int day = 0)
    {
        _year = year;
        _day = day;

        _month = 1;

        while (_day > GetMonthDay(_year, _month))
        {
            _day -= GetMonthDay(_year, _month);
            _month++;
        }
    }

    int GetMonthDay(int year, int month)
    {
        int day = 0;
        int MonthDay[13] = {0, 31,28,31,30,31,30,31,31,30,31,30,31};
        day = MonthDay[month];
        if (month == 2 && ((year % 400 == 0)||(year % 4 == 0 && year % 100 != 0)))
        day += 1;
        return day;
    }

    void Print()
    {
        printf("%d-%02d-%02d\n", _year, _month, _day);
    }

private:
    int _year;
    int _month;
    int _day;
};

int main() {
    int a, b;
    while (cin >> a >> b) { // 注意 while 处理多个 case
        Date A(a, 0, b);
        A.Print();
    }
}
```

### 日期累加

[日期累加_牛客题霸_牛客网 (nowcoder.com)](https://www.nowcoder.com/practice/eebb2983b7bf40408a1360efb33f9e5d?tpId=40&&tqId=31013&rp=1&ru=/activity/oj&qru=/ta/kaoyan/question-ranking)

```c++
#include <iostream>
#include <stdio.h>
using namespace std;

class Date 
{
public:
    Date(int year = 0, int month = 0, int day = 0, int count = 0)
    {
        _year = year;
        _month = month;
        _day = day + count;

       while (_day > GetMonthDay(_year, _month)) 
       {
           if (_month < 13)
           {
               _day -= GetMonthDay(_year, _month);
               ++_month;
           }

           if (_month > 12)
           {
               _month = 1;
               ++_year;
           }
       }
    }

    int GetMonthDay(int year, int month)
    {
        int day = 0;
        int MonthDay[13] = {0, 31, 28, 31, 30 ,31, 30, 31, 31, 30, 31, 30, 31};
        
        day = MonthDay[month];

        if (month == 2 && ((year % 400 == 0)|| (year % 4 == 0 && year % 100 != 0)))
        {
            day += 1;
        }    

        return day;    
    }

    void Print()
    {
        printf("%d-%02d-%02d\n", _year, _month, _day);
    }
    
private:
    int _year;
    int _month;
    int _day;
};

int main() {
    int n, a, b, c, d;
    cin >> n;
    for (int i = 0; i < n; i++)
    {
        cin >> a >> b >> c >> d;
        Date A(a, b, c, d);
        A.Print();
    }
}
```

****
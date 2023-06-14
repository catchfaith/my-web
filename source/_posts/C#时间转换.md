---
title: 记录CSharp获取系统当前时间、当前日期、年月日时分秒等
date: 2023-06-06 11:07:05
---

## CSharp中获取系统当前时间、当前日期、年月日时分秒
**code实例：**
``` java
string st0 = DateTime.Now.ToString();            // "2022/8/25 17:55:01"
string st1 = DateTime.Now.Date.ToString();            // "2022/8/25 0:00:00"

DateTime t2 = DateTime.Now.ToLocalTime();        // {2022/8/25 17:55:01}
string st2 = DateTime.Now.ToLocalTime().ToString();        // "2022/8/25 17:55:01"
//获取日期
string t3 = DateTime.Now.ToLongDateString();    // "2022年8月25日, 星期四"
string st3 = DateTime.Now.ToLongDateString().ToString();    // "2022年8月25日, 星期四"

string t4 = DateTime.Now.ToShortDateString();    // "2022/8/25"
string st4 = DateTime.Now.ToShortDateString().ToString();    // "2022/8/25"
//获取当前年月日并格式化
string st5 = DateTime.Now.ToString("yyyy-MM-dd");        // "2022-08-25"
//获取时间
string t6 = DateTime.Now.ToLongTimeString();   // "17:55:01"
string st6 = DateTime.Now.ToLongTimeString().ToString();   // "17:55:01"

string t7 = DateTime.Now.ToShortTimeString();   // "17:55"
string st7 = DateTime.Now.ToShortTimeString().ToString();   // "17:55"

string st8 = DateTime.Now.ToString("hh:mm:ss");        // "05:55:01"

string st9 = DateTime.Now.TimeOfDay.ToString();        // 17:55:01.8069038

//可以获得当前时间的长整型数字，这个数字应该是由年精确到微秒(或许更多)，用它作文件名的话几乎不会重复
long t10 = DateTime.Now.ToFileTime();       // 133058949018078916
string st10 = DateTime.Now.ToFileTime().ToString();       // "133058949018078916"

long t11 = DateTime.Now.ToFileTimeUtc();   // 133058949018078916
string st11 = DateTime.Now.ToFileTimeUtc().ToString();   // "133058949018078916"

double t12 = DateTime.Now.ToOADate();       // 44798.746548692128
string st12 = DateTime.Now.ToOADate().ToString();       // "44798.7465486921"

//将当前DateTime对象的值转换为世界标准时间(UTC)
DateTime t13 = DateTime.Now.ToUniversalTime();   // {2022/8/25 9:55:01}
string st13 = DateTime.Now.ToUniversalTime().ToString();   // "2022/8/25 9:55:01"

string st14 = DateTime.Now.Year.ToString(); //获取年份  // "2022"
string st15 = DateTime.Now.Month.ToString(); //获取月份   // "8"
string st16 = DateTime.Now.DayOfWeek.ToString(); //获取星期   // "Thursday"
string st17 = DateTime.Now.DayOfYear.ToString(); //获取第几天   // "237"
string st18 = DateTime.Now.Hour.ToString(); //获取小时   // "17"
string st19 = DateTime.Now.Minute.ToString(); //获取分钟   // "55"
string st20 = DateTime.Now.Second.ToString(); //获取秒数   // "2"
```
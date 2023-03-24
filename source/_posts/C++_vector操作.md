---
title: C++_vector操作
urlname: rtd32q
date: '2021-05-15 21:26:38 +0800'
tags: []
categories: []
---

---

## title: C++\_vector 操作 tags: "C++"

# C++\_vector 操作

## 1. vector：

#### 1.1 vector 说明

- vector 是向量类型，可以容纳许多类型的数据，因此也被称为容器
- (可以理解为动态数组，是封装好了的类）
- 进行`vector`操作前应添加头文件`#include`

#### 1.2 vector 初始化：

**方式 1.**

```cpp
//定义具有10个整型元素的向量（尖括号为元素类型名，它可以是任何合法的数据类型），不具有初值，其值不确定
vector<int>a(10);
```

**方式 2.**

```cpp
//定义具有10个整型元素的向量，且给出的每个元素初值为1
vector<int>a(10,1);
```

**方式 3.**

```cpp
//用向量b给向量a赋值，a的值完全等价于b的值
vector<int>a(b);
```

**方式 4**.

```cpp
//将向量b中从0-2（共三个）的元素赋值给a，a的类型为int型
vector<int>a(b.begin(),b.begin+3);
```

**方式 5.**

```cpp
 //从数组中获得初值
int b[7]={1,2,3,4,5,6,7};
vector<int> a(b,b+7）;
```

#### 1.3 vector 对象的常用内置函数使用（举例说明）

```cpp
#include<vector>
vector<int> a,b;
//b为向量，将b的0-2个元素赋值给向量a
a.assign(b.begin(),b.begin()+3);
//a含有4个值为2的元素
a.assign(4,2);
//返回a的最后一个元素
a.back();
//返回a的第一个元素
a.front();
//返回a的第i元素,当且仅当a存在
a[i];
//清空a中的元素
a.clear();
//判断a是否为空，空则返回true，非空则返回false
a.empty();
//删除a向量的最后一个元素
a.pop_back();
//删除a中第一个（从第0个算起）到第二个元素，也就是说删除的元素从a.begin()+1算起（包括它）一直到a.begin()+3（不包括它）结束
a.erase(a.begin()+1,a.begin()+3);
//在a的最后一个向量后插入一个元素，其值为5
a.push_back(5);
//在a的第一个元素（从第0个算起）位置插入数值5,
a.insert(a.begin()+1,5);
//在a的第一个元素（从第0个算起）位置插入3个数，其值都为5
a.insert(a.begin()+1,3,5);
//b为数组，在a的第一个元素（从第0个元素算起）的位置插入b的第三个元素到第5个元素（不包括b+6）
a.insert(a.begin()+1,b+3,b+6);
//返回a中元素的个数
a.size();
//返回a在内存中总共可以容纳的元素个数
a.capacity();
//将a的现有元素个数调整至10个，多则删，少则补，其值随机
a.resize(10);
//将a的现有元素个数调整至10个，多则删，少则补，其值为2
a.resize(10,2);
//将a的容量扩充至100，
a.reserve(100);
//b为向量，将a中的元素和b中的元素整体交换
a.swap(b);
//b为向量，向量的比较操作还有 != >= > <= <
a==b;
```

## 2. 顺序访问 vector 的几种方式，举例说明

#### 2.1. 对向量 a 添加元素的几种方式

1.向向量 a 中添加元素

```cpp
vector<int>a;
for(int i=0;i<10;++i){a.push_back(i);}
```

2.从数组中选择元素向向量中添加

```cpp
int a[6]={1,2,3,4,5,6};
vector<int> b;
for(int i=0;i<=4;++i){b.push_back(a[i]);}
```

3.从现有向量中选择元素向向量中添加

```cpp
int a[6]={1,2,3,4,5,6};
vector<int>b;
vector<int>c(a,a+4);
for(vector<int>::iterator it=c.begin();it<c.end();++it)
{
	b.push_back(*it);
}
```

4.从文件中读取元素向向量中添加

```cpp
ifstream in("data.txt");
vector<int>a;
for(int i;in>>i){a.push_back(i);}
```

5.常见错误赋值方式

```cpp
vector<int>a;
for(int i=0;i<10;++i){a[i]=i;}//下标只能用来获取已经存在的元素
```

2.2 从向量中读取元素

1.通过下标方式获取

```cpp
int a[6]={1,2,3,4,5,6};
vector<int>b(a,a+4);
for(int i=0;i<=b.size()-1;++i){cout<<b[i]<<endl;}
```

2.通过迭代器方式读取

2.通过迭代器方式读取

```cpp
int a[6]={1,2,3,4,5,6};
vector<int>b(a,a+4);
for(vector<int>::iterator it=b.begin();it!=b.end();it++){cout<<*it<<"  ";}
```

## 3.几个常用的算法

```cpp
 #include<algorithm>
 //对a中的从a.begin()（包括它）到a.end()（不包括它）的元素进行从小到大排列
 sort(a.begin(),a.end());
 //对a中的从a.begin()（包括它）到a.end()（不包括它）的元素倒置，但不排列，如a中元素为1,3,2,4,倒置后为4,2,3,1
 reverse(a.begin(),a.end());
  //把a中的从a.begin()（包括它）到a.end()（不包括它）的元素复制到b中，从b.begin()+1的位置（包括它）开始复制，覆盖掉原有元素
 copy(a.begin(),a.end(),b.begin()+1);
 //在a中的从a.begin()（包括它）到a.end()（不包括它）的元素中查找10，若存在返回其在向量中的位置
  find(a.begin(),a.end(),10);
```

## find 使用

不同于 map（map 有 find 方法），vector 本身是没有 find 这一方法，其 find 是依靠 algorithm 来实现的。

话不多说，上代码：

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main()
{
   using namespace std;

   vector<int> vec;

   vec.push_back(1);
   vec.push_back(2);
   vec.push_back(3);
   vec.push_back(4);
   vec.push_back(5);
   vec.push_back(6);

   vector<int>::iterator it = find(vec.begin(), vec.end(), 6);

   if (it != vec.end())
       cout<<*it<<endl;
   else
       cout<<"can not find"<<endl;

   return 0;
}
```

记着要包含 algorithm 这一头文件，其定义了 find 这一函数。

---
title: C++集合操作之集合交集：stdset_intersection
urlname: cf2f4g
date: '2021-05-15 21:26:43 +0800'
tags: []
categories: []
---

---

## title: C 集合操作之集合交集：std::set_intersectiontags: "C"

# C++集合操作之集合交集：std::set_intersection

算法 set_intersection 可以用来求两个集合的交集，此处的集合可以为 std::set，也可以是 std::multiset，但是不可以是 hash_set 以及 hash_multiset。为什么呢？因为 set_intersection 要求两个区间必须是有序的（从小到大排列），std::set 和 std::multiset 为有序序列，而 hash_set 以及 hash_multiset 为无序序列。

由于两个集合内的每个元素都不需唯一，因此，如果某个值在区间 1 中出现 m 次，在区间 2 中出现 n 次，那么该值在输出区间中会出现 min(m,n)次，且全部来自于区间 1.函数返回值为一个迭代器，指向输出区间的尾部。

set_intersection 为稳定操作，即输出区间内的每个元素的相对顺序都和区间 1 内的相对顺序相同。

一定谨记：两个区间必须是有序区间（从小到大）

源码如下：

```cpp
template<class InputIterator1,class InputIterator2,class OutputIterator>
OutputIterator set_intersection(InputIterator1 first1,InputIterator1 last1,InputIterator2 first2,InputIterator2 last2,OutputIterator result)
{
	while (first1!=last1 && first2!=last2) //若均未到达尾端，则进行以下操作
	{
		//在两个区间内分别移动迭代器。若二者值相同用result记录该值，移动first1,first2和result
		//若first1较小，则移动first1，其他不动
		//若first2较小，则移动first2，其他不动
		if (*first1<*first2)
			first1++;
		else if (*first2<*first1)
			first2++;
		else
		{
			*result=*first1;
			first1++;
			first2++;
			result++;
		}
	}
	return result;
}
```

示例：

```cpp
int main(void)
{
	int iarr1[]={1,2,3,3,4,5,6,7,9};
	int iarr2[]={1,4,3,9,10};
	multiset<int> iset1(begin(iarr1),end(iarr1));
	multiset<int> iset2(begin(iarr2),end(iarr2));
	vector<int> ivec(20);
	auto iter=set_intersection(iset1.begin(),iset1.end(),iset2.begin(),iset2.end(),ivec.begin());	//ivec为：1,3,4,9
	ivec.resize(iter-ivec.begin());//重新确定ivec大小
	return 0;
}
```

其实 ，上述代码并不仅限于对两个集合取交集，只要是两个有序区间，均可以用此代码求交集。

用于非 set 场合：

```cpp
int main(void)
{
	int iarr1[]={1,2,3,3,6,7,4,5};
	int iarr2[]={1,4,3,10,9};
	std::sort(begin(iarr1),end(iarr1));
	std::sort(begin(iarr2),end(iarr2));
	vector<int> ivec(10);
	auto iter=set_intersection(begin(iarr1),end(iarr1),begin(iarr2),end(iarr2),ivec.begin());	//ivec为：1,2,4,9
	ivec.resize(iter-ivec.begin());//重新确定ivec大小
	return 0;
}
```

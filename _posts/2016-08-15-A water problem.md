---
layout:     post
title:      "A water problem"
subtitle:   "网络赛整理"
date:       2016-08-15
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
- HDU
- ACM
---

## A water problem
### Problem Description
Two planets named Haha and Xixi in the universe and they were created with the universe beginning.

There is 73 days in Xixi a year and 137 days in Haha a year. 

Now you know the days N after Big Bang, you need to answer whether it is the first day in a year about the two planets.
### Input
There are several test cases(about 5 huge test cases).

For each test, we have a line with an only integer N(0≤N), the length of N is up to 10000000.
### Output
For the i-th test case, output Case #i: , then output "YES" or "NO" for the answer.

主要就是大数取模的问题，涉及到秦九韶算法,数论

数论的的基本定理：

```
(a + b) mod c = 
	((a mod c) + (b mod c)) mod c
```
把一个n次多项式
{% raw %}
  $$f(x) = {a_n}x^n + {a_{n-1}x^{n-1}} + \cdot\cdot\cdot+{a_1}x+a_0  $$
{% endraw %}
改写成如下形式：



综上，代码如下：


```
#include <iostream>
#include <cstring>
#include <cstdio>
using namespace std;
#define N 10000010
//#define N 1000
int cnt;
int main()
{
	char m[N];
	while (scanf("%s", m) != EOF)
	{
		int d = 0;
		for (int i = 0; i<strlen(m); i++)
			d = ((d * 10) % 10001 + m[i] - '0') % 10001;
		if (d == 0)
			printf("Case #%d: YES\n", ++cnt);
		else
			printf("Case #%d: NO\n", ++cnt);
		memset(m, 0, sizeof(m));
	}
	return 0;
}
```

{% raw %}
  $$a^2 + b^2 = c^2$$ --> note that all equations between these tags will not need escaping! 
 {% endraw %}

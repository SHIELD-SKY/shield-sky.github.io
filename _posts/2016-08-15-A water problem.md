---
layout:     post
title:      "A water problem"
subtitle:   "网络赛整理"
date:       2016-08-15 23:23:30
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
- HDU
- ACM
- Latex
- 数论
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

{% raw %} 
$$\begin{eqnarray}
     f(x) &=& {a_n}x^n + {a_{n-1}x^{n-1}} + \cdot\cdot\cdot+{a_2}x^2+{a_1}x+a_0\nonumber \\
             &=& ({a_n}x^{n-1} + {a_{n-1}x^{n-2}} + \cdot\cdot\cdot+{a_2}x^2+{a_1})x+a_0 \nonumber \\
             &=& (({a_n}x^{n-2} + {a_{n-1}x^{n-3}} + \cdot\cdot\cdot+{a_2})x+{a_1})x+a_0\nonumber \\
             &\vdots \nonumber \\
             &=& ((\cdot\cdot\cdot({a_n}x + a_{n-1})x+a_{n-2})x\cdot\cdot\cdot+a_1)x+a_0\nonumber
\end{eqnarray}$$
{% endraw %}

这里把要测试的数看成是 x = 10，各位数字为a的多项式的和，结合数论的基本定理即可解之：


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

#### PS：
吐槽一下，实验了好多次终于知道怎么在这里面用latex写漂亮的数学公式了，但是真的好麻烦。。。。我就是按照[这个](http://wanguolin.github.io/mathmatics_rending/)在default.html里加句代码，然后如果对自己写的Latex代码信心不足，可以先在[这个](http://mathurl.com)地方实验，这个网站是实时生成公式，挺方便的，另外可参考
[How to supported latex in github-pages](http://stackoverflow.com/questions/26275645/how-to-supported-latex-in-github-pages)


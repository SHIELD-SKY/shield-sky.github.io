---
layout:     post
title:      "I Count Two Three"
subtitle:   "二分 预处理"
date:       2016-09-17 22:00:3 0
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
- ACM
- 二分
- 预处理
---

### I Count Two Three

#### Problem Description

I will show you the most popular board game in the Shanghai Ingress Resistance Team.
It all started several months ago.
We found out the home address of the enlightened agent Icount2three and decided to draw him out.
Millions of missiles were detonated, but some of them failed.

After the event, we analysed the laws of failed attacks.
It's interesting that the i-th attacks failed if and only if i can be rewritten as the form of 2a3b5c7d which a,b,c,d are non-negative integers.

At recent dinner parties, we call the integers with the form 2a3b5c7d "I Count Two Three Numbers".
A related board game with a given positive integer n from one agent, asks all participants the smallest "I Count Two Three Number" no smaller than n.

#### Input
The first line of input contains an integer t (1≤t≤500000), the number of test cases. t test cases follow. Each test case provides one integer n (1≤n≤109).
#### Output
For each test case, output one line with only one integer corresponding to the shortest "I Count Two Three Number" no smaller than n.

```c++
#include <iostream>
#include <cstring>
#include <cstdio>
#include <cstdlib>
#include <cmath>
#include <algorithm>

using namespace std;
typedef long long LL;
const LL INF = 1e18+100;
const int MAXN = 70*70*70*70;
LL a[MAXN];
int cnt = 0;
void Init()
{
    cnt = 0;
    for(LL i=1; i<INF; i*=2)
        for(LL j=1; j*i<INF; j*=3)
            for(LL k=1; i*j*k<INF; k*=5)
                for(LL m=1; i*j*k*m<INF;m*=7)
                		a[cnt++] = i*j*k*m;
}
int main()
{
    Init();
    sort(a, a+cnt);
    int T;
    cin>>T;
    while(T--)
    {
        LL n;
        scanf("%lld",&n);
        if(n <= 10) printf("%lld\n", n);
        else{
            printf("%lld\n",a[lower_bound(a+1,a+cnt+1,n)-a]);
        }
    }
    return 0;
}
```
---
layout: post
title: hdu 4324 Triangle LOVE
subtitle: hdu 4324 Triangle LOVE
date: 2018-12-14
author: ikka
header-img: ./img/bg1.jpg
tags: 
     - OI
     - 图论
     - 三元环
     - 结论题
---

## [hdu 4324] Triangle LOVE

**Description**

  Recently, scientists find that there is love between any of two people. For example, between $$ A $$ and $$ B $$, if $$ A $$ don’t love $$ B $$, then $$ B $$ must love $$ A $$, vice versa. And there is no possibility that two people love each other, what a crazy world!
  Now, scientists want to know whether or not there is a “Triangle Love” among $$ N $$ people. “Triangle Love” means that among any three people ($$ A $$,$$ B $$ and $$ C $$) , $$ A $$ loves $$ B $$, $$ B $$ loves $$ C $$ and $$ C $$ loves $$ A $$.
  Your problem is writing a program to read the relationship among $$ N $$ people firstly, and return whether or not there is a “Triangle Love”.

**Input Format**

  The first line contains a single integer $$ t (1 <= t <= 15) $$, the number of test cases.
  For each case, the first line contains one integer $$ N (0 < N <= 2000) $$.
  In the next N lines contain the adjacency matrix A of the relationship (without spaces). $$ A_{i,j} = 1 $$ means $$ i-th $$ people loves $$ j-th $$ people, otherwise $$ A_{i,j} = 0 $$.
  It is guaranteed that the given relationship is a tournament, that is, $$ A_{i,i}= 0, A_{i,j} \neq A_{j,i}(1<=i, j<=n,i \neq j) $$.

**Output Format**

  For each case, output the case number as shown and then print “Yes”, if there is a “Triangle Love” among these $$ N $$ people, otherwise print “No”.
  Take the sample output for more details.


**Input Sample**

```
2
5
00100
10000
01001
11101
11000
5
01111
00000
01000
01100
01110
```

**Output Sample**

```
Case #1: Yes
Case #2: No
```

### Solution

题意：给你一个竞赛图，判断图中是否存在三元环。

要考虑竞赛图这样一个性质，**如果竞赛图中有任意长度的环，则一定存在三元环**。

于是这个题就变成判断是否存在环了，**如果一个竞赛图是DAG的话，那么它的每个节点的度数一定是 $$ 0 $$ 到 $$ n-1 $$ 的一个排列**。

于是这道题就做完了。

**Code**

``` cpp
#include <cstdio>

#include <cstring>

#include <cctype>

const int maxn = 2010;

int p[maxn], in[maxn];

int getch() {
  int t;
  while (!isdigit(t = getchar()));
  return t ^ 48;
}

int main() {
  int T, n;
  scanf("%d", &T);

  for (int cnt = 1; cnt <= T; ++cnt) {
    memset(p, 0, sizeof p);
    memset(in, 0, sizeof in);
    scanf("%d", &n);
    for (int i = 1; i <= n; ++i) {
      for (int j = 1; j <= n; ++j) {
        if (getch()) in[j]++;
      }
    }
    int flag = 1;
    for (int i = 1; i <= n; ++i) {
      if (in[i] < 0 || in[i] > n - 1 || p[in[i]]) {
        flag = 0; break;
      } else {
        p[in[i]] = 1;
      }
    }
    printf("Case #%d: %s\n", cnt, flag ? "No" : "Yes");
  }
  return 0;
}
```

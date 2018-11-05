---
layout: post
title: 中途相遇法优化算法
subtitle: 中途相遇(Meet-in-The-Middle)在OI中的应用
date: 2018-11-05
author: ikka
header-img: ./img/home-bg.png
tags: 
     - OI
     - Meet-in-The-Middle
     - 暴力
---

# 扯

昨天刚刚奶了wxy阿克noip，结果今天noip模拟就被wxy大爷阿克掉了……tql

还是今天的模拟题，虽然只是三道sd题目，但是我却只有160分……

原因是T2不会做，T3输入顺序写错~~（该打）~~。

考完之后去问wxy大爷，然后发现，T2不是一个我曾经用来出题的算法吗？？？

然而我现在忘了，这个算法就是密码学中的**中途相遇攻击（Meet-in-The-Middle Attack）**。

# Chapter 1

用我的话来说，中途相遇法把问题分成两部分，然后求解一半，将答案保存下来，然后再去解另一半，和之前保存过的另一半的答案合并求出全局最优解。

比如说今天的模拟赛，题意大概是这样的：

> 给你两个数$$n,k$$，有$$n$$件物品可以选，你有$$k$$元钱，第$$i$$件物品的价格为$$v_{i}$$，每件物品只能选一次，求剩下的钱的最小值。
> $$n \leq 40, 1 \leq k, v_{i} \leq 10^{9}$$。

这个题就是用中途相遇法优化的。

### Solution

首先能想到的是枚举子集，即枚举选不选当前这件物品，复杂度$$O(2^{n})$$，显然会T到飞起。

考虑先搜前一半，即搜$$\frac{n}{2}$$的大小，不会TLE。然后将搜到的每一个小于等于$$k$$的值保存下来，再去搜另一半，设$$x$$为$$k$$减去搜到的值，然后再在刚才保存的前一半的值中进行二分查找，查找第一个小于$$x$$的元素，用$$x$$减去这个值，这个值就是可以剩下的钱，再用这个值去更新最小值即可。

时间复杂度$$O(2^{\frac{n}{2}}\log 2^{\frac{n}{2}})$$，即$$O(n\times 2^{\frac{n}{2}})$$。

### Code

``` cpp
#include <cstdio>

#include <iostream>

#include <algorithm>

const int maxn = 50;
const int maxm = 2000010;

int n, k, m;
int v[maxn];
int x[maxm];
int ans = 0x7fffffff;

void dfs1(int p, int lm) {
  if (p == m + 1) {
    if (lm <= k) x[++x[0]] = lm;
  } else {
    dfs1(p + 1, lm + v[p]);
    dfs1(p + 1, lm);
  }
}

void dfs2(int p, int lm) {
  if (p == m) {
    if (lm <= k) {
      int p = *(std::upper_bound(x + 1, x + x[0] + 1, k - lm) - 1);
      ans = std::min(ans, k - lm - p);
    }
  } else {
    dfs2(p - 1, lm + v[p]);
    dfs2(p - 1, lm);
  }
}

int main() {
  scanf("%d%d", &n, &k);
  m = n >> 1;
  for (int i = 1; i <= n; ++i) scanf("%d", v + i);
  dfs1(1, 0); std::sort(x + 1, x + x[0] + 1);
  dfs2(n, 0);
  printf("%d\n", ans);
  return 0;
}
```

# Chapter 2

接下来看几道例题。

## [UVA1152 4 Values whose Sum is 0](https://uva.onlinejudge.org/external/11/p1152.pdf)

### Solution

首先还是考虑暴力，~~根据套路~~会发现暴力是过不去的，接下来考虑优化。

先把前两个数组的所有$$n^{2}$$个和求出来，然后存到哈希表中，然后再求后两个数组的相反数，在哈希表中查找，统计答案。

$$O(n^{4})$$瞬间就变成了$$O(n^{2})$$（如果用map是$$O(n^{2}\log n)$$的）。

### Code

``` cpp
#include <cstdio>

#include <tr1/unordered_map>

const int maxn = 10010;

int a[maxn][4];
std::tr1::unordered_map<int, int> mp;

int main() {
  int T, n;
  scanf("%d", &T);
  while (T--) {
    long long ans = 0;
    scanf("%d", &n);
    for (int i = 1; i <= n; ++i) {
      scanf("%d%d%d%d", &a[i][0], &a[i][1], &a[i][2], &a[i][3]);
    }
    for (int i = 1; i <= n; ++i) {
      for (int j = 1; j <= n; ++j) {
        mp[a[i][0] + a[j][1]]++;
      }
    }
    for (int i = 1; i <= n; ++i) {
      for (int j = 1; j <= n; ++j) {
        ans += mp[-a[i][2]-a[j][3]];
      }
    }
    mp.clear();
    printf("%lld\n", ans);
    if (T) puts("");
  }
  return 0;
}
```

## [UVA1326 Jurassic Remains](https://uva.onlinejudge.org/external/13/p1326.pdf)

### Solution

~~根据套路，我们不考虑暴力。~~

选择的字符串中有偶数个相同字母，可以看做是异或运算，把每个字符串按照是否有某个字母存在压成一个26位的二进制数。

然后答案即转化为求选出尽可能多的数使得这些数的异或等于$$0$$。

然后就可以用中途相遇优化。

先求前一半的异或值，存入map中，然后再求另一半的异或值，在map中查找

$$O(n\times 2^{n})$$瞬间就变成了$$O(n\times 2^{\frac{n}{2}})$$（如果用map是$$O(n\times 2^{\frac{n}{2}}\log n)$$的）。

### Code

``` cpp
#include <bits/stdc++.h>

#include <tr1/unordered_map>

#define idx(x) (x - 'A')

const int maxn = 1000;

char g[maxn][maxn];
int sta[maxn];
std::tr1::unordered_map<int, int> mp;

int bc(int x) { return x ? (x & 1) + bc(x >> 1) : 0; }

int main() {
  int n;
  while (~scanf("%d", &n)) {
    memset(sta, 0, sizeof sta);
    for (int i = 1; i <= n; ++i) scanf("%s", g[i] + 1);
    int s = n >> 1, t = n - s;
    int _max__ = 0, ans = 0;
    for (int i = 1; i <= n; ++i) {
      g[i][0] = strlen(g[i] + 1);
      for (int j = 1; j <= g[i][0]; ++j) {
        sta[i] |= 1 << idx(g[i][j]);
      }
    }
    for (int i = 0; i < (1 << s); ++i) {
      int res = 0;
      for (int j = 0; j < s; ++j) if ((i >> j) & 1) res ^= sta[j + 1];
      mp[res] = i;
    }
    for (int i = 0; i < (1 << t); ++i) {
      int res = 0;
      for (int j = 0; j < t; ++j) if ((i >> j) & 1) res ^= sta[s + j + 1];
      if (!mp.count(res)) continue;
      int d = mp[res];
      if (bc(d) + bc(i) > _max__) ans = d | (i << s), _max__ = bc(d) + bc(i);
    }
    printf("%d\n", _max__);
    for (int i = 0; i < n; ++i) {
      if ((ans >> i) & 1) printf("%d%c", i + 1, " \n"[i == n - 1]);
    }
    puts("");
    mp.clear();
  }
  return 0;
}
```

# 总结

中途相遇法其实就是用来解决问题可以分成几块单独考虑且答案可以由分成的每块的选择合并的一系列问题。

**本质是用空间换取时间。**

---
layout: post
title: 算法模板
subtitle: 自带大常数的模板
date: 2018-10-07
author: ikka
header-img: ./img/home-bg.png
tags: 
     - OI
     - 模板
---

因为我天天为了不会写算法而烦恼……                    
所以我准备写一点模板~~以备不时之需~~以便随时复习。            

# 图论

### toposort(bfs)

``` cpp
for (int i = 0; i < n; ++i) if (!in[i]) q.push(i);
while (!q.empty()) {
  int u = q.front(); q.pop();
  tps[++cnt] = u;
  for (edge *i = fi[u]; i; i = i -> next) {
    if (--in[i -> v] == 0) q.push(i -> v);
  }
}
```

### 二分图染色判断奇偶环

``` cpp
void dfs(int u, int fa) {
  for (edge *i = fi[u]; i; i = i -> next) {
    int v = i -> v;
    if (v == fa) continue;
    if (~vis[v]) {
      if (vis[u] == vis[v]) odd = 1;
      else even = 1;
    }
    if (!i -> vis) i -> vis = 1, vis[v] = vis[u] ^ 1, dfs(v, u);
  }
  vis[u] = -1;
}
```

# 字符串

### 字符串最小表示法

``` cpp
int minrep(int n) {
  int i = 0, j = 1, k = 0;
  while (i < n && j < n && k < n) {
    int t = s[(i + k) % n] - s[(j + k) % n];
    if (t) {
      if (t > 0) i += k + 1;
      else j += k + 1;
      if (i == j) ++j;
      k = 0;
    } else {
      ++k;
    }
  }
  return std::min(i, j);
}
```



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

### 

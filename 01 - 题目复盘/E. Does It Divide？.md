# E. Does It Divide？

## 基本信息

来源：ZJU Summer 2026 Contest 3  
难度：中等偏难 / 期望数论转化题  
状态：提示后完成  
掌握程度：大致理解  
比赛日期：2026-07-08  
复盘日期：2026-07-08  
入库日期：2026-07-08  
题目链接：https://codeforces.com/gym/703113/problem/E  

## 题意简述

Alice 从 $1$ 到 $n$ 中等概率随机选择一个正整数 $m$。

Bob 每次可以选择一个正整数 $x$，询问：

$$
m \bmod x = 0
$$

也就是问：$m$ 是否是 $x$ 的倍数，或者说 $x$ 是否整除 $m$。

Alice 会如实回答 Yes 或 No。

Bob 可以根据之前的回答动态调整之后的问题，目标是用尽可能少的期望询问次数，唯一确定 $m$ 的值。

要求输出最小期望询问次数。

因为答案可能是分数，所以需要在模 $998244353$ 意义下输出。

## 第一反应

一开始看到题面时没有思路。

题面里同时出现了：

- 期望；
- 最优策略；
- 动态调整问题；
- 模意义下输出分数。

第一反应是这题可能是概率 DP、决策树，或者很复杂的最优策略题。

当时我对期望的理解也不够熟，对模意义下的分数和逆元也不熟，所以没有马上把题目转化出来。

一开始没有意识到这题真正要做的不是直接模拟 Bob 的所有可能策略，而是先固定真实数字 $m$，思考确定这个 $m$ 需要多少次询问。

## 错误思路

一开始没有形成有效做法。

主要错误是被题面表述吓住了：

```text
期望不知道怎么处理
最优询问策略不知道怎么建模
模意义下分数和逆元不熟
数论结构不知道从哪里出现
```

正确方向应该是先固定真实数字 $m$，思考在最优策略下确定这个 $m$ 需要多少次询问，也就是求：

$$
depth(m)
$$

然后因为 $m$ 在 $1$ 到 $n$ 中等概率随机选择，所以答案就是所有 $m$ 的询问次数平均值：

$$
ans=\frac{\sum_{m=1}^{n} depth(m)}{n}
$$

## 核心思路

这题的核心转化是：

```text
不要直接模拟 Bob 的所有策略，
而是固定真实答案 m，计算确定 m 需要多少次询问。
```

因为 $m$ 是从 $1$ 到 $n$ 等概率随机选择的，所以最小期望询问次数可以写成：

$$
ans=\frac{\sum_{m=1}^{n} depth(m)}{n}
$$

其中 $depth(m)$ 表示真实数字是 $m$ 时，最优策略下需要询问多少次才能唯一确定 $m$。

接下来问题就变成：

```text
如何计算每个 m 的 depth(m)？
```

Bob 每次问：

$$
m \bmod x = 0
$$

本质是在确认 $m$ 的因子信息。

由于一个整数可以由质因子唯一分解，所以最优询问过程可以理解成逐步确认 $m$ 的质因子，同时排除一些不可能的额外质因子。

## Yes 次数：Omega(m)

如果真实数字是：

$$
m=12=2^2 \times 3
$$

那么它的质因子按重复次数计算有：

$$
2,2,3
$$

每确认一个质因子，通常对应一次 Yes。

因此，对于 $m>1$，Yes 次数是：

$$
\Omega(m)
$$

其中 $\Omega(m)$ 表示 $m$ 的质因子个数，重复计算。

例如：

$$
\Omega(12)=3
$$

因为：

$$
12=2^2 \times 3
$$

## No 次数的第一类来源：找到最大质因子

设 $P(m)$ 表示 $m$ 的最大质因子。

为了走到 $m$ 的最大质因子，需要排除最大质因子前面的质数。

这部分 No 次数是：

$$
\pi(P(m))-1
$$

其中 $\pi(x)$ 表示不超过 $x$ 的质数个数。

这里减 $1$ 的原因是：

```text
最大质因子 P(m) 本身对应的是一次 Yes，
已经被 Omega(m) 统计过了。
```

所以只需要统计它前面的质数。

## No 次数的第二类来源：排除 m 的更大倍数

即使已经确认答案至少包含了 $m$ 的质因子结构，还要确认答案正好是 $m$，而不是 $m$ 的更大倍数。

例如真实答案是 $m$，但还可能需要排除：

$$
m \times 2,\ m \times 3,\ m \times 5,\dots
$$

如果：

$$
m \times p \le n
$$

那么 $m \times p$ 仍然是一个可能答案，需要被排除。

也就是：

$$
p \le \left\lfloor\frac{n}{m}\right\rfloor
$$

所以这类 No 的数量是：

$$
\pi\left(\left\lfloor\frac{n}{m}\right\rfloor\right)
$$

## 为什么两类 No 取 max，而不是相加

这是这题最容易卡住的地方之一。

两类 No 看起来像两部分：

1. 为了找到最大质因子 $P(m)$，要排除最大质因子前面的质数；
2. 为了确认答案不是 $m$ 的更大倍数，要排除不超过 $\lfloor n/m \rfloor$ 的质数。

但是它们不是两段独立流程。

它们本质上都在扫同一串质数前缀：

$$
2,3,5,7,11,\dots
$$

所以为了同时满足这两个要求，只需要扫到更远的那个位置。

因此 No 次数不是相加，而是：

$$
\max\left(
\pi\left(\left\lfloor\frac{n}{m}\right\rfloor\right),
\pi(P(m))-1
\right)
$$

## depth(m) 公式

对于 $m=1$，它没有任何质因子。

如果真实数字是 $1$，Bob 需要排除所有不超过 $n$ 的质数，所以：

$$
depth(1)=\pi(n)
$$

对于 $m>1$，询问次数由 Yes 次数和 No 次数组成：

$$
depth(m)
=
\Omega(m)
+
\max\left(
\pi\left(\left\lfloor\frac{n}{m}\right\rfloor\right),
\pi(P(m))-1
\right)
$$

因此答案为：

$$
ans
=
\frac{
\pi(n)
+
\sum_{m=2}^{n}
\left[
\Omega(m)
+
\max\left(
\pi\left(\left\lfloor\frac{n}{m}\right\rfloor\right),
\pi(P(m))-1
\right)
\right]
}{n}
$$

## 如何预处理公式需要的量

因为 $n \le 10^7$，可以用线性筛预处理数论信息。

需要维护：

```text
spf[i]：i 的最小质因子
omega[i]：i 的质因子个数，重复计算
maxPrime[i]：i 的最大质因子
pi[i]：不超过 i 的质数个数
```

先用线性筛求 `spf[i]`。

然后用 `spf[i]` 递推：

$$
omega[i]=omega\left(\frac{i}{spf[i]}\right)+1
$$

含义是：

```text
把 i 拆掉一个最小质因子以后，
剩下部分的质因子个数已经算过，
再加上拆出来的这个质因子。
```

最大质因子也可以递推：

$$
maxPrime[i]=
\max\left(
maxPrime\left(\frac{i}{spf[i]}\right),
spf[i]
\right)
$$

最后用：

```cpp
spf[i] == i
```

判断 $i$ 是否是质数，并做前缀和得到：

$$
\pi(i)
$$

## 模意义下的分数

最终答案是：

$$
\frac{sum}{n}
$$

但题目要求在模 $998244353$ 意义下输出，所以不能直接做除法。

需要转成：

$$
sum \times n^{-1} \bmod 998244353
$$

因为 $998244353$ 是质数，所以根据费马小定理：

$$
n^{-1}
\equiv
n^{998244353-2}
\pmod {998244353}
$$

用快速幂求出逆元即可。

## 知识链路

主链路：

[[数学]]
→ [[期望]]
→ [[期望转平均贡献]]
→ [[决策树深度]]
→ [[固定真实答案分析代价]]

数论链路：

[[数论]]
→ [[质因数分解]]
→ [[最小质因子筛]]
→ [[质因子个数 Omega]]
→ [[最大质因子]]
→ [[质数计数函数]]

模运算链路：

[[数学]]
→ [[模意义下的分数]]
→ [[模逆元]]
→ [[费马小定理]]
→ [[快速幂]]

思维链路：

[[思维方法]]
→ [[固定真实答案分析代价|不要直接模拟策略]]
→ [[把最优策略问题转成单点代价公式]]
→ [[转化|数论公式转化]]

实现链路：

[[C++实现细节]]
→ [[线性筛]]
→ [[最小质因子筛|最小质因子 spf]]
→ [[前缀计数]]

## 为什么我会卡住

这题卡住不是因为代码一开始就很复杂，而是没有找到正确的抽象方式。

我一开始看到"期望 + 最优策略 + 动态询问"时，容易往复杂 DP 或决策树搜索方向想。

真正应该做的是：

```text
先固定 m，
再问最优情况下确定这个 m 要几次，
最后对所有 m 取平均。
```

第二个卡点是没有马上看出询问：

$$
m \bmod x = 0
$$

和质因子之间的关系。

这类询问本质上是在确认 $m$ 的因子结构，而整数的因子结构又由质因子分解决定，所以公式自然会和：

```text
质因子个数
最大质因子
质数前缀数量
```

有关。

第三个卡点是没有理解两类 No 为什么取 max。

这其实是因为两类 No 都是在扫同一串质数前缀，不是两条互相独立的路线。

## 正确做法步骤

1. 用线性筛预处理 `spf[i]`。
2. 用 `spf[i]` 递推 `omega[i]`，表示 $i$ 的质因子个数，重复计算。
3. 用 `spf[i]` 递推 `maxPrime[i]`，表示 $i$ 的最大质因子。
4. 用 `spf[i] == i` 判断质数，做前缀和得到 `pi[i]`。
5. 初始化：

$$
sum=\pi(n)
$$

表示 $m=1$ 的贡献。

6. 枚举 $m=2$ 到 $n$，计算：

$$
depth(m)
=
\Omega(m)
+
\max\left(
\pi\left(\left\lfloor\frac{n}{m}\right\rfloor\right),
\pi(P(m))-1
\right)
$$

7. 把所有 `depth(m)` 加到 `sum` 中。
8. 最终答案为：

$$
sum \times n^{-1} \bmod 998244353
$$

9. 用快速幂计算 $n^{-1}$。

## 代码实现要点

- `spf[i]` 表示最小质因子。
- `omega[i] = omega[i / spf[i]] + 1`。
- `maxPrime[i] = max(maxPrime[i / spf[i]], spf[i])`。
- `pi[i]` 是质数个数前缀和。
- $m=1$ 要单独处理，贡献是 `pi[n]`。
- `pi[maxPrime[i]] - 1` 中的 `-1` 表示最大质因子本身已经被 Yes 统计。
- 两类 No 用 `max`，不要相加。
- 模意义下除以 $n$ 要乘 `qpow(n, mod-2)`。
- 线性筛里乘法判断建议写成 `1LL * i * p > n`，避免潜在溢出风险。

## 参考代码

```cpp
#include <bits/stdc++.h>
#define code using
#define by namespace
#define Mxiaocao std
code by Mxiaocao;

using ll = long long;

const int mod = 998244353;

ll qpow(ll a,ll b){
    ll res = 1;
    while(b){
        if(b & 1) res = res * a % mod;
        a = a * a % mod;
        b >>= 1;
    }
    return res;
}

void solve(){
    int n; cin >> n;

    vector<int> spf(n+1);
    vector<int> primes;
    vector<int> omega(n+1);
    vector<int> maxPrime(n+1);
    vector<int> pi(n+1);

    for(int i = 2;i <= n;++i){
        if(!spf[i]){
            spf[i] = i;
            primes.push_back(i);
        }
        for(auto p : primes){
            if(1LL * i * p > n) break;
            spf[i*p] = p;
            if(i % p == 0) break;
        }
    }

    for(int i = 2;i <= n;++i){
        omega[i] = omega[i/spf[i]] + 1;
        maxPrime[i] = max(maxPrime[i/spf[i]],spf[i]);
    }

    for(int i = 2;i <= n;++i){
        pi[i] = pi[i-1];
        if(spf[i] == i){
            pi[i]++;
        }
    }

    ll sum = pi[n];

    for(int i = 2;i <= n;++i){
        int dep = omega[i] + max(pi[n/i],pi[maxPrime[i]] - 1);
        sum = (sum + dep) % mod;
    }

    ll ans = sum * qpow(n,mod-2) % mod;
    cout << ans << '\n';
}

signed main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    solve();
    return 0;
}
```

## 复杂度

线性筛：

$$
O(n)
$$

递推 `omega` 和 `maxPrime`：

$$
O(n)
$$

计算 `pi`：

$$
O(n)
$$

枚举所有 $m$ 求和：

$$
O(n)
$$

总时间复杂度：

$$
O(n)
$$

空间复杂度：

$$
O(n)
$$

## 本题总结

这题表面是：

```text
期望 + 最优询问策略
```

但真正落地以后是：

```text
固定真实答案 m
计算确定 m 的 depth(m)
对所有 m 求平均
```

核心公式是：

$$
depth(m)
=
\Omega(m)
+
\max\left(
\pi\left(\left\lfloor\frac{n}{m}\right\rfloor\right),
\pi(P(m))-1
\right)
$$

其中：

- $\Omega(m)$ 表示确认 $m$ 的质因子需要的 Yes 次数；
- $\pi(P(m))-1$ 表示找到最大质因子前需要排除的质数；
- $\pi(\lfloor n/m \rfloor)$ 表示为了确认答案不是 $m$ 的更大倍数，需要排除的质数；
- 两类 No 是同一串质数前缀，所以取 max，而不是相加。

这题给我的最大提醒是：

```text
遇到期望最优策略题，不一定要直接设计整个策略树。
可以先固定真实答案，把期望转成所有答案的代价平均值。
```

## 二刷提醒

本题必须二刷。

二刷时不要直接背公式，要按下面顺序重新推：

1. 为什么期望可以写成：

$$
ans=\frac{\sum depth(m)}{n}
$$

2. 为什么询问过程和质因子有关。
3. 为什么 Yes 次数是：

$$
\Omega(m)
$$

4. 为什么要排除 $m$ 的更大倍数，推出：

$$
\pi\left(\left\lfloor\frac{n}{m}\right\rfloor\right)
$$

5. 为什么找到最大质因子需要：

$$
\pi(P(m))-1
$$

6. 为什么两类 No 取 max，而不是相加。
7. 最后再写筛法预处理 `spf`、`omega`、`maxPrime`、`pi`。

二刷重点不是代码，而是独立推出 `depth(m)` 公式。

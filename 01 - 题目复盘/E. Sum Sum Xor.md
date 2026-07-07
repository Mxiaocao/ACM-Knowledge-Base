# E. Sum Sum Xor

## 基本信息

来源：ZJU Summer 2026 Warmup
难度：E
状态：提示后完成
当前掌握程度：大致理解，代码实现已修正，但 `countOne` 与 `even / odd` DP 仍需二刷巩固
比赛日期：2026/7/5
复盘日期：2026/7/7
入库日期：2026/7/7
题目链接：https://codeforces.com/gym/702542/problem/E
正式文件名：`E. Sum Sum Xor.md`

## 题意简述

给定 `n` 个区间：

```text
[l1, r1], [l2, r2], ..., [ln, rn]
```

需要从每个区间中各选一个数：

```text
x1 ∈ [l1, r1]
x2 ∈ [l2, r2]
...
xn ∈ [ln, rn]
```

对于每一种选法，计算：

```text
x1 xor x2 xor ... xor xn
```

题目要求把所有可能选法得到的 XOR 值全部加起来，并对：

$$
10^9 + 7
$$

取模。

也就是求：

$$
\sum (x_1 \oplus x_2 \oplus \cdots \oplus x_n)
$$

其中每个：

$$
x_i \in [l_i, r_i]
$$

## 数据范围分析

数据范围：

```text
1 <= n <= 5 * 10^5
0 <= li <= ri <= 10^9
```

每个区间长度可能很大，不能枚举区间内的数，更不能枚举所有组合。

所有数不超过：

$$
10^9
$$

而：

$$
2^{30} > 10^9
$$

所以只需要考虑大约 `30` 个二进制位。

因此本题可接受的复杂度方向是：

$$
O(30n)
$$

## 第一反应

一开始容易把题目理解成枚举所有选法：

```text
for x1 in [l1, r1]
    for x2 in [l2, r2]
        ...
            计算 x1 xor x2 xor ... xor xn
```

但所有选法数量是：

$$
(r_1-l_1+1)(r_2-l_2+1)\cdots(r_n-l_n+1)
$$

这个数量完全无法枚举。

真正要做的是把 XOR 的整体求和拆成每一位的贡献。

## 核心思路

一个整数的值可以拆成每个二进制位的贡献：

```text
第 0 位贡献 2^0
第 1 位贡献 2^1
第 2 位贡献 2^2
...
```

所以总答案可以拆成：

```text
枚举每一位 b
统计所有组合中，第 b 位 XOR 结果为 1 的组合数
贡献 = 组合数 * 2^b
```

于是本题变成：

```text
固定某一位 b，统计有多少种选法使得这一位最终 XOR 为 1。
```

对于 XOR 来说，某一位最终为 `1`，等价于：

```text
这一位上选到 1 的次数是奇数。
```

因此，固定一位 `b` 后，每个区间只需要知道：

```text
cnt1 = 当前区间中第 b 位为 1 的数的个数
cnt0 = 当前区间中第 b 位为 0 的数的个数
```

然后用 `even / odd` DP 统计当前处理到若干个区间后，第 `b` 位上选到 `1` 的个数是偶数还是奇数。

## 知识链路

主链路：

[[位运算]]
→ [[按位拆贡献]]
→ [[XOR 性质]]
→ [[奇偶 DP]]
→ [[模意义下计数]]

副链路 1：

[[前缀计数]]
→ [[周期计数]]
→ [[区间内某一位为 1 的数量]]

副链路 2：

[[组合计数]]
→ [[从枚举所有组合到统计贡献]]
→ [[大数据范围下避免枚举]]

副链路 3：

[[C++实现细节]]
→ [[位运算左移]]
→ [[不要把异或当乘方]]
→ [[long long 与取模]]

## 关键知识点

### 1. XOR 可以按位拆贡献

最终要求的是所有 XOR 值的总和。

由于每个数都可以写成二进制位贡献之和，所以可以逐位统计。

如果第 `b` 位在某些选法中最终 XOR 为 `1`，那么这些选法对答案贡献：

$$
2^b
$$

因此第 `b` 位的总贡献是：

$$
cnt_b \times 2^b
$$

其中 `cnt_b` 表示所有选法中第 `b` 位 XOR 结果为 `1` 的选法数。

### 2. 某一位 XOR 为 1 等价于 1 的个数为奇数

固定第 `b` 位，每个数这一位只有两种情况：

```text
0 或 1
```

XOR 的规律是：

```text
1 出现偶数次 -> XOR 结果为 0
1 出现奇数次 -> XOR 结果为 1
```

所以问题转化为：

```text
从每个区间各选一个数，有多少种选法使得第 b 位上 1 的个数为奇数。
```

### 3. even / odd DP

固定某一位 `b`。

定义：

```text
even = 当前已经处理的区间中，第 b 位选到 1 的个数为偶数的选法数
odd  = 当前已经处理的区间中，第 b 位选到 1 的个数为奇数的选法数
```

初始时还没有选任何数，`1` 的个数为 `0`，属于偶数：

```text
even = 1
odd = 0
```

处理一个新区间时：

* 选到第 `b` 位为 `0` 的数，不改变奇偶性；
* 选到第 `b` 位为 `1` 的数，会翻转奇偶性。

所以转移为：

$$
newEven = even \times cnt0 + odd \times cnt1
$$

$$
newOdd = even \times cnt1 + odd \times cnt0
$$

处理完所有区间后，`odd` 就是第 `b` 位 XOR 结果为 `1` 的选法数。

### 4. 区间中某一位为 1 的数量

需要快速计算：

```text
[l, r] 中第 b 位为 1 的数有多少个
```

先定义：

```text
countOne(x, b)
```

表示：

```text
[0, x] 中第 b 位为 1 的数的数量
```

第 `b` 位的变化是周期性的。

设：

$$
half = 2^b
$$

$$
cycle = 2^{b+1}
$$

第 `b` 位的模式是：

```text
连续 half 个 0
连续 half 个 1
然后重复
```

对于 `[0, x]`，一共有：

$$
x + 1
$$

个数。

完整周期数：

$$
full = \frac{x+1}{cycle}
$$

每个完整周期中有：

$$
half
$$

个 `1`。

剩余长度：

$$
rem = (x+1) \bmod cycle
$$

剩余部分中，前 `half` 个是 `0`，超过 `half` 的部分才是 `1`。

所以：

$$
countOne(x,b)=full \times half + \max(0, rem-half)
$$

区间 `[l, r]` 中第 `b` 位为 `1` 的数量就是：

$$
countOne(r,b)-countOne(l-1,b)
$$

## 为什么我会卡住

这题主要卡在三个地方。

第一，没有马上把“所有 XOR 值求和”拆成“每一位分别统计贡献”。

题面看起来像巨大组合求和，如果不先想到按位拆，就会被组合数量吓住。

第二，固定一位后，没有立刻把 XOR 转成奇偶问题。

这一步是本题最关键的转化：

```text
第 b 位 XOR 为 1
等价于
第 b 位选到 1 的次数为奇数
```

第三，`countOne(x, b)` 的周期统计容易写错。

容易混淆：

```text
half = 2^b
cycle = 2^(b+1)
full = (x+1) / cycle
rem = (x+1) % cycle
```

尤其容易漏掉：

```text
完整周期贡献是 full * half
```

以及：

```text
剩余部分贡献是 max(0, rem - half)
```

## 正确做法步骤

1. 读入 `n` 和所有区间 `[l_i, r_i]`。

2. 初始化答案：

```cpp
res = 0;
```

3. 枚举二进制位 `b`。

4. 对当前位初始化：

```cpp
even = 1;
odd = 0;
```

5. 遍历每个区间 `[l_i, r_i]`，计算：

```cpp
cnt1 = countOne(r[i], b) - countOne(l[i] - 1, b);
len = r[i] - l[i] + 1;
cnt0 = len - cnt1;
```

6. 用 `cnt0 / cnt1` 更新奇偶 DP：

```cpp
new_even = even * cnt0 + odd * cnt1;
new_odd  = even * cnt1 + odd * cnt0;
```

7. 处理完所有区间后，当前位的贡献是：

```cpp
odd * (1LL << b)
```

8. 累加到答案并取模。

## 代码实现要点

### 1. `^` 不是乘方

C++ 中：

```cpp
2 ^ (b + 1)
```

不是：

$$
2^{b+1}
$$

而是按位异或。

正确写法是：

```cpp
1LL << (b + 1)
```

### 2. `countOne` 的周期由位数 `b` 决定

错误思路是把统计上界 `x` 和位数 `b` 混在一起。

正确写法：

```cpp
ll half = 1LL << b;
ll cycle = 1LL << (b + 1);
```

### 3. `cnt0` 是区间长度减去 `cnt1`

不能写成：

```cpp
cnt0 = (1 << (b + 1)) - cnt1;
```

因为 `cnt0` 表示的是当前区间 `[l, r]` 内第 `b` 位为 `0` 的数的个数。

正确写法：

```cpp
ll len = r - l + 1;
ll cnt0 = len - cnt1;
```

### 4. DP 转移中要取模

方案数会非常大，所以：

```cpp
even
odd
cnt0
cnt1
res
```

都要使用 `long long`。

DP 转移时也要取模：

```cpp
new_even %= mod;
new_odd %= mod;
```

### 5. `countOne(l - 1, b)` 要处理负数

当 `l = 0` 时，会调用：

```cpp
countOne(-1, b)
```

所以函数开头要写：

```cpp
if(x < 0) return 0;
```

## 参考代码

```cpp
#include <bits/stdc++.h>
#define code using
#define by namespace
#define Mxiaocao std
code by Mxiaocao;

using ll = long long;

const ll mod = 1000000007;

ll count1(ll x,ll b){
    if(x < 0) return 0;

    x++;
    ll half = 1LL << b;
    ll cycle = 1LL << (b + 1);
    ll full = x / cycle;
    ll rem = x % cycle;

    return full * half + max(0LL, rem - half);
}

void solve(){
    int n; cin >> n;
    vector<ll> l(n), r(n);

    for(int i = 0;i < n;++i){
        cin >> l[i] >> r[i];
    }

    ll res = 0;

    for(int b = 0;b < 32;++b){
        ll even = 1, odd = 0;

        for(int i = 0;i < n;++i){
            ll cnt1 = count1(r[i], b) - count1(l[i] - 1, b);
            ll len = r[i] - l[i] + 1;
            ll cnt0 = len - cnt1;

            cnt1 %= mod;
            cnt0 %= mod;

            ll new_even = (even * cnt0 + odd * cnt1) % mod;
            ll new_odd = (even * cnt1 + odd * cnt0) % mod;

            even = new_even;
            odd = new_odd;
        }

        res = (res + odd * (1LL << b)) % mod;
    }

    cout << res << '\n';
    return;
}

signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int t = 1;
    // cin >> t;
    while(t--){
        solve();
    }
    return 0;
}
```

## 复杂度

枚举二进制位大约 `30` 位。

每一位遍历所有 `n` 个区间。

时间复杂度：

$$
O(30n)
$$

空间复杂度：

$$
O(n)
$$

可以通过：

$$
n \le 5 \times 10^5
$$

的数据范围。

## 本题总结

这题的核心是把看似无法枚举的组合求和转成按位贡献统计。

整体链路是：

```text
所有 XOR 值求和
→ 按位拆贡献
→ 固定一位
→ XOR 为 1 等价于 1 的个数为奇数
→ even / odd DP 统计奇偶方案数
→ countOne 周期统计区间内某一位为 1 的数量
```

这题不是难在代码很长，而是难在连续转化比较多。

如果没有形成“XOR 按位拆”的反应，很难入手。

如果没有掌握“奇偶 DP”，固定一位之后也不知道怎么统计。

如果 `countOne` 的周期公式不熟，最后实现又容易错。

所以这题需要二刷，重点不是背代码，而是把三层转化独立推出。

## 二刷提醒

二刷时按这个顺序重新推：

```text
1. 总 XOR 和为什么可以按位拆？
2. 固定第 b 位后，什么情况下 XOR 结果为 1？
3. 为什么这等价于选到 1 的个数为奇数？
4. even / odd DP 的状态和转移怎么写？
5. 如何计算 [l, r] 中第 b 位为 1 的数有多少个？
6. countOne(x, b) 里的 half、cycle、full、rem 分别是什么？
7. 为什么 cnt0 = len - cnt1？
8. 哪些地方需要取模？
```

其中最需要手写巩固的是：

```cpp
ll count1(ll x,ll b){
    if(x < 0) return 0;

    x++;
    ll half = 1LL << b;
    ll cycle = 1LL << (b + 1);
    ll full = x / cycle;
    ll rem = x % cycle;

    return full * half + max(0LL, rem - half);
}
```

以及：

```cpp
ll new_even = (even * cnt0 + odd * cnt1) % mod;
ll new_odd = (even * cnt1 + odd * cnt0) % mod;
```

如果二刷时能不看笔记独立写出这两段，这题就基本掌握了。


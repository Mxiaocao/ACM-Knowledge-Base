# C. Cool P #1

## 基本信息

来源：ZJU Summer 2026 Contest 2
难度：C
状态：回炉后补出严格实现，待提交验证
当前掌握程度：核心数学转化基本理解，严格实现已能跟写，线性基仍需二刷巩固
比赛日期：2026/7/7
初次复盘日期：2026/7/7
实现回炉日期：2026/7/8
入库日期：2026/7/8
题目链接：https://codeforces.com/gym/702920/problem/C
正式文件名：`C. Cool P No.1.md`
是否需要二刷：是
二刷状态：未二刷

## 状态变化

```text
2026/7/7：回炉中（偏实现回炉）
2026/7/8：回炉后补出严格实现，待提交验证
```

说明：本题 2026/7/7 初次复盘时仍是回炉中，主要卡在严格实现；2026/7/8 隔天已经补出严格版实现，但目前还未正式提交验证，所以不能写成“已 AC”。

## 题意简述

给定一个长度为 `n` 的正整数序列：

```text
a1, a2, ..., an
```

对于每个前缀 `1...k`，统计这个前缀中有多少个下标子集 `S`，满足：

```text
P(S) = 所有选中元素的乘积
```

是完全平方数。

空集允许，空集乘积为 `1`，也是完全平方数。

需要对每个前缀输出答案，答案对：

```text
998244353
```

取模。

## 一开始的状态

一开始看到这题时，完全没有直接思路。

主要卡在：

```text
怎么统计所有子集的乘积是不是完全平方？
```

直接枚举子集肯定不可能，因为前缀长度为 `k` 时，子集数量是：

```text
2^k
```

而题目数据范围很大，必须做数学转化。

## 第一阶段：从完全平方数转成质因子奇偶

最先建立的关键认识是：

```text
一个数是完全平方数，当且仅当所有质因子的指数都是偶数。
```

例如：

```text
36 = 2^2 * 3^2
```

是完全平方数。

而：

```text
60 = 2^2 * 3 * 5
```

不是完全平方数，因为 `3` 和 `5` 的指数都是奇数。

因此对每个数，不需要记录完整质因数分解，只需要记录：

```text
哪些质因子出现了奇数次
```

例如：

```text
12 = 2^2 * 3
```

只保留 `3`，所以：

```text
12 -> {3}
```

再如：

```text
60 = 2^2 * 3 * 5
```

只保留 `3, 5`，所以：

```text
60 -> {3, 5}
```

## 第二阶段：子集乘积转成集合 XOR

每个数可以看成一个“奇数质因子集合”。

选一个子集，本质就是把这些集合做 XOR。

这里的 XOR 含义是：

```text
同一个质因子出现两次就抵消
出现奇数次就保留
```

所以：

```text
子集乘积是完全平方数
```

等价于：

```text
选中的这些奇数质因子集合 XOR 后为空集合
```

例如样例：

```text
2 3 6 4 5
```

转化为：

```text
2 -> {2}
3 -> {3}
6 -> {2, 3}
4 -> {}
5 -> {5}
```

其中：

```text
{2} xor {3} xor {2,3} = {}
```

所以子集 `{2, 3, 6}` 的乘积：

```text
2 * 3 * 6 = 36
```

是完全平方数。

## 第三阶段：答案公式 `2^(k-rank)`

题目进一步转化为：

```text
对于每个前缀，问有多少个子集的 XOR 结果为 0。
```

如果前 `k` 个向量的线性无关数量为 `rank`，那么 XOR 为 `0` 的子集数量是：

```text
2^(k-rank)
```

理解方式：

- 前 `k` 个数一共有 `2^k` 个子集；
- 这些向量能产生 `2^rank` 种不同的 XOR 结果；
- 每种 XOR 结果对应的子集数量相同；
- 所以 XOR 为 `0` 的子集数量为：

```text
2^k / 2^rank = 2^(k-rank)
```

因此本题核心变成：

```text
在线维护当前前缀向量的 rank。
```

## 样例一理解

样例：

```text
2 3 6 4 5
```

转化为：

```text
2 -> {2}
3 -> {3}
6 -> {2, 3}
4 -> {}
5 -> {5}
```

前缀过程：

```text
i = 1:
加入 {2}
新信息，rank = 1
答案 = 2^(1-1) = 1

i = 2:
加入 {3}
新信息，rank = 2
答案 = 2^(2-2) = 1

i = 3:
加入 {2,3}
可以由 {2} xor {3} 得到
不是新信息，rank = 2
答案 = 2^(3-2) = 2

i = 4:
加入 {}
空向量，不是新信息，rank = 2
答案 = 2^(4-2) = 4

i = 5:
加入 {5}
新信息，rank = 3
答案 = 2^(5-3) = 4
```

输出：

```text
1 1 2 4 4
```

## 第四阶段：先尝试随机哈希练习版

为了先理解实现框架，先学习了一个更容易写的版本：

```text
给每个质数随机一个 unsigned long long 哈希值。
```

例如：

```text
2 -> h[2]
3 -> h[3]
5 -> h[5]
```

那么：

```text
6 = 2 * 3
```

可以表示为：

```cpp
h[2] ^ h[3]
```

而：

```text
12 = 2^2 * 3
```

因为 `2` 出现偶数次，只保留 `3`，所以表示为：

```cpp
h[3]
```

这样每个数就被压缩成一个 `unsigned long long`，再用 64 位 XOR 线性基维护 rank。

这一阶段主要补了几个实现模块：

```text
1. spf 最小质因子筛
2. 给质数随机哈希
3. get_value(x)：只保留奇数次质因子并 XOR 哈希
4. XOR 线性基 insert
5. rank 增加判断
6. 输出 pow2[i-rank]
```

## 随机哈希阶段学到的内容

### `spf` 的含义

`spf` 是：

```text
smallest prime factor
```

中文是：

```text
最小质因子
```

也就是：

```cpp
spf[x]
```

表示 `x` 的最小质因子。

例如：

```text
spf[12] = 2
spf[45] = 3
spf[17] = 17
```

它的作用是快速分解质因数。

### `h[p]` 的含义

`h[p]` 可以理解成：

```text
质数 p 的随机身份证
```

例如：

```text
h[2] = 一个随机 64 位数
h[3] = 一个随机 64 位数
h[5] = 一个随机 64 位数
```

如果：

```text
60 = 2^2 * 3 * 5
```

只保留 `3` 和 `5`，那么随机哈希版中：

```cpp
get_value(60) = h[3] ^ h[5]
```

### `mt19937_64` 的含义

随机哈希版中使用：

```cpp
mt19937_64 rng(chrono::steady_clock::now().time_since_epoch().count());
```

可以理解为：

```text
创建一个 64 位随机数生成器 rng，用当前时间作为种子。
```

更好记的写法：

```cpp
auto seed = chrono::steady_clock::now().time_since_epoch().count();
mt19937_64 rng(seed);
```

如果为了调试，也可以固定种子：

```cpp
mt19937_64 rng(114514);
```

然后每次：

```cpp
rng()
```

都会生成一个新的 64 位随机数。

### `get_value(x)` 的含义

随机哈希版中：

```cpp
ull get_value(int x)
```

用于把一个数变成它的“奇数质因子集合”的哈希值。

例如：

```text
60 = 2^2 * 3 * 5
```

只保留 `3` 和 `5`，所以：

```cpp
get_value(60) = h[3] ^ h[5]
```

### `insert(x)` 的含义

随机哈希版中：

```cpp
bool insert(ull x)
```

用于判断 `x` 是否是新信息。

返回值含义：

```text
true  -> x 是新信息，rank++
false -> x 能被之前的向量 XOR 出来，rank 不变
```

一开始对 `insert` 不理解，后来通过小二进制例子理解：

```text
001
010
011
```

其中：

```text
011 = 001 xor 010
```

所以插入 `011` 时会被已有 basis 消成 `000`，返回 `false`，表示它不是新信息。

## 随机哈希练习版代码

这一版是为了先理解整体实现框架而写的：

```text
spf 分解质因数
随机哈希表示奇数质因子集合
64 位 XOR 线性基维护 rank
输出 2^(i-rank)
```

但这一版不是严格正解，因为 `unsigned long long` 只有 64 位，线性基最多维护 64 维，真实质因子空间的 rank 可能超过 64。

因此它适合作为“理解版 / 练习版”，不作为最终 AC 版提交。

```cpp
#include <bits/stdc++.h>
#define code using
#define by namespace
#define Mxiaocao std
code by Mxiaocao;

using ll = long long;
using ull = unsigned long long;

const int MAXN = 1e6 + 10;
const ll mod = 998244353;

vector<int> spf(MAXN);
vector<ull> h(MAXN);
ull basis[64];

mt19937_64 rng(chrono::steady_clock::now().time_since_epoch().count());

void build_spf(){
    for(int i = 0;i < MAXN;++i){
        spf[i] = i;
    }

    for(int i = 2;i * i < MAXN;++i){
        if(spf[i] == i){
            for(int j = i * i;j < MAXN;j += i){
                if(spf[j] == j){
                    spf[j] = i;
                }
            }
        }
    }
}

void build_hash(){
    for(int i = 2;i < MAXN;++i){
        if(spf[i] == i){
            h[i] = rng();
        }
    }
}

ull get_value(int x){
    ull res = 0;

    while(x > 1){
        int p = spf[x];
        int cnt = 0;

        while(x % p == 0){
            x /= p;
            cnt++;
        }

        if(cnt % 2 == 1){
            res ^= h[p];
        }
    }

    return res;
}

bool insert(ull x){
    for(int i = 63;i >= 0;--i){
        if(((x >> i) & 1) == 0) continue;

        if(!basis[i]){
            basis[i] = x;
            return true;
        }

        x ^= basis[i];
    }

    return false;
}

void solve(){
    int n;
    cin >> n;

    vector<ll> pow2(n + 1);
    pow2[0] = 1;
    for(int i = 1;i <= n;++i){
        pow2[i] = pow2[i - 1] * 2 % mod;
    }

    int rank = 0;

    for(int i = 1;i <= n;++i){
        int x;
        cin >> x;

        ull val = get_value(x);

        if(insert(val)){
            rank++;
        }

        cout << pow2[i - rank] << ' ';
    }

    cout << '\n';
}

signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    build_spf();
    build_hash();

    int t = 1;
    // cin >> t;
    while(t--){
        solve();
    }

    return 0;
}
```

## 随机哈希练习版的问题

这部分对应知识节点：[[随机哈希]]、[[64 位随机哈希]]、[[unsigned long long]]、[[mt19937_64]]、[[质因子集合哈希]]、[[随机哈希练习版]]、[[哈希碰撞风险]]。

随机哈希版虽然帮助理解了整体框架，但后来发现它不适合作为本题严格 AC 做法。

原因是：

```cpp
ull basis[64];
```

最多只能维护 64 维空间，因此：

```text
rank 最大只能到 64
```

但本题中 `1e6` 以内的质数数量远远超过 64，真实 rank 可能大于 64。

例如如果输入前 65 个不同质数，它们在严格意义下都线性无关，真实 rank 应该到 65。

但 64 位线性基最多只能到 64，因此随机哈希版会在理论上出错。

所以随机哈希版的定位是：

```text
练习实现框架可以，不能作为严格正解。
```

## 第五阶段：转向严格 AC 做法

发现随机哈希版的 rank 上限问题后，改为严格做法。

本题有一个关键性质：

```text
a[i] <= 1e6
```

因此一个数不可能同时包含两个大于 `1000` 的质因子。

因为：

```text
1009 * 1013 > 1e6
```

所以每个数的奇数质因子中：

```text
大于 1000 的质因子最多只有一个。
```

这使得可以把质因子分成两类：

```text
小质因子：<= 1000
大质因子：> 1000
```

## 严格做法的表示方式

`1000` 以内的质数只有 `168` 个，所以小质因子可以用：

```cpp
bitset<168>
```

表示。

大质因子单独用一个整数记录。

因此每个数表示为：

```cpp
struct Node{
    bitset<SMALL> mask;
    int big;
};
```

含义：

```text
mask：小质因子部分
big：大质因子，如果没有则为 0
```

例如：

```text
60 = 2^2 * 3 * 5
```

只保留 `3, 5`，它们都是小质因子，所以：

```text
mask = {3, 5}
big = 0
```

再如：

```text
2018 = 2 * 1009
```

其中 `2` 是小质因子，`1009` 是大质因子，所以：

```text
mask = {2}
big = 1009
```

## 严格版 `get_value(x)`

严格版 `get_value(x)` 不再返回 `ull`，而是返回 `Node`。

逻辑：

```text
分解 x
只看出现奇数次的质因子
如果 p <= 1000，翻转 mask 中对应位置
如果 p > 1000，记录 big = p
```

代码：

```cpp
Node get_value(int x){
    bitset<SMALL> mask;
    int big = 0;

    while(x > 1){
        int p = spf[x];
        int cnt = 0;

        while(x % p == 0){
            x /= p;
            cnt++;
        }

        if(cnt % 2 == 1){
            if(p <= 1000){
                mask.flip(id[p]);
            }else{
                big = p;
            }
        }
    }

    return {mask, big};
}
```

## 严格版线性基设计

严格版维护两部分：

```cpp
bitset<SMALL> small_basis[SMALL];
vector<bitset<SMALL>> big_basis(MAXN);
vector<int> has_big(MAXN, 0);
```

含义：

```text
small_basis：只处理小质因子的线性基
has_big[p]：大质因子 p 是否已经作为主元出现过
big_basis[p]：大质因子 p 对应向量的小质因子部分
```

## 插入小质因子 mask

如果当前向量没有大质因子，即：

```cpp
cur.big == 0
```

那么直接把 `cur.mask` 插入小线性基：

```cpp
bool insert_small(bitset<SMALL> x){
    for(int i = SMALL - 1;i >= 0;--i){
        if(!x[i]) continue;

        if(small_basis[i].none()){
            small_basis[i] = x;
            return true;
        }

        x ^= small_basis[i];
    }

    return false;
}
```

返回含义：

```text
true  -> 新信息，rank++
false -> 旧信息，rank 不变
```

## 插入带大质因子的 Node

严格版插入逻辑分三种情况。

### 情况一：没有大质因子

```cpp
if(cur.big == 0){
    return insert_small(cur.mask);
}
```

直接插入小线性基。

### 情况二：有大质因子，且这个大质因子第一次出现

如果：

```cpp
!has_big[cur.big]
```

说明之前没有任何向量负责这个大质因子。

它一定是新信息。

于是：

```cpp
has_big[cur.big] = 1;
big_basis[cur.big] = cur.mask;
return true;
```

### 情况三：有大质因子，且这个大质因子已经出现过

如果这个大质因子已经作为主元出现过，那么两个相同的大质因子可以抵消。

例如：

```text
2018 = 2 * 1009
3027 = 3 * 1009
```

第一个存下：

```text
big = 1009
mask = {2}
```

第二个也有：

```text
big = 1009
mask = {3}
```

两者异或后：

```text
1009 xor 1009 = 0
```

剩下：

```text
{2} xor {3} = {2,3}
```

于是只剩小质因子 mask，需要插入小线性基：

```cpp
cur.mask ^= big_basis[cur.big];
return insert_small(cur.mask);
```

完整代码：

```cpp
bool insert_node(Node cur){
    if(cur.big == 0){
        return insert_small(cur.mask);
    }

    if(!has_big[cur.big]){
        has_big[cur.big] = 1;
        big_basis[cur.big] = cur.mask;
        return true;
    }

    cur.mask ^= big_basis[cur.big];
    return insert_small(cur.mask);
}
```

## 当前实现中的主要错误与修正

### 错误一：`if(spf[j] = j)`

一开始写成：

```cpp
if(spf[j] = j)
```

这是赋值，不是判断。

应改为：

```cpp
if(spf[j] == j)
```

### 错误二：`build_hash()` 越界

随机哈希版中曾写：

```cpp
for(int i = 2;i <= MAXN;++i)
```

但数组大小为 `MAXN`，合法下标是 `0...MAXN-1`。

应改为：

```cpp
for(int i = 2;i < MAXN;++i)
```

严格版已经不再需要 `build_hash()`。

### 错误三：输出格式

题面要求一行输出 `n` 个整数，用空格分隔。

当前代码曾写成：

```cpp
cout << pow2[i-rank] << '\n';
```

虽然大多数 OJ 把空格和换行都视为普通空白，但更稳的写法是：

```cpp
cout << pow2[i-rank] << ' ';
```

最后统一：

```cpp
cout << '\n';
```

## 最终严格版代码

```cpp
#include <bits/stdc++.h>
#define code using
#define by namespace
#define Mxiaocao std
code by Mxiaocao;

using ll = long long;

const int MAXN = 1e6 + 10;
const ll mod = 998244353;
const int SMALL = 168;

vector<int> spf(MAXN);
vector<int> id(MAXN, -1);

bitset<SMALL> small_basis[SMALL];
vector<bitset<SMALL>> big_basis(MAXN);
vector<int> has_big(MAXN, 0);

struct Node{
    bitset<SMALL> mask;
    int big;
};

void build_spf(){
    for(int i = 0;i < MAXN;++i){
        spf[i] = i;
    }

    for(int i = 2;i * i < MAXN;++i){
        if(spf[i] == i){
            for(int j = i * i;j < MAXN;j += i){
                if(spf[j] == j){
                    spf[j] = i;
                }
            }
        }
    }
}

void build_small_id(){
    int cnt = 0;

    for(int i = 2;i <= 1000;++i){
        if(spf[i] == i){
            id[i] = cnt;
            cnt++;
        }
    }
}

Node get_value(int x){
    bitset<SMALL> mask;
    int big = 0;

    while(x > 1){
        int p = spf[x];
        int cnt = 0;

        while(x % p == 0){
            x /= p;
            cnt++;
        }

        if(cnt % 2 == 1){
            if(p <= 1000){
                mask.flip(id[p]);
            }else{
                big = p;
            }
        }
    }

    return {mask, big};
}

bool insert_small(bitset<SMALL> x){
    for(int i = SMALL - 1;i >= 0;--i){
        if(!x[i]) continue;

        if(small_basis[i].none()){
            small_basis[i] = x;
            return true;
        }

        x ^= small_basis[i];
    }

    return false;
}

bool insert_node(Node cur){
    if(cur.big == 0){
        return insert_small(cur.mask);
    }

    if(!has_big[cur.big]){
        has_big[cur.big] = 1;
        big_basis[cur.big] = cur.mask;
        return true;
    }

    cur.mask ^= big_basis[cur.big];
    return insert_small(cur.mask);
}

void solve(){
    int n;
    cin >> n;

    vector<ll> pow2(n + 1);
    pow2[0] = 1;
    for(int i = 1;i <= n;++i){
        pow2[i] = pow2[i - 1] * 2 % mod;
    }

    int rank = 0;

    for(int i = 1;i <= n;++i){
        int x;
        cin >> x;

        Node cur = get_value(x);

        if(insert_node(cur)){
            rank++;
        }

        cout << pow2[i - rank] << ' ';
    }

    cout << '\n';
}

signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    build_spf();
    build_small_id();

    int t = 1;
    // cin >> t;
    while(t--){
        solve();
    }

    return 0;
}
```

## 复杂度分析

### 预处理

`build_spf()` 复杂度约为：

```text
O(MAXN log log MAXN)
```

其中：

```text
MAXN = 1e6 + 10
```

### 每个数分解

使用 `spf` 分解，每个数分解复杂度较低，接近质因子数量。

### 线性基插入

小质因子维度为 `168`，所以每次插入最多扫：

```text
168
```

位。

总复杂度约为：

```text
O(MAXN log log MAXN + n * 168)
```

可以接受。

## 当前掌握情况

这题现在已经从“完全没思路”推进到了：

```text
1. 理解完全平方数和质因子奇偶的关系；
2. 理解每个数可以转成奇数质因子集合；
3. 理解子集合法等价于集合 XOR 为空；
4. 理解答案为 2^(k-rank)；
5. 写出随机哈希练习版；
6. 发现随机哈希版 rank 只有 64 的理论风险；
7. 改成小质因子 bitset + 大质因子主元的严格版；
8. 写出严格版 get_value / insert_small / insert_node / solve。
```

当前仍需要二刷的点：

```text
1. XOR 线性基 insert 的消元过程；
2. 为什么答案是 2^(k-rank)；
3. 为什么 a[i] <= 1e6 可以推出大于 1000 的质因子最多一个；
4. 为什么相同 big 可以抵消成纯小质因子 mask；
5. 严格版代码中 small_basis 和 big_basis 的职责区别。
```

## 二刷提醒

二刷时建议按这个顺序重新推：

```text
1. 完全平方数 <=> 质因子指数全偶数
2. 每个数只保留奇数次质因子
3. 子集乘积为平方 <=> 这些集合 XOR 为空
4. 前缀答案 = 2^(i-rank)
5. rank 用线性基维护
6. 随机哈希版为什么有 64 维上限
7. 严格版为什么要拆小质因子和大质因子
8. insert_node 的三种情况
```

最关键的一句话：

```text
本题不是在真的统计乘积，而是在统计前缀向量空间里 XOR 为 0 的子集数量。
```

## 知识链接

副链路：随机哈希练习版

[[随机哈希]]
→ [[64 位随机哈希]]
→ [[unsigned long long]]
→ [[mt19937_64]]
→ [[质因子集合哈希]]
→ [[随机哈希练习版]]
→ [[哈希碰撞风险]]
→ [[严格做法替代随机哈希]]

- [[完全平方数判定]]
- [[质因数分解]]
- [[指数奇偶性]]
- [[GF(2) 线性代数]]
- [[XOR 线性基]]
- [[子集异或为 0 的计数]]
- [[核空间大小公式|2^(n-rank)]]
- [[最小质因子筛]]
- [[奇数质因子集合]]
- [[大质因子分离]]
- [[小质因子 bitset 表示]]
- [[大质因子主元]]
- [[相同大质因子抵消]]
- [[bitset]]

# unsigned long long

## 简记

`unsigned long long` 是 C++ 中常用的 64 位无符号整数类型。

## 在 C. Cool P #1 中

[[C. Cool P No.1|C. Cool P #1]] 的随机哈希练习版用 `unsigned long long` 存储质数哈希值和质因子集合哈希。

## 易错点

`unsigned long long` 只有 64 位，因此对应的 XOR 线性基最多只能维护 64 个维度。
> [!info] 原始材料说明
> 本文件保存 AI 于 2026-07-15 生成的原始题单内容。
> 文件用于保留生成时的完整版本，不代表 Codeforces 官方分类或用户个人掌握状态。
> 后续正式训练资源位于 `07 - 训练题单/Codeforces 模型题库/`。

# Codeforces 最近 100 场：Div.2 B/C + Div.3 C/D 模型题库

## 本次扩展结果

- 沿用原来的最近 100 场比赛样本，不向更早比赛继续滚动。
- 原有：Div.2 B 80 题 + Div.3 C 20 题。
- 新增：Div.2 C 80 题 + Div.3 D 20 题。
- 最终题库：200 题。
- 新增交互题：4 题，已单独标记。
- 新增核验：A 70 题，B 29 题，C 1 题。

> A 表示题面与公开标准解法逻辑交叉核验；B 表示核心转化已核但仍建议补题前再读官方 Tutorial；C 表示公开索引不完整，仅为临时占位。

## 总体变化

从 Div.2 B / Div.3 C 上升到 Div.2 C / Div.3 D 后，题目不只是"多一个算法"。更明显的变化是：

1. 动态过程更常被压成树、函数轨道、盆地、值域链或小状态。
2. 数学结论开始要求证明充要性，而不仅是观察奇偶。
3. 构造题多了逆向恢复、输出操作和全局合法性证明。
4. DP 的状态仍不大，但需要主动发现"上一列、一个 gcd、一个区间或一个极值"已经足够。

## 主模型统计

| 主模型                       | 原 100 题 | 新增 100 题 | 合计 |
| ---------------------------- | --------: | ----------: | ---: |
| M3 贪心、排序与配对          |        24 |          22 |   46 |
| M9 小状态 DP、博弈与极值状态 |        12 |          22 |   34 |
| M4 数学性质、数论与几何      |        15 |          13 |   28 |
| M2 构造与逆向设计            |        14 |          12 |   26 |
| M5 前缀、差分与区间压缩      |         9 |           5 |   14 |
| M1 操作闭包与不变量          |         6 |           5 |   11 |
| M10 过程模拟与结构压缩       |         7 |           4 |   11 |
| M6 字符串正规形与有限状态    |         5 |           5 |   10 |
| M7 位运算与线性结构          |         4 |           6 |   10 |
| M8 阈值、均衡与二分          |         4 |           6 |   10 |

## 模型升级说明

### M3 贪心、排序与配对

- **识别信号：** 交换论证、排序、极值配对、边际收益。
- **本批升级点：** 新增环上合并、阈值跨越排序、值差受限交换、区间/图森林贪心。
- **训练时第一问：** 先问：交换两个决策会怎样？最小/最大元素应被放到何时、何处？
- **新增代表题：** CF2237C Duck Surplus（单调栈 / 最小化最终极值）；CF2234C Vessels, Heights and Two Versions (Easy)（环形断点枚举）；CF2229C1 We Be Flipping (Easy Version)（符号翻转 / 前缀贪心）；CF2205C Simons and Posting Blogs（最后出现 / 移到最前）。

### M9 小状态 DP、博弈与极值状态

- **识别信号：** 常数状态 DP、gcd 状态、博弈势能、树上小状态。
- **本批升级点：** 新增 2×n DP、值域博弈、gcd 最短路、块长子序列 DP 和树路径收益。
- **训练时第一问：** 先问：历史信息能否压成上一列、一个 gcd、一个极值或少量胜负状态？
- **新增代表题：** CF2244D Yaroslav and Productivity（符号状态 / 分段 DP）；CF2246C 0mar and Alternating Sums（分组计数 DP）；CF2238C Village Guilds（树上分层 DP）；CF2240C Nim Game Is XOR Game（Nim 异或 / 必胜步计数）。

### M4 数学性质、数论与几何

- **识别信号：** 整除、奇偶、gcd/lcm、质因数、低维计数。
- **本批升级点：** 新增 2-adic 分类、容斥、函数二阶差分、倍数闭包与环面轨道。
- **训练时第一问：** 先问：题面条件能否化成一个等式、不等式、gcd、余数或位数公式？
- **新增代表题：** CF2226C Mental Monumental (Easy)（模运算可达性）；CF2220C Grid L（整式反推尺寸）；CF2217C Grid Covering（环面轨道 / gcd）；CF2204C Spring（周期集合容斥 / lcm）。

### M2 构造与逆向设计

- **识别信号：** 从目标性质逆推排列、树、数值或交互指令。
- **本批升级点：** 新增 MEX 构造、树的路径最小值和、中心扩张网格、交互式通用指令。
- **训练时第一问：** 先问：能否先设计一个更强、但更容易保证的结构？逆向是否更确定？
- **新增代表题：** CF2230C Arrange the Numbers in a Circle（频次块构造）；CF2209C Find the Zero（交互式信息构造）；CF2200D Portal（排列分区 / 旋转构造）；CF2179D Blackslex and Penguin Civilization（前缀 AND 构造）。

### M5 前缀、差分与区间压缩

- **识别信号：** 区间条件变成前缀差、差分或覆盖统计。
- **本批升级点：** 新增 ±1 最短删除段、二维前缀、位置去排名、窗口多重集到模 k 链。
- **训练时第一问：** 先问：区间条件能否写成两个前缀的关系？删除/修改的贡献能否局部化？
- **新增代表题：** CF2211C1 Equal Multisets (Easy Version)（模 k 位置链）；CF2195D Absolute Cinema（绝对值函数二阶差分）；CF2169C Range Operation（区间替换代数化）；CF2145C Monocarp's String（±1 前缀和 / 最短删除段）。

### M1 操作闭包与不变量

- **识别信号：** 反复操作、可达性、等价类、不变量。
- **本批升级点：** 新增了交替差分、相邻 gcd 最小代表元、前缀容量与等差操作消元。
- **训练时第一问：** 先问：所有操作做完后，哪些量永远不变？能否选一个规范代表状态？
- **新增代表题：** CF2241D An Alternative Way（交替差分不变量）；CF2216C Interval Mod（模区间闭包）；CF2210C1 A Simple GCD Problem (Easy Version)（相邻 gcd 约束 / 最小代表元）；CF2128C Leftmost Below（前缀容量 / 可达判定）。

### M10 过程模拟与结构压缩

- **识别信号：** 动态故事先压成链、环、盆地、事件或确定函数轨道。
- **本批升级点：** 新增函数图汇合、盆地树、连续 +1 跳转、资源门槛调度与交互信息设计。
- **训练时第一问：** 先问：真正改变答案的事件有哪些？早期过程是否会被最后一次事件覆盖？
- **新增代表题：** CF2242C Unstable Elements（游程压缩 / 可达长度）；CF2232C1 Seating Arrangement (Easy Version)（约束模拟 / 小规模枚举）；CF2231C Chipmunk Theo and Equality（函数轨道 / 汇合代价）；CF2133C The Nether（交互式 DAG 路径恢复）。

### M6 字符串正规形与有限状态

- **识别信号：** 字符串正规形、括号平衡、少量自动机状态。
- **本批升级点：** 新增双串括号列分配、最长括号子序列极端删除、子序列骨架合并。
- **训练时第一问：** 先问：重复过程有没有唯一正规形？前缀合法性只需记哪些余额/状态？
- **新增代表题：** CF2233C Cost of a Bracket Sequence（括号匹配 / 极端删除）；CF2224C Zhily and Bracket Swapping（双串括号平衡 / 列交换）；CF2227D Palindromex（回文区间扩张 / 哈希）；CF2175C Needle in a Haystack（保留子序列的字典序构造）。

### M7 位运算与线性结构

- **识别信号：** 逐 bit、XOR/AND、GF(2)、镜像位关系。
- **本批升级点：** 新增位容量与进位、镜像位 DP、前缀 AND 存活长度、XOR 最大和构造。
- **训练时第一问：** 先问：每一位是否独立？若不独立，唯一耦合是不是进位、位长或奇偶？
- **新增代表题：** CF2203C Test Generator（位容量 / 进位）；CF2194C Secret message（周期位集交）；CF2189C1 XOR Convenience (Easy Version)（XOR 映射构造）；CF2180C XOR-factorization（XOR 和最大化构造）。

### M8 阈值、均衡与二分

- **识别信号：** 答案阈值单调、可行区间、极值水位。
- **本批升级点：** 新增可行高度区间回溯、值域双指针、一次交换补救、固定最大子段和。
- **训练时第一问：** 先问：给定答案 X 是否容易 check？全部可行状态是否形成区间？
- **新增代表题：** CF2192C All-in-one Gun（时间阈值 / 一次交换）；CF2193C Monster Game（顺序统计 / 二分）；CF2163C Monopati（值域窗口 / 单调路径）；CF2162D Hidden Segment Query（交互式前缀差 / 二分）。

## 新增 100 题明细

|    # | 层级    | 题目                                                         | 主模型                       | 子模型 / 识别信号           | 形态   | 核验 |
| ---: | ------- | ------------------------------------------------------------ | ---------------------------- | --------------------------- | ------ | :--: |
|    1 | Div.3 D | [CF2244D Yaroslav and Productivity](https://codeforces.com/problemset/problem/2244/D) | M9 小状态 DP、博弈与极值状态 | 符号状态 / 分段 DP          | 普通题 |  B   |
|    2 | Div.2 C | [CF2246C 0mar and Alternating Sums](https://codeforces.com/problemset/problem/2246/C) | M9 小状态 DP、博弈与极值状态 | 分组计数 DP                 | 普通题 |  A   |
|    3 | Div.2 C | [CF2242C Unstable Elements](https://codeforces.com/problemset/problem/2242/C) | M10 过程模拟与结构压缩       | 游程压缩 / 可达长度         | 普通题 |  A   |
|    4 | Div.3 D | [CF2241D An Alternative Way](https://codeforces.com/problemset/problem/2241/D) | M1 操作闭包与不变量          | 交替差分不变量              | 普通题 |  A   |
|    5 | Div.2 C | [CF2238C Village Guilds](https://codeforces.com/problemset/problem/2238/C) | M9 小状态 DP、博弈与极值状态 | 树上分层 DP                 | 普通题 |  B   |
|    6 | Div.2 C | [CF2240C Nim Game Is XOR Game](https://codeforces.com/problemset/problem/2240/C) | M9 小状态 DP、博弈与极值状态 | Nim 异或 / 必胜步计数       | 普通题 |  B   |
|    7 | Div.2 C | [CF2237C Duck Surplus](https://codeforces.com/problemset/problem/2237/C) | M3 贪心、排序与配对          | 单调栈 / 最小化最终极值     | 普通题 |  A   |
|    8 | Div.3 D | [CF2236D Brand New Tatar TV Show](https://codeforces.com/problemset/problem/2236/D) | M9 小状态 DP、博弈与极值状态 | 有序取数博弈 / 值域 DP      | 普通题 |  A   |
|    9 | Div.2 C | [CF2233C Cost of a Bracket Sequence](https://codeforces.com/problemset/problem/2233/C) | M6 字符串正规形与有限状态    | 括号匹配 / 极端删除         | 普通题 |  A   |
|   10 | Div.2 C | [CF2234C Vessels, Heights and Two Versions (Easy)](https://codeforces.com/problemset/problem/2234/C) | M3 贪心、排序与配对          | 环形断点枚举                | 普通题 |  B   |
|   11 | Div.2 C | [CF2232C1 Seating Arrangement (Easy Version)](https://codeforces.com/problemset/problem/2232/C1) | M10 过程模拟与结构压缩       | 约束模拟 / 小规模枚举       | 普通题 |  B   |
|   12 | Div.2 C | [CF2229C1 We Be Flipping (Easy Version)](https://codeforces.com/problemset/problem/2229/C1) | M3 贪心、排序与配对          | 符号翻转 / 前缀贪心         | 普通题 |  B   |
|   13 | Div.2 C | [CF2231C Chipmunk Theo and Equality](https://codeforces.com/problemset/problem/2231/C) | M10 过程模拟与结构压缩       | 函数轨道 / 汇合代价         | 普通题 |  A   |
|   14 | Div.2 C | [CF2230C Arrange the Numbers in a Circle](https://codeforces.com/problemset/problem/2230/C) | M2 构造与逆向设计            | 频次块构造                  | 普通题 |  A   |
|   15 | Div.2 C | [CF2228C1 Cirno and Number (Easy Version)](https://codeforces.com/problemset/problem/2228/C1) | M9 小状态 DP、博弈与极值状态 | 十进制数位 DP / 最近合法数  | 普通题 |  A   |
|   16 | Div.2 C | [CF2224C Zhily and Bracket Swapping](https://codeforces.com/problemset/problem/2224/C) | M6 字符串正规形与有限状态    | 双串括号平衡 / 列交换       | 普通题 |  A   |
|   17 | Div.3 D | [CF2227D Palindromex](https://codeforces.com/problemset/problem/2227/D) | M6 字符串正规形与有限状态    | 回文区间扩张 / 哈希         | 普通题 |  B   |
|   18 | Div.2 C | [CF2226C Mental Monumental (Easy)](https://codeforces.com/problemset/problem/2226/C) | M4 数学性质、数论与几何      | 模运算可达性                | 普通题 |  A   |
|   19 | Div.2 C | [CF2222C Median Partition](https://codeforces.com/problemset/problem/2222/C) | M9 小状态 DP、博弈与极值状态 | 中位数符号化 / 划分 DP      | 普通题 |  B   |
|   20 | Div.2 C | [CF2225C Red-Black Pairs](https://codeforces.com/problemset/problem/2225/C) | M9 小状态 DP、博弈与极值状态 | 2×n 网格状态 DP             | 普通题 |  B   |
|   21 | Div.2 C | [CF2220C Grid L](https://codeforces.com/problemset/problem/2220/C) | M4 数学性质、数论与几何      | 整式反推尺寸                | 普通题 |  A   |
|   22 | Div.2 C | [CF2216C Interval Mod](https://codeforces.com/problemset/problem/2216/C) | M1 操作闭包与不变量          | 模区间闭包                  | 普通题 |  B   |
|   23 | Div.2 C | [CF2217C Grid Covering](https://codeforces.com/problemset/problem/2217/C) | M4 数学性质、数论与几何      | 环面轨道 / gcd              | 普通题 |  B   |
|   24 | Div.2 C | [CF2210C1 A Simple GCD Problem (Easy Version)](https://codeforces.com/problemset/problem/2210/C1) | M1 操作闭包与不变量          | 相邻 gcd 约束 / 最小代表元  | 普通题 |  A   |
|   25 | Div.2 C | [CF2211C1 Equal Multisets (Easy Version)](https://codeforces.com/problemset/problem/2211/C1) | M5 前缀、差分与区间压缩      | 模 k 位置链                 | 普通题 |  A   |
|   26 | Div.2 C | [CF2209C Find the Zero](https://codeforces.com/problemset/problem/2209/C) | M2 构造与逆向设计            | 交互式信息构造              | 交互题 |  B   |
|   27 | Div.2 C | [CF2204C Spring](https://codeforces.com/problemset/problem/2204/C) | M4 数学性质、数论与几何      | 周期集合容斥 / lcm          | 普通题 |  A   |
|   28 | Div.2 C | [CF2208C Stamina and Tasks](https://codeforces.com/problemset/problem/2208/C) | M9 小状态 DP、博弈与极值状态 | 倒序标量 DP                 | 普通题 |  A   |
|   29 | Div.2 C | [CF2207C Where's My Water?](https://codeforces.com/problemset/problem/2207/C) | M9 小状态 DP、博弈与极值状态 | 盆地结构 / 笛卡尔树         | 普通题 |  B   |
|   30 | Div.3 D | [CF2200D Portal](https://codeforces.com/problemset/problem/2200/D) | M2 构造与逆向设计            | 排列分区 / 旋转构造         | 普通题 |  B   |
|   31 | Div.2 C | [CF2205C Simons and Posting Blogs](https://codeforces.com/problemset/problem/2205/C) | M3 贪心、排序与配对          | 最后出现 / 移到最前         | 普通题 |  A   |
|   32 | Div.2 C | [CF2203C Test Generator](https://codeforces.com/problemset/problem/2203/C) | M7 位运算与线性结构          | 位容量 / 进位               | 普通题 |  A   |
|   33 | Div.2 C | [CF2202C CF2202C（题名未同步）](https://codeforces.com/problemset/problem/2202/C) | M9 小状态 DP、博弈与极值状态 | 连续 +1 链 / 跳转 DP        | 普通题 |  C   |
|   34 | Div.2 C | [CF2192C All-in-one Gun](https://codeforces.com/problemset/problem/2192/C) | M8 阈值、均衡与二分          | 时间阈值 / 一次交换         | 普通题 |  B   |
|   35 | Div.3 D | [CF2195D Absolute Cinema](https://codeforces.com/problemset/problem/2195/D) | M5 前缀、差分与区间压缩      | 绝对值函数二阶差分          | 普通题 |  A   |
|   36 | Div.2 C | [CF2197C Game with a Fraction](https://codeforces.com/problemset/problem/2197/C) | M9 小状态 DP、博弈与极值状态 | 网格博弈 / 距离奇偶         | 普通题 |  B   |
|   37 | Div.2 C | [CF2194C Secret message](https://codeforces.com/problemset/problem/2194/C) | M7 位运算与线性结构          | 周期位集交                  | 普通题 |  A   |
|   38 | Div.2 C | [CF2188C Restricted Sorting](https://codeforces.com/problemset/problem/2188/C) | M3 贪心、排序与配对          | 受限交换 / 极值中转         | 普通题 |  A   |
|   39 | Div.3 D | [CF2193C Monster Game](https://codeforces.com/problemset/problem/2193/C) | M8 阈值、均衡与二分          | 顺序统计 / 二分             | 普通题 |  A   |
|   40 | Div.2 C | [CF2189C1 XOR Convenience (Easy Version)](https://codeforces.com/problemset/problem/2189/C1) | M7 位运算与线性结构          | XOR 映射构造                | 普通题 |  B   |
|   41 | Div.2 C | [CF2191C Sorting Game](https://codeforces.com/problemset/problem/2191/C) | M9 小状态 DP、博弈与极值状态 | 二进制串博弈 / 逆序势能     | 普通题 |  B   |
|   42 | Div.3 D | [CF2184D Unfair Game](https://codeforces.com/problemset/problem/2184/D) | M9 小状态 DP、博弈与极值状态 | 区间递归计数                | 普通题 |  B   |
|   43 | Div.2 C | [CF2182C Production of Snowmen](https://codeforces.com/problemset/problem/2182/C) | M4 数学性质、数论与几何      | 参数对计数                  | 普通题 |  B   |
|   44 | Div.3 D | [CF2179D Blackslex and Penguin Civilization](https://codeforces.com/problemset/problem/2179/D) | M2 构造与逆向设计            | 前缀 AND 构造               | 普通题 |  B   |
|   45 | Div.2 C | [CF2180C XOR-factorization](https://codeforces.com/problemset/problem/2180/C) | M7 位运算与线性结构          | XOR 和最大化构造            | 普通题 |  A   |
|   46 | Div.2 C | [CF2176C Odd Process](https://codeforces.com/problemset/problem/2176/C) | M3 贪心、排序与配对          | 奇偶资源分组                | 普通题 |  A   |
|   47 | Div.2 C | [CF2175C Needle in a Haystack](https://codeforces.com/problemset/problem/2175/C) | M6 字符串正规形与有限状态    | 保留子序列的字典序构造      | 普通题 |  A   |
|   48 | Div.2 C | [CF2173C Kanade's Perfect Multiples](https://codeforces.com/problemset/problem/2173/C) | M4 数学性质、数论与几何      | 倍数闭包 / 筛法             | 普通题 |  A   |
|   49 | Div.2 C | [CF2158C Annoying Game](https://codeforces.com/problemset/problem/2158/C) | M9 小状态 DP、博弈与极值状态 | 交替和 / 一次修改           | 普通题 |  A   |
|   50 | Div.2 C | [CF2170C Quotient and Remainder](https://codeforces.com/problemset/problem/2170/C) | M3 贪心、排序与配对          | 商余数匹配 / 阈值双指针     | 普通题 |  A   |
|   51 | Div.2 C | [CF2157C Meximum Array 2](https://codeforces.com/problemset/problem/2157/C) | M2 构造与逆向设计            | MEX 约束构造                | 普通题 |  A   |
|   52 | Div.3 D | [CF2171D Rae Taylor and Trees (Easy Version)](https://codeforces.com/problemset/problem/2171/D) | M4 数学性质、数论与几何      | 树结构计数                  | 普通题 |  B   |
|   53 | Div.2 C | [CF2166C Cyclic Merging](https://codeforces.com/problemset/problem/2166/C) | M3 贪心、排序与配对          | 环上合并 / DSU 贪心         | 普通题 |  A   |
|   54 | Div.2 C | [CF2169C Range Operation](https://codeforces.com/problemset/problem/2169/C) | M5 前缀、差分与区间压缩      | 区间替换代数化              | 普通题 |  A   |
|   55 | Div.2 C | [CF2163C Monopati](https://codeforces.com/problemset/problem/2163/C) | M8 阈值、均衡与二分          | 值域窗口 / 单调路径         | 普通题 |  A   |
|   56 | Div.2 C | [CF2164C Dungeon](https://codeforces.com/problemset/problem/2164/C) | M3 贪心、排序与配对          | 资源门槛 / 优先队列         | 普通题 |  B   |
|   57 | Div.2 C | [CF2161C Loyalty](https://codeforces.com/problemset/problem/2161/C) | M3 贪心、排序与配对          | 跨阈值排序 / 双指针         | 普通题 |  A   |
|   58 | Div.2 C | [CF2156C Maximum GCD on Whiteboard](https://codeforces.com/problemset/problem/2156/C) | M4 数学性质、数论与几何      | 倍数可修复性 / 下降枚举 gcd | 普通题 |  A   |
|   59 | Div.2 C | [CF2154C1 No Cost Too Great (Easy Version)](https://codeforces.com/problemset/problem/2154/C1) | M4 数学性质、数论与几何      | 相邻整数质因数 / 0-1-2 结论 | 普通题 |  A   |
|   60 | Div.3 D | [CF2162D Hidden Segment Query](https://codeforces.com/problemset/problem/2162/D) | M8 阈值、均衡与二分          | 交互式前缀差 / 二分         | 交互题 |  B   |
|   61 | Div.2 C | [CF2160C Reverse XOR](https://codeforces.com/problemset/problem/2160/C) | M7 位运算与线性结构          | 镜像位 DP                   | 普通题 |  A   |
|   62 | Div.2 C | [CF2153C Symmetrical Polygons](https://codeforces.com/problemset/problem/2153/C) | M3 贪心、排序与配对          | 成对边长 / 多边形不等式     | 普通题 |  A   |
|   63 | Div.2 C | [CF2145C Monocarp's String](https://codeforces.com/problemset/problem/2145/C) | M5 前缀、差分与区间压缩      | ±1 前缀和 / 最短删除段      | 普通题 |  A   |
|   64 | Div.2 C | [CF2155C Ancient Wizards' Capes](https://codeforces.com/problemset/problem/2155/C) | M9 小状态 DP、博弈与极值状态 | 二选一递推 / 枚举首态       | 普通题 |  A   |
|   65 | Div.2 C | [CF2152C Triple Removal](https://codeforces.com/problemset/problem/2152/C) | M9 小状态 DP、博弈与极值状态 | 区间小状态 / 模 3           | 普通题 |  B   |
|   66 | Div.3 D | [CF2149D A and B](https://codeforces.com/problemset/problem/2149/D) | M3 贪心、排序与配对          | 中位数 / 位置去偏移         | 普通题 |  A   |
|   67 | Div.2 C | [CF2151C Incremental Stay](https://codeforces.com/problemset/problem/2151/C) | M3 贪心、排序与配对          | 事件配对 / 边际收益排序     | 普通题 |  B   |
|   68 | Div.2 C | [CF2146C Permutation and Binary Search](https://codeforces.com/problemset/problem/2146/C) | M2 构造与逆向设计            | 固定点 + 反转零块           | 普通题 |  A   |
|   69 | Div.2 C | [CF2147C Rabbits](https://codeforces.com/problemset/problem/2147/C) | M6 字符串正规形与有限状态    | 零段方向 DP                 | 普通题 |  A   |
|   70 | Div.2 C | [CF2143C Max Tree](https://codeforces.com/problemset/problem/2143/C) | M2 构造与逆向设计            | 边定向 / 拓扑构造           | 普通题 |  A   |
|   71 | Div.2 C | [CF2144C Non-Descending Arrays](https://codeforces.com/problemset/problem/2144/C) | M9 小状态 DP、博弈与极值状态 | 交换/不交换 2 状态 DP       | 普通题 |  A   |
|   72 | Div.3 D | [CF2140D A Cruel Segment's Thesis](https://codeforces.com/problemset/problem/2140/D) | M3 贪心、排序与配对          | 线段极值配对                | 普通题 |  A   |
|   73 | Div.2 C | [CF2139C Cake Assignment](https://codeforces.com/problemset/problem/2139/C) | M2 构造与逆向设计            | 逆向欧几里得式构造          | 普通题 |  A   |
|   74 | Div.2 C | [CF2137C Maximum Even Sum](https://codeforces.com/problemset/problem/2137/C) | M4 数学性质、数论与几何      | 因子转移 / 2-adic 分类      | 普通题 |  A   |
|   75 | Div.2 C | [CF2136C Against the Difference](https://codeforces.com/problemset/problem/2136/C) | M9 小状态 DP、博弈与极值状态 | 值即块长 / 子序列 DP        | 普通题 |  A   |
|   76 | Div.2 C | [CF2133C The Nether](https://codeforces.com/problemset/problem/2133/C) | M10 过程模拟与结构压缩       | 交互式 DAG 路径恢复         | 交互题 |  B   |
|   77 | Div.3 D | [CF2132D From 1 to Infinity](https://codeforces.com/problemset/problem/2132/D) | M4 数学性质、数论与几何      | 数字块计数 / 位数前缀       | 普通题 |  A   |
|   78 | Div.3 D | [CF2131D Arboris Contractio](https://codeforces.com/problemset/problem/2131/D) | M9 小状态 DP、博弈与极值状态 | 树路径压缩 / 结构 DP        | 普通题 |  B   |
|   79 | Div.2 C | [CF2127C Trip Shopping](https://codeforces.com/problemset/problem/2127/C) | M3 贪心、排序与配对          | 区间重叠 / 最小间隙         | 普通题 |  A   |
|   80 | Div.2 C | [CF2130C Double Perspective](https://codeforces.com/problemset/problem/2130/C) | M3 贪心、排序与配对          | 区间并长度 / 去环保森林     | 普通题 |  A   |
|   81 | Div.2 C | [CF2128C Leftmost Below](https://codeforces.com/problemset/problem/2128/C) | M1 操作闭包与不变量          | 前缀容量 / 可达判定         | 普通题 |  B   |
|   82 | Div.2 C | [CF2125C Count Good Numbers](https://codeforces.com/problemset/problem/2125/C) | M4 数学性质、数论与几何      | 质数容斥                    | 普通题 |  A   |
|   83 | Div.2 C | [CF2122C Manhattan Pairs](https://codeforces.com/problemset/problem/2122/C) | M3 贪心、排序与配对          | 二维极值配对                | 普通题 |  A   |
|   84 | Div.3 D | [CF2126D This Is the Last Time](https://codeforces.com/problemset/problem/2126/D) | M3 贪心、排序与配对          | 可达区间 / 动态选最大终点   | 普通题 |  A   |
|   85 | Div.2 C | [CF2124C Subset Multiplication](https://codeforces.com/problemset/problem/2124/C) | M4 数学性质、数论与几何      | gcd/lcm 链反推因子          | 普通题 |  A   |
|   86 | Div.2 C | [CF2119C A Good Problem](https://codeforces.com/problemset/problem/2119/C) | M7 位运算与线性结构          | AND 约束构造                | 普通题 |  A   |
|   87 | Div.3 D | [CF2123D Binary String Battle](https://codeforces.com/problemset/problem/2123/D) | M9 小状态 DP、博弈与极值状态 | 删改博弈 / 覆盖长度         | 普通题 |  A   |
|   88 | Div.2 C | [CF2112C Coloring Game](https://codeforces.com/problemset/problem/2112/C) | M3 贪心、排序与配对          | 排序三元组计数              | 普通题 |  A   |
|   89 | Div.2 C | [CF2120C Divine Tree](https://codeforces.com/problemset/problem/2120/C) | M2 构造与逆向设计            | 路径最小值和构造            | 普通题 |  A   |
|   90 | Div.3 D | [CF2121D 1709](https://codeforces.com/problemset/problem/2121/D) | M2 构造与逆向设计            | 双行冒泡构造                | 普通题 |  A   |
|   91 | Div.2 C | [CF2113C Smilo and Minecraft](https://codeforces.com/problemset/problem/2113/C) | M5 前缀、差分与区间压缩      | 二维前缀 / 边框损失         | 普通题 |  A   |
|   92 | Div.2 C | [CF2118C Make It Beautiful](https://codeforces.com/problemset/problem/2118/C) | M3 贪心、排序与配对          | lowbit 边际成本 / 最小堆    | 普通题 |  A   |
|   93 | Div.3 D | [CF2117D Retaliation](https://codeforces.com/problemset/problem/2117/D) | M1 操作闭包与不变量          | 等差操作 / 差分消元         | 普通题 |  A   |
|   94 | Div.2 C | [CF2111C Equal Values](https://codeforces.com/problemset/problem/2111/C) | M3 贪心、排序与配对          | 保留最长同值段              | 普通题 |  A   |
|   95 | Div.2 C | [CF2116C Gellyfish and Flaming Peony](https://codeforces.com/problemset/problem/2116/C) | M9 小状态 DP、博弈与极值状态 | gcd 状态最短路 DP           | 普通题 |  A   |
|   96 | Div.3 D | [CF2114D Come a Little Closer](https://codeforces.com/problemset/problem/2114/D) | M3 贪心、排序与配对          | 删除一个点后的包围盒        | 普通题 |  A   |
|   97 | Div.2 C | [CF2110C Racing](https://codeforces.com/problemset/problem/2110/C) | M8 阈值、均衡与二分          | 可行高度区间 / 回溯构造     | 普通题 |  A   |
|   98 | Div.2 C | [CF2109C1 Hacking Numbers (Easy Version)](https://codeforces.com/problemset/problem/2109/C1) | M2 构造与逆向设计            | 交互式数值归一化            | 交互题 |  A   |
|   99 | Div.2 C | [CF2102C Mex in the Grid](https://codeforces.com/problemset/problem/2102/C) | M2 构造与逆向设计            | 中心扩张排列构造            | 普通题 |  A   |
|  100 | Div.2 C | [CF2107C Maximum Subarray Sum](https://codeforces.com/problemset/problem/2107/C) | M8 阈值、均衡与二分          | 固定最大子段和 / 单点调节   | 普通题 |  A   |

## 使用方法

不要按比赛顺序从第 1 题刷到第 100 题。先在 Excel 的"新增题分析"中筛选同一个子模型，每组做 3–5 题：第一题看提示建立模板，第二题只看模型名，第三题开始完全独立。做完后比较"变换点 / 创新点"，确认自己学到的是可迁移结构，而不是记住某道题的结论。

交互题建议单独安排，不计入普通补题完成率。CF2202C 当前公开题目索引未完整同步，已经用 C 级核验标记，后续应在官方页面恢复后重新核对题名、题面与模型。

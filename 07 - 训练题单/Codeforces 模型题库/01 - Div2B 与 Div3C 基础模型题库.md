> [!warning] 使用说明
> 本题库由 AI 根据指定 Codeforces 比赛样本整理，用于训练规划，不是官方题解合集。
>
> 每道题正式训练前，应再次核对 Codeforces 题名、题目链接、题面、数据范围和官方 Tutorial。
>
> 模型分类表示本题最值得迁移的主要转化，不代表该题只有一种解法，也不代表与官方 tags 完全一致。
>
> 本文件中的 A、B、C 表示资料核验等级，不表示用户的个人掌握程度。

# Div.2 B 与 Div.3 C 基础模型题库

## 题库定位

- 题量：100 题；
- Div.2 B：80 题；
- Div.3 C：20 题；
- 当前优先级：最高；
- 适合目标：稳定 Div.2 B 与 Div.3 C，强化 1000—1300 分段的模型识别；
- 正式做题状态不在本文件批量维护；
- 当前正在训练的少量题目统一进入 [[03 - 当前训练队列]]；
- 真正做过的题目再根据状态进入每日复盘、单题复盘和二刷清单。

## 状态流转规则

```text
完整题库
→ 选入当前训练队列
→ 实际做题
→ 记录每日复盘
→ 根据真实状态决定是否创建单题复盘
→ 提示后 / 看题解后 / 未补懂的题进入二刷清单
```

---

## 样本口径

- 时间范围：2025-05-05 至 2026-07-14。
- 最近 100 场已结束且明确属于 Div.2 / Rated for Div.2 / Div.1+2 / Div.3 的正式轮次。
- Div.2（含 Educational 与联合场）取 B；Div.3 取 C；官方为 C1 的 Easy Version保留 C1。
- 共 100 题：Div.2 B 80 题，Div.3 C 20 题。
- 分类依据是"核心转化后的决策对象"，不是机械照搬官方 tags。

## 总体结论

- **M3 贪心、排序与配对：24 题（24%）**。先证明交换论证或单调优先级，再排序、双端配对、选极值或逐步匹配。
- **M4 数学性质、数论与几何：15 题（15%）**。将题意压成整除、质因数、奇偶、距离、对数或低维几何判定。
- **M2 构造与逆向设计：14 题（14%）**。不正面模拟答案，而是从目标性质反推前缀、排列、符号块或局部万能结构。
- **M9 小状态 DP、博弈与极值状态：12 题（12%）**。状态空间很小，或博弈可压为距离势能、两端极值、有限 DP。
- **M5 前缀、差分与区间压缩：9 题（9%）**。把区间/前缀条件转成差分、计数平衡、记录点或覆盖统计。
- **M10 过程模拟与结构压缩：7 题（7%）**。题面是动态过程，但先找记录点、零段、循环/链等静态结构再模拟。
- **M1 操作闭包与不变量：6 题（6%）**。把反复操作看成可达关系，寻找余数类、奇偶、连通分量、正规形等不会被改变的量。
- **M6 字符串正规形与有限状态：5 题（5%）**。通过消除、失配段、括号前缀或少量自动机状态压缩字符串过程。
- **M7 位运算与线性结构：4 题（4%）**。逐位独立、XOR 前缀、GF(2) 约束或最高位结构。
- **M8 阈值、均衡与二分：4 题（4%）**。答案由阈值、平均、最大缺口或单调 check 决定，常配合二分/水位思想。

真正高频的不是某个算法名，而是三种转换能力：**把动态过程压成静态量、把操作闭包压成不变量、把全局最优压成排序后的局部决策**。

## 按模型分组逐题分析

### M3 贪心、排序与配对（24 题）

**核心识别：** 先证明交换论证或单调优先级，再排序、双端配对、选极值或逐步匹配。

**常见变换：** 延后大值、相邻配对、最小券配最贵组、逐级爬升。

**做题第一问：** 交换两个决策会不会更优？应把最大/最小放在什么时刻或位置？

| 比赛题 | 题目 | 核心转化 | 变换点 / 创新点 | 处理方法 |
| --- | --- | --- | --- | --- |
| [CF2237B](https://codeforces.com/problemset/problem/2237/B) | Annoying the Ghost | 加值后允许相邻交换，目标转为满足支配关系的最小逆序匹配。 | 值的可行性与交换代价被拆开：先匹配，再统计移动距离。 | 排序/数据结构选当前可匹配位置，按最早或最优位置贪心配对并累计逆序。 |
| [CF2236C](https://codeforces.com/problemset/problem/2236/C) | Omsk Programmers | "加一"和"整除"可用交换论证：整除越早做越不劣。 | 混合操作顺序被标准化为"先除后加"，只需枚举除法祖先链。 | 枚举两个数沿 floor-divide 的祖先状态，计算到共同/目标状态的最小代价。 |
| [CF2229B](https://codeforces.com/problemset/problem/2229/B) | Absolute Cinema | 每对可交换，目标可拆成固定的 Σmax(ai,bi) 加一个受控极值项。 | 局部交换只影响谁承担全局最大/最小角色。 | 每对取 max 进入基础和，再用所有 min 中的最大值修正。 |
| [CF2224B](https://codeforces.com/problemset/problem/2224/B) | Zhily and Mex and Max | 最大值应尽早固定为前缀 max；其余不同的小值应尽早推进 MEX。 | 重复值对 MEX 无贡献，应延后；最大值与 MEX 两条贡献链需分开算。 | 排序去重，最大值放首位，按 0,1,... 推进 MEX，剩余重复值放尾部并公式求和。 |
| [CF2227C](https://codeforces.com/problemset/problem/2227/C) | Snowfall | 只关心元素含因子 2/3 的掩码，乘积被 6 整除由相邻类别决定。 | 具体数值被压成四类；排列目标变为隔离危险配对。 | 按掩码分类计数，构造合适块顺序/交错，计算最少坏子数组。 |
| [CF2210B](https://codeforces.com/problemset/problem/2210/B) | Simply Sitting on Chairs | 扫描排列时，当前位置可安全放置当且仅当 p_i≤i。 | 局部条件其实编码了所有前驱依赖，无需回溯。 | 从左到右按条件贪心选择/计数，并维护已满足位置。 |
| [CF2203B](https://codeforces.com/problemset/problem/2203/B) | Beautiful Numbers | F(F(x))=F(x) 等价于十进制数位和不超过 9。 | 修改数字问题变为把数位和降到阈值，且前导位有特殊成本。 | 从高贡献数位贪心修改，计算减少数位和的最小次数并处理首位。 |
| [CF2194B](https://codeforces.com/problemset/problem/2194/B) | Offshores | 每捐出 x 只能让接收者增加 y，损失固定为 x-y，余数决定保留价值。 | 要选择一个最终接收账户，不能简单把所有可转移量相加而忽略其余数。 | 预计算每个账户可贡献 floor(ai/x)*y，枚举/挑选保留余数最优的接收者。 |
| [CF2189B](https://codeforces.com/problemset/problem/2189/B) | The Curse of the Frog | 每种跳跃前 b_i-1 次免费推进，之后每次净推进固定。 | 把"周期退后"摊销成一次性免费距离+可重复的最大净收益。 | 先累计所有免费段；剩余距离用最大正净收益整除向上取整，无正收益则 -1。 |
| [CF2180B](https://codeforces.com/problemset/problem/2180/B) | Ashmal | 每步把新串放前或后，局部选择可直接比较 ai+s 与 s+ai。 | 通常字符串拼接排序是全局的；这里输入顺序固定，但每步二选一可贪心。 | 逐步比较两种拼接结果，保留字典序较小者。 |
| [CF2158B](https://codeforces.com/problemset/problem/2158/B) | Split | 奇频值天然贡献两边的奇数种类；偶频值可成对分配并受子序列长度奇偶约束。 | 目标不是均分总和，而是最大化两边奇频种类数。 | 统计奇/偶频值，先分奇频，再用偶频补齐两侧容量与奇偶，公式计算。 |
| [CF2154B](https://codeforces.com/problemset/problem/2154/B) | Make it Zigzag | 免费操作可把偶数位抬到前缀最大，因此付费只发生在需要降低的谷位。 | 先把"免费可做什么"吃满，再局部计算每个奇数位最低下降量。 | 预处理前缀最大值替代峰位，对每个谷位累加 max(0,ai-min(neighbors)+1)。 |
| [CF2146B](https://codeforces.com/problemset/problem/2146/B) | Merging the Sets | 至少三种覆盖方案可由"全集已覆盖且至少两套方案可删除一个集合仍覆盖"刻画。 | 不计数所有子集，只找具有冗余覆盖的集合。 | 统计每个元素出现次数；对每个集合检查其元素是否全有其他覆盖，计数可移除集合。 |
| [CF2143B](https://codeforces.com/problemset/problem/2143/B) | Discounts | 券面 x 占用 x 件但只免其中最便宜一件；小 x 的券更高效。 | 为了让免费金额尽量大，应让最小券覆盖当前最贵的一组。 | 商品升序、券升序；从最贵端依次用能放下的最小券，利用前缀和算支付额。 |
| [CF2139B](https://codeforces.com/problemset/problem/2139/B) | Cake Collection | 选中的烤箱只在指定收集时刻贡献，较大速率应分配更晚时刻。 | 重排不等式决定排序方向，且只需取 top min(n,m)。 | 速率降序取前 q 个，给第 i 大分配权重 m-i+1，求加权和。 |
| [CF2133B](https://codeforces.com/problemset/problem/2133/B) | Villagers | 连接两人花费 max，较小值变 0，等价于把非零值两两配对支付较大者。 | 最优是相邻配对，避免两个大值分别付费。 | 降序排序，每两项一组累加第 1、3、5… 项。 |
| [CF2128B](https://codeforces.com/problemset/problem/2128/B) | Deque Process | 要避免连续 5 个单调，可通过周期性交替制造局部峰谷。 | 不需要预测长远，只要每两步按相反比较方向取端点。 | 从双端取数，按奇偶轮次选择较小/较大端，形成锯齿序列。 |
| [CF2122B](https://codeforces.com/problemset/problem/2122/B) | Pile Shuffling | 每堆是上 0 下 1，目标也同形；移动多余 0 还会暴露需要移动的 1。 | 0 的变化与 1 的变化不是简单独立相加，要计暴露成本。 | 逐堆比较初末 0/1 数，计算必须移出的零以及因此额外移出的一个数量。 |
| [CF2126C](https://codeforces.com/problemset/problem/2126/C) | I Will Definitely Make It | 水位上涨时传送到更高塔，最安全的选择是按高度从小到大逐级爬。 | 选择更高并不总优；跳幅过大会在传送途中淹没。 | 排序所有不低于起点的高度，贪心检查相邻增量是否≤当前安全余量。 |
| [CF2124B](https://codeforces.com/problemset/problem/2124/B) | Minimise Sum | 一次把后项变 0 后，后续前缀最小值贡献全部归零；最优操作只需考虑极早位置。 | 全局 n 项目标实际只与前两个/最早前缀最小有关。 | 计算不操作值与把第 2 项并入第 1 项等少量候选，取最小。 |
| [CF2117C](https://codeforces.com/problemset/problem/2117/C) | Cool Partition | 下一段必须包含上一段的所有不同值，可视为一组待偿还义务。 | 贪心切段时不仅要当前义务已出现，还要确保当前段值在未来仍有出现。 | 预处理每值最后位置；扫描维护 required/seen 与当前段最小最后位置，能安全时立即切。 |
| [CF2116B](https://codeforces.com/problemset/problem/2116/B) | Gellyfish and Baby's Breath | 最大化 2^{p_j}+2^{q_{i-j}} 时，先比较更大的指数，再比较另一个指数。 | 取模后的数不能直接用于大小比较；每个前缀只需考虑 p 前缀最大与 q 前缀最大产生的两个候选。 | 维护两排列的前缀最大位置，对每个 i 比较两个指数有序对，再模运算输出。 |
| [CF2114C](https://codeforces.com/problemset/problem/2114/C) | Need More Arrays | 新数组要求相邻选择值至少差 2，等价于选最大数量子序列。 | 排序已给出时，选最早可行值为后续留下最大空间。 | 从左到右贪心，若 ai>last+1 就开启新组/选择它。 |
| [CF2102B](https://codeforces.com/problemset/problem/2102/B) | The Picky Cat | 每个数可取 ±，相对 abs(a1) 更小的元素无论怎样都限制其中位数位置。 | 符号自由度使具体正负无关，只数绝对值小于 abs(a1) 的个数。 | 统计 smaller；若 smaller≤floor(n/2) 则可调整符号使 ±a1 为中位数。 |

### M4 数学性质、数论与几何（15 题）

**核心识别：** 将题意压成整除、质因数、奇偶、距离、对数或低维几何判定。

**做题第一问：** 能否把条件化简为 gcd、余数、奇偶、距离或对数？

| 比赛题 | 题目 | 核心转化 | 变换点 / 创新点 | 处理方法 |
| --- | --- | --- | --- | --- |
| [CF2238B](https://codeforces.com/problemset/problem/2238/B) | Crimson Triples | 恒等式把 gcd(lcm(a,b),lcm(b,c))=gcd(a,c) 化为 b 同时整除 a、c。 | 三元组条件看似复杂，消去 lcm 后只需按中间数统计倍数个数。 | 枚举 b，令 cnt=floor(n/b)，贡献为 cnt²，再求和。 |
| [CF2234B](https://codeforces.com/problemset/problem/2234/B) | Palindrome, Twelve and Two Terms | 要求 n=回文数+12 的倍数，只需研究 n 模 12 的小余数代表。 | 大范围 n 被压成有限余数表；仅极小数需特判。 | 预处理每个余数可用的回文补数，通常取个位回文，处理 10 等边界。 |
| [CF2226B](https://codeforces.com/problemset/problem/2226/B) | Everything Everywhere | 互异数组中长度≥3 的区间无法满足 max-min=gcd 的强等式。 | 全体区间骤降为只检查单点/相邻对。 | 证明中间元素造成矛盾，线性扫描相邻元素判定贡献。 |
| [CF2205B](https://codeforces.com/problemset/problem/2205/B) | Simons and Cakes for Success | n 整除 k^n 的最小 k 在每个质因子上只需至少出现一次。 | 指数 n 自动放大质因子次数，答案退化为 rad(n)。 | 质因数分解 n，乘所有不同质因子。 |
| [CF2179C](https://codeforces.com/problemset/problem/2179/C) | Blackslex and Number Theory | 模操作后的可区分性由最小元素附近的倍数/余数间隔限制。 | 答案通常由最小值和第二临界值决定，而非枚举所有 x。 | 排序，分析哪些 k 会让两数落到同余/相同值，取首个破坏阈值。 |
| [CF2157B](https://codeforces.com/problemset/problem/2157/B) | Expansion Plan 2 | 四邻扩张贡献曼哈顿距离 1，八邻扩张贡献切比雪夫距离 1。 | 二维过程只需同时满足两个距离预算不等式。 | 计算目标点的 Manhattan 与 Chebyshev 距离，检查 d1≤c4+2c8 且 d∞≤c4+c8。 |
| [CF2166B](https://codeforces.com/problemset/problem/2166/B) | Tab Closing | 标签宽度随数量变化，但一次点击能否一直点同坐标只有两个临界条件。 | 连续几何动态被压成答案只可能 1 或 2。 | 检查初始总宽是否不超过可视区或单标签已固定，否则输出 2。 |
| [CF2164B](https://codeforces.com/problemset/problem/2164/B) | Even Modulo Pair | 若一直找不到 y mod x 为偶数，候选数会至少指数增长。 | 利用值域上限，证明只需检查排序后前约 40 个数。 | 排序，暴力检查小前缀内所有对；找到即输出，否则无解。 |
| [CF2149C](https://codeforces.com/problemset/problem/2149/C) | MEX rose | 要让 MEX=k，必须补齐 [0,k-1] 缺失值并消灭所有 k。 | 一次修改可同时承担"移走一个 k"和"补一个缺失值"。 | missing=缺失低值数，cntk=k 的数量，答案 max(missing,cntk)。 |
| [CF2132C1](https://codeforces.com/problemset/problem/2132/C1) | Cunning Seller (Easy Version) | 最少交易次数等于 n 的三进制数位和，交易按 3^x 分块。 | 贪心分块与进制表示完全对应。 | 做三进制分解，按每位次数累加对应交易价格。 |
| [CF2125B](https://codeforces.com/problemset/problem/2125/B) | Left and Down | 一步向量 (dx,dy) 可重复，能一次完成当且仅当约分后的基本向量两坐标≤k。 | 二维移动被 gcd 约分成最小方向向量。 | g=gcd(a,b)，检查 a/g、b/g；满足答案 1，否则 2。 |
| [CF2119B](https://codeforces.com/problemset/problem/2119/B) | Line Segments | 固定长度折线能达到距离 d，当且仅当最长边不超过其余边之和；把 d 也当一条边。 | 坐标方向无关，只剩多边形存在性。 | 求目标距离平方/长度，检查 max(d,max ai)≤总和减该 max，避免浮点可用平方比较。 |
| [CF2120B](https://codeforces.com/problemset/problem/2120/B) | Square Pool | 镜面反射展开后轨迹是直线；到角落只可能沿两条对角线族。 | 不做碰撞模拟，只验初始点与方向是否对齐对应对角线。 | 检查 x=y 且 dx=dy，或 x+y=s 且 dx=-dy。 |
| [CF2113B](https://codeforces.com/problemset/problem/2113/B) | Good Start | 无限延伸铺砖只要求两个已放矩形在 x 相位或 y 相位兼容。 | 边界不限制，相对位移的模 a/b 才是关键。 | 检查 x 坐标差是否为 a 的倍数或 y 坐标差是否为 b 的倍数，并处理同轴重叠。 |
| [CF2111B](https://codeforces.com/problemset/problem/2111/B) | Fibonacci Cubes | 所有斐波那契立方体装箱条件被最大两个边长控制。 | 无需三维装箱搜索；最大方块和次大方块给出必要充分阈值。 | 排序箱子三边，检查最小边≥F_n 且最大边≥F_n+F_{n-1}。 |

### M2 构造与逆向设计（14 题）

**核心识别：** 不正面模拟答案，而是从目标性质反推前缀、排列、符号块或局部万能结构。

**做题第一问：** 能否先把目标性质加强成更容易保证的结构，再反推答案？

| 比赛题 | 题目 | 核心转化 | 变换点 / 创新点 | 处理方法 |
| --- | --- | --- | --- | --- |
| [CF2246B](https://codeforces.com/problemset/problem/2246/B) | ezraft and Array | 要求每个元素整除总和，反推一个共享总和与互异因子结构。 | 不是找数组，而是先设计总和，再让所有项成为它的不同约数。 | 选可控的公倍数/倍数模板生成 n 个互异正数，并直接验证整除条件。 |
| [CF2233B](https://codeforces.com/problemset/problem/2233/B) | Different Distances | 把每个数出现 4 次且间距互异，拆成可拼接的小型局部构造。 | 创新点是找到长度 2、3 的万能 gadget，而不是全局搜索。 | 按 n 对 2/3 分块，用固定模板平移拼接。 |
| [CF2216B](https://codeforces.com/problemset/problem/2216/B) | THU Packing Puzzle | 宽度固定为 3，所有局部摆放状态是有限的。 | 一般铺砖问题在固定宽度下退化为小 gadget/有限状态。 | 枚举少量行状态或使用预制构造块，拼出目标数量。 |
| [CF2211B](https://codeforces.com/problemset/problem/2211/B) | Mickey Mouse Constructive | 等和划分由前缀和相等刻画，±1 块的周期与 abs(x-y) 的因子相关。 | 通过符号块控制前缀和轨迹，避免出现多余等和切点。 | 按差值的整除结构选择块长，交替构造正负段并验证。 |
| [CF2175B](https://codeforces.com/problemset/problem/2175/B) | XOR Array | 区间 XOR 为 0 等价于两个前缀 XOR 相等。 | "恰好一个零区间"转为前缀值除指定一对外全部互异。 | 构造前缀 XOR 序列，使 P[l-1]=P[r] 且其他值不同，再令 ai=P[i-1] xor P[i]。 |
| [CF2163B](https://codeforces.com/problemset/problem/2163/B) | Siga ta Kymata | 索引区间×值区间的矩形操作有通用覆盖模板。 | 绝大多数点可用至多 5 个固定结构覆盖；只有极端四点组合造成无解。 | 检查位置 1,n 及值 1,n 的逆位置等必要障碍，否则输出预制矩形操作。 |
| [CF2161B](https://codeforces.com/problemset/problem/2161/B) | Make Connected | 无三个连续黑格的连通图形被限制为极少的 2×2/相邻对角模板。 | 一般网格连通问题因局部禁型而高度刚性。 | 枚举有限模板，检查原黑格可被包含并补点最少/可行。 |
| [CF2155B](https://codeforces.com/problemset/problem/2155/B) | Abraham's Great Escape | 不逃脱的格子必须落入有向环；除只剩 1 个不逃格外都可用 2 环/蛇形构造。 | 从"让 k 个逃脱"反向构造 n²-k 个环内节点。 | 若 n²-k=1 输出 NO；否则用相邻二环和蛇形路径布置剩余方向。 |
| [CF2147B](https://codeforces.com/problemset/problem/2147/B) | Multiple Construction | 每个数出现两次且距离为其倍数，可利用奇偶位置和对称排列统一满足。 | 避免逐个放置回溯，直接给出全局公式。 | 按奇偶/前后半段输出固定排列，使两次位置差等于 x 或 2x 的倍数。 |
| [CF2144B](https://codeforces.com/problemset/problem/2144/B) | Maximum Cost Permutation | 代价由首个/末个不在正确位置的下标决定，零位可用缺失值扩张这个区间。 | 无需最大化全排列逆序，只需最大化"未排序区间"的跨度。 | 统计缺失值与零位置，确定必错边界，再把缺失值分配给零位以向两侧扩张。 |
| [CF2137B](https://codeforces.com/problemset/problem/2137/B) | Fun Permutation | 令 q_i=n-p_i+1，则每个位置 p_i+q_i 恒为 n+1。 | 把相邻 gcd 条件强化为所有位置的和完全相同。 | 逐项输出补排列 n-p_i+1。 |
| [CF2136B](https://codeforces.com/problemset/problem/2136/B) | Like the Bitset | 标记为 1 的位置不能成为覆盖区间的最大值，因此要把小值放 1 位、大值放 0 位。 | 还需避免连续 k 个全为 1 的无解窗口。 | 先检查最长 1 段<k；再按位置类别分配小/大排列值，保证每个窗口有 0 位压住最大值。 |
| [CF2130B](https://codeforces.com/problemset/problem/2130/B) | Pathless | 从 1 走到 n 的基础和固定，来回走只会增加相邻对之和。 | 把 0、2、1 分块可让增量生成集合为 2x+3y；唯一不可表示的正数是 1。 | 设 d=s-sum；d<0 任意，d=0 或 d≥2 无法阻止，d=1 输出 0…0 2…2 1…1。 |
| [CF2118B](https://codeforces.com/problemset/problem/2118/B) | Make It Permutation | 每行初始 1..n，用至多两次反转可制造循环移位式列排列。 | 不逐列修复，而是给每行固定前缀+后缀反转模板。 | 第 i 行反转适当前缀与后缀，输出 2n-2 个以内操作。 |

### M9 小状态 DP、博弈与极值状态（12 题）

**核心识别：** 状态空间很小，或博弈可压为距离势能、两端极值、有限 DP。

**做题第一问：** 完整状态是否只需保留少量极值/奇偶/端点？博弈是否有势能？

| 比赛题 | 题目 | 核心转化 | 变换点 / 创新点 | 处理方法 |
| --- | --- | --- | --- | --- |
| [CF2228B](https://codeforces.com/problemset/problem/2228/B) | Remilia Plays Soku | 环上追逐博弈可压成双方距离与每回合位移的势能。 | 复杂技能描述最终只剩最短环距离；小 n 单独退化。 | 求顺/逆时针距离，代入回合数公式；n≤3 特判。 |
| [CF2217B](https://codeforces.com/problemset/problem/2217/B) | Flip the Bit Easy | 每次翻转必须覆盖特殊位置，左右两侧需求可独立计数，只由操作总奇偶耦合。 | 两边最大需求之外，还要把总次数调到正确奇偶。 | 计算左右所需次数，取不小于二者的最小合法同奇偶整数。 |
| [CF2202B](https://codeforces.com/problemset/problem/2202/B) | ABAB Construction | 从双端取字符只需记录当前首尾与目标 '?' 的少量状态。 | 指数级双端选择被压成 4 个端点状态。 | 做小状态 DP/自动机，逐步更新可行首尾类型。 |
| [CF2195C](https://codeforces.com/problemset/problem/2195/C) | Dice Roll Sequence | 每一位骰面只有 6 种，合法相邻关系固定。 | 大序列被压为 6 状态最短编辑 DP。 | dp[i][face] 表示前 i 位以 face 结尾的最少修改，枚举前一面。 |
| [CF2184C](https://codeforces.com/problemset/problem/2184/C) | Huge Pile | 每分钟把 x 变成 floor(x/2) 或 ceil(x/2)，状态数仅 O(log n)。 | 看似二叉树爆炸，但大量节点合并为相同整数。 | 记忆化搜索/集合分层，只保存不同 pile 大小，求到 k 的最短层。 |
| [CF2173B](https://codeforces.com/problemset/problem/2173/B) | Niko's Tactical Cards | 两种变换 k-ai 与 bi-k 都是斜率 ±1 的仿射映射。 | 所有可达值的极值只由上一步最小值和最大值产生。 | 每轮只维护 min/max，枚举它们经过两种映射后的 4 个候选。 |
| [CF2171C1](https://codeforces.com/problemset/problem/2171/C1) | Renako Amaori and XOR Game (Easy Version) | 交换 ai,bi 只在二者不同的位置改变两边 XOR。 | 每个有效位置同时翻转两个最终 XOR 的同一位影响，游戏只剩少量奇偶状态。 | 逐位/逐位置维护 XOR 差与轮次归属，按倒序关键位置判胜负。 |
| [CF2169B](https://codeforces.com/problemset/problem/2169/B) | Drifting Away | 每格出边至多 2 条，河流形成有向链/环；星号取两侧最大。 | 不是从每个起点重复走，记忆化后每个格只算一次，并需检测环表示无限。 | DFS 三色标记，'<','>' 单转移，'*' 取两侧最大；遇在栈节点判无限。 |
| [CF2140C](https://codeforces.com/problemset/problem/2140/C) | Ultimate Value | Bob 的交换可被 Alice 反向抵消，因此博弈只剩 Alice 最多一次有效交换的增益。 | 多回合对抗塌缩为一个 pair 的最大改善。 | 把下标奇偶符号并入值，扫描维护前缀极值，求最佳交换增益加基础和。 |
| [CF2127B](https://codeforces.com/problemset/problem/2127/B) | Hamiiid, Haaamid... Hamid? | 建墙与逃跑的对抗只由人物两侧最近墙和边界距离决定。 | 整个 01 串的大部分位置无关，只保留左右最近障碍。 | 求 L、R，比较双方两侧策略的 min/max 组合得到回合数。 |
| [CF2109B](https://codeforces.com/problemset/problem/2109/B) | Slice to Survive | 第一刀可利用怪物初始位置，之后怪物任意移动，每个维度都需 ceil(log2 length)) 次。 | 只有第一回合有位置信息，后续退化为对半切最坏情况。 | 分别枚举第一刀切行或切列，保留 min(a,n-a+1) 或 min(b,m-b+1)，加两维对数次数取最小。 |
| [CF2107B](https://codeforces.com/problemset/problem/2107/B) | Apples in Boxes | 合法首步只需检查最大值减 1 后与次大、最小的差；之后胜负由苹果总数奇偶决定。 | 约束博弈先判断是否存在安全首步，再退化为普通取一枚游戏。 | 求最大、次大、最小与总和；若安全条件失败 Jerry，否则和为奇数 Tom、偶数 Jerry。 |

### M5 前缀、差分与区间压缩（9 题）

**核心识别：** 把区间/前缀条件转成差分、计数平衡、记录点或覆盖统计。

**做题第一问：** 区间条件能否改成两个前缀的关系？真正需要维护的状态有几个？

| 比赛题 | 题目 | 核心转化 | 变换点 / 创新点 | 处理方法 |
| --- | --- | --- | --- | --- |
| [CF2242B](https://codeforces.com/problemset/problem/2242/B) | Predominant Frequency Division | 三段频次不等式可改写成前缀计数之差的正负条件。 | 创新在于把"枚举两个切点"压成扫描时维护能作为第一段结尾的状态。 | 对关键值建立 ±1 平衡前缀；扫描第二切点，维护满足第一组约束的最优/可行状态。 |
| [CF2231B](https://codeforces.com/problemset/problem/2231/B) | Another Sorting Problem | 对差分数组观察后，全局操作次数 k 被最小差分强制确定。 | 无需搜索 k；最紧约束直接钉死候选。 | 构造差分，取候选 k 后完整模拟一次并验证所有位置。 |
| [CF2188B](https://codeforces.com/problemset/problem/2188/B) | Seats | 已有座位把路径切成若干连续空段，每段的最少补点数独立。 | 边界空段与中间空段公式不同，可用哨兵统一。 | 加入虚拟已占点，扫描零段，按每 3 个位置放一个的支配结构求和。 |
| [CF2193C](https://codeforces.com/problemset/problem/2193/C) | Replace and Sum | 可替换值的影响从右向左闭包，得到 c_i=max(a_i,b_i,c_{i+1})。 | 动态选择被预处理成一个确定数组，区间查询只剩前缀和。 | 逆序计算闭包数组 c，再建前缀和回答查询。 |
| [CF2170B](https://codeforces.com/problemset/problem/2170/B) | Addition on a Segment | 区间加一的总贡献与覆盖长度受"非零个数"和总和共同限制。 | 最大区间长度由两个简单上界取最小。 | 统计 z=0 的数量与 S=sum(b)，答案 min(n-z,S-(n-1))（按边界截断）。 |
| [CF2160B](https://codeforces.com/problemset/problem/2160/B) | Distinct Elements | b_i-b_{i-1} 等于 a_i 上次出现距离；新值时等于 i。 | "后缀不同元素总数"反推出一个确定的前驱引用。 | 令 d=b_i-b_{i-1}；d=i 创建新值，否则复制 a_{i-d}，并验证范围。 |
| [CF2123C](https://codeforces.com/problemset/problem/2123/C) | Prefix Min and Suffix Max | 元素可成为最终幸存者当且仅当前缀最小或后缀最大。 | 复杂删除选择被两个方向极值的充要条件替代。 | 预处理 prefix min 与 suffix max，逐位标记。 |
| [CF2112B](https://codeforces.com/problemset/problem/2112/B) | Shrinking Array | 合并区间能得到其 [min,max] 内任意值，故与邻值距离≤1 时一次即可变美。 | 区间操作被压成可达值区间。 | 先查相邻是否已满足；再枚举/扩张区间 min/max 与旁边元素，求最短合并次数。 |
| [CF2121C](https://codeforces.com/problemset/problem/2121/C) | Those Who Are With Us | 一次选行列减一，能把最大值降 1 当且仅当所有最大单元格被一行∪一列覆盖。 | 只需关注全局最大值位置，其他数完全无关。 | 统计各行/列最大值个数及交点，枚举 (r,c) 是否 row+col-intersection=总数。 |

### M10 过程模拟与结构压缩（7 题）

**做题第一问：** 真正改变答案的事件有哪些？能否只保留记录点、零段、链或环？

| 比赛题 | 题目 | 核心转化 | 变换点 / 创新点 | 处理方法 |
| --- | --- | --- | --- | --- |
| [CF2220B](https://codeforces.com/problemset/problem/2220/B) | OIE Excursion | 带时间的守卫过程可压成相同相位的连续段与阻塞长度。 | 不逐秒扩展状态，而是识别真正造成不可通过的长记录段。 | 扫描字符串/时间相位，维护连续相同状态长度并按阈值判定。 |
| [CF2204B](https://codeforces.com/problemset/problem/2204/B) | Right Maximum | 反复删除"最右最大值+后缀"的过程只保留前缀记录最大值。 | 动态删除序列可一次扫描得到最终层数/答案。 | 从右或左维护严格记录点，统计被保留的最大值层。 |
| [CF2208B](https://codeforces.com/problemset/problem/2208/B) | Cyclists | 目标牌在前 k 时立即使用，否则用前 k 中最便宜牌推动队列。 | 不是无脑选全局最小；决策必须服从滑动队首窗口。 | 用队列模拟，窗口内优先目标，否则选最小费用；预算不足停止。 |
| [CF2182B](https://codeforces.com/problemset/problem/2182/B) | New Year Cake | 蛋糕层大小按 2 的幂增长且颜色交替，层数只有对数级。 | 只需尝试两种起始颜色，不需要 DP。 | 分别模拟两种颜色顺序，每层消耗 1,2,4…，取最大层数。 |
| [CF2156B](https://codeforces.com/problemset/problem/2156/B) | Strange Machine | 只要循环中存在一次除 2，x 每轮至少减半。 | 即使 n 大，实际循环轮数只有 O(log x)；全 A 才单独线性结果。 | 若无 B 直接输出；否则按字符串循环模拟到 0。 |
| [CF2145B](https://codeforces.com/problemset/problem/2145/B) | Deck of Cards | 确定删顶/删底移动确定边界，未知操作只扩张"可能被删"的边界。 | 不枚举 2^? 种选择，只维护确定区间与可能区间两对指针。 | 统计 0、1、2 操作，逐位置输出必删 '-'、必留 '+' 或不确定 '?'。 |
| [CF2151B](https://codeforces.com/problemset/problem/2151/B) | Incremental Path | B 命令跳到下一白格，相当于动态 successor；每个前缀又从 1 重走，但可增量复用路径。 | 直接重跑是 O(n²)；新染黑只会让 successor 链向右跳。 | 维护当前前缀终点与有序黑点/并查集 next-unpainted，逐命令增量更新并加入终点。 |

### M1 操作闭包与不变量（6 题）

**做题第一问：** 操作反复做后，哪些量永远不变？可达位置是否形成等价类？

| 比赛题 | 题目 | 核心转化 | 变换点 / 创新点 | 处理方法 |
| --- | --- | --- | --- | --- |
| [CF2244C](https://codeforces.com/problemset/problem/2244/C) | Stepan and Permutation | 受限交换形成图，能交换到的位置是由步长 gcd 决定的余数类。 | 题面像排列交换，真正区别是"每个元素只需检查目标位置是否同属一个连通分量"。 | 求允许步长的 gcd，按下标模 gcd 分组，验证每个值的当前位置与目标位置同余。 |
| [CF2222B](https://codeforces.com/problemset/problem/2222/B) | Artistic Balance Tree | 镜像交换保持下标奇偶类，两个奇偶位置集合互不混合。 | 树/艺术叙事被化为两个独立多重集合的匹配。 | 分别统计奇位和偶位值，排序或计数后检查目标集合。 |
| [CF2197B](https://codeforces.com/problemset/problem/2197/B) | Array and Permutation | 相邻复制操作不能改变目标中不同值块的相对次序。 | 值可扩张但不能凭空跨过别的值；压缩后的目标必须是原排列的子序列。 | 压缩目标连续相等段，检查每个值只出现一段且压缩序列为 p 的子序列。 |
| [CF2191B](https://codeforces.com/problemset/problem/2191/B) | MEX Reordering | 前后缀 MEX 只受从 0 开始的低值连续段影响。 | 大值均是噪声；真正限制集中在 0、1… 的出现位置与次数。 | 统计低值，判断必要条件后把关键低值按构造顺序放置，其余任意。 |
| [CF2176B](https://codeforces.com/problemset/problem/2176/B) | Optimal Shifts | 循环位移式置 1 操作每次可同时缩短所有零段。 | 总操作数不是零的数量，而是环上最长连续零段。 | 把字符串复制一遍或环扫描，求最长零段。 |
| [CF2131C](https://codeforces.com/problemset/problem/2131/C) | Make it Equal | 操作 ±k 与取绝对值保持规范余数 min(r,k-r)。 | 原值正负和多次操作全部压成一个等价类代表。 | 把两数组都映射到 canonical residue，排序后比较。 |

### M6 字符串正规形与有限状态（5 题）

**做题第一问：** 重复删除/翻转是否存在唯一正规形？目标模板是否只有常数个？

| 比赛题 | 题目 | 核心转化 | 变换点 / 创新点 | 处理方法 |
| --- | --- | --- | --- | --- |
| [CF2241C](https://codeforces.com/problemset/problem/2241/C) | RemovevomeR | 删除回文子序列前先压缩相邻相同字符，问题只剩交替串的长度。 | 原字符串细节大多无关；只有压缩后长度等于 2 时需要两次，其余一次。 | 线性去重压缩，按压缩长度直接判答案。 |
| [CF2230B](https://codeforces.com/problemset/problem/2230/B) | Digit String | 十进制数被 4 整除只看末两位，因此禁用子序列只涉及单个 4 与有序末两位。 | 从"删字符串"降为在一个切点两侧保留允许数字的计数。 | 枚举/扫描切点，维护左侧 2 类与右侧 1/3 类等合法计数，取最小删除。 |
| [CF2225B](https://codeforces.com/problemset/problem/2225/B) | Alternating String | 翻转一次区间后成为交替串，等价于相对某个目标模板的失配位置连续。 | 只需测试 0101… 与 1010… 两个模板。 | 记录失配位置，判断是否为空或构成一个连续区间。 |
| [CF2200C](https://codeforces.com/problemset/problem/2200/C) | Specialty String | 删除相邻相同字符存在唯一栈正规形。 | 多种删除顺序无需搜索；最终能否清空由栈结果决定。 | 逐字符入栈，相同则弹出，最终栈空即成立。 |
| [CF2110B](https://codeforces.com/problemset/problem/2110/B) | Down with Brackets | 删一左一右不改总平衡，只可能让某个前缀变负。 | 最优破坏方式等价于看原串是否存在除末尾外的内部前缀平衡归零。 | 扫描括号前缀；若在最后位置前出现 balance=0，则 YES，否则 NO。 |

### M7 位运算与线性结构（4 题）

**做题第一问：** 逐 bit 是否独立？能否改写成前缀 XOR 或 GF(2) 方程？

| 比赛题 | 题目 | 核心转化 | 变换点 / 创新点 | 处理方法 |
| --- | --- | --- | --- | --- |
| [CF2240B](https://codeforces.com/problemset/problem/2240/B) | AI Finds Nothing Here | 矩阵异或条件本质是 GF(2) 上的线性约束，二维前缀异或使每个子矩形可表达。 | 不是暴力枚举矩阵；自由度由独立约束数决定。 | 推导行列/前缀变量的自由元数量，答案为 2 的自由度次方。 |
| [CF2192B](https://codeforces.com/problemset/problem/2192/B) | Flipping Binary String | 每次除一个位置外全部翻转，可写成 GF(2) 线性方程。 | 选择集合由全局操作奇偶直接确定：零数奇偶决定选零位或一位。 | 设总操作奇偶，逐位解 xi xor parity=目标，检查自洽并输出集合。 |
| [CF2162C](https://codeforces.com/problemset/problem/2162/C) | Beautiful XOR | 限制 x≤当前值时，最高位决定能否一步 XOR 到目标。 | 任意可行实例最多两步；关键是先调整最高位，再补差。 | 比较 bit_length；若 a=b 为 0 步，a xor b≤a 则一步，否则构造中间值两步。 |
| [CF2153B](https://codeforces.com/problemset/problem/2153/B) | Bitwise Reversion | 三个两两 AND 的单个二进制位只可能出现 000、100、010、001、111。 | 恰好两个 1 的位型不可能，这是完整充要条件。 | 逐位等价检查：不存在只出现在 x,y,z 中恰好两个数的 bit。 |

### M8 阈值、均衡与二分（4 题）

**做题第一问：** 答案是否随某阈值单调？瓶颈是不是平均值、最大缺口或水位？

| 比赛题 | 题目 | 核心转化 | 变换点 / 创新点 | 处理方法 |
| --- | --- | --- | --- | --- |
| [CF2232B](https://codeforces.com/problemset/problem/2232/B) | Cake Leveling | 前 j 项可达到的共同最低水平由 floor(prefix_sum/j) 决定。 | 看似操作模拟，实质是"水位不能超过前缀平均值"的瓶颈。 | 维护前缀和，对每个 j 输出/更新 floor(Sj/j) 的最小约束。 |
| [CF2209B](https://codeforces.com/problemset/problem/2209/B) | Array | 不等式 abs(ai-k)>abs(aj-k) 对 k 是由中点决定的半轴条件。 | 对每个 i，所有 j 只贡献一个阈值方向，问题变成选 k 覆盖最多半轴。 | 收集与 ai 的中点阈值并排序，统计左右可同时满足的最大数。 |
| [CF2207B](https://codeforces.com/problemset/problem/2207/B) | One Night At Freddy's | 对手可增量/清零的过程本质是负载均衡与 majorization。 | 最坏策略只关心当前最小/最大负载分布，不关心完整历史。 | 排序或维护水位，尽量均衡负载，推导能保证的极值。 |
| [CF2152B](https://codeforces.com/problemset/problem/2152/B) | Catching the Krug | t 回合后 Krug 的可达集是受边界裁剪的矩形，Doran 的切比雪夫可达集也是矩形。 | 生存 t 秒可行具有单调性，追逐博弈转为矩形包含判定。 | 二分 t，check Krug 可达矩形是否仍存在 Doran 无法覆盖的位置/是否未被包含。 |

## 建议训练顺序

1. 先做 **M3 + M5**：训练从题面故事中提取排序、极值、前缀和计数。
2. 再做 **M4 + M1**：每题强制写出操作前后不变量与充要条件。
3. 然后做 **M2**：从目标反推，积累 gadget、补排列、前缀 XOR 等构造方式。
4. 最后穿插 **M6/M7/M9/M10**：练状态压缩、逐位独立、博弈势能和动态过程静态化。

## 核验说明

A 级 87 题；B 级 13 题。B 级题已核对题面并完成主模型推导，但由于比赛较新或公开题解材料较少，正式做题前建议再读官方 editorial 核边界。

> 本报告是训练型归纳，不是官方题解合集。分类允许一题有主模型与副模型；主模型表示最值得迁移到下一题的思维步骤。

---

## 训练维护说明

本文件是相对静态的完整基础题库。

个人训练进度不在 100 行表格中逐项反复修改，避免正式分析内容与每日状态耦合。

动态状态统一维护在：

- [[03 - 当前训练队列]]
- `00 - 每日复盘/`
- `01 - 题目复盘/`
- `06 - 二刷清单/`

已有单题复盘只在总索引的"已存在复盘入口"中集中导航，不为全部题目提前创建空文件。

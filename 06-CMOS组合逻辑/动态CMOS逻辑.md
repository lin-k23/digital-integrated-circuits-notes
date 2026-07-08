# 动态 CMOS 逻辑

## 一句话理解

动态 CMOS 逻辑（dynamic CMOS logic）用时钟把电路分成预充电（precharge）和求值（evaluate）两个阶段：预充电时把动态输出节点充到高电平，求值时只用 NMOS 下拉网络决定是否放电。它面积小、输入电容小、速度快，但对 charge sharing、leakage、crosstalk、clock feedthrough、monotonicity 和级联规则更敏感。

## 来源范围

- 主要参考：用户上传课件 `VLSI Design : 2021-22 Lecture 12 Dynamic Logic`，共 26 页。
- 对应教材：`source/ch6_en.pdf`，第 6 章 6.3 Dynamic CMOS Design。
- 本轮整理范围：static vs dynamic、dynamic gate、dynamic gate properties、transition activity、charge sharing、keeper、capacitive coupling、contention、clock feedthrough、monotonicity、cascading dynamic gates。

## 为什么需要动态逻辑

静态 CMOS 的优点是鲁棒、满摆幅、无比例、稳态低功耗，但复杂函数可能需要大量晶体管。对 $N$ 输入逻辑：

$$
\text{static CMOS transistor count}\approx 2N
$$

因为每个输入通常同时驱动一个 NMOS 和一个 PMOS。

动态逻辑只用 NMOS 下拉网络实现逻辑函数，再加：

- 一个预充电 PMOS。
- 一个求值 NMOS footer。

因此晶体管数量约为：

$$
\text{dynamic CMOS transistor count}\approx N+2
$$

也就是：

```text
N 个 PDN NMOS + 1 个 evaluate NMOS + 1 个 precharge PMOS
```

代价是输出节点在某些阶段处于高阻态，逻辑值靠电容暂存。

## 基本动态门结构

典型动态门包括：

- 预充电 PMOS $M_p$：由 clock 控制，负责把输出节点充到 $V_{DD}$。
- NMOS 下拉网络 PDN：实现逻辑函数。
- 求值 NMOS $M_e$：由 clock 控制，求值阶段接通到 GND。
- 输出负载电容 $C_L$：保存动态节点电荷。

> 图占位：课件第 4/6 页 dynamic logic / dynamic gate 结构图，待后续补图。

## 两相操作

动态逻辑需要周期性执行：

```text
precharge -> evaluate -> precharge -> evaluate -> ...
```

### 预充电阶段

当：

$$
CLK=0
$$

有：

- PMOS $M_p$ 导通。
- evaluate NMOS $M_e$ 关断。
- 输出节点被充到高电平。

因此：

$$
Out=1
$$

### 求值阶段

当：

$$
CLK=1
$$

有：

- PMOS $M_p$ 关断。
- evaluate NMOS $M_e$ 导通。
- 如果 PDN 导通，输出节点放电到 0。
- 如果 PDN 不导通，输出节点保持预充的高电平。

因此动态门输出是反相型逻辑：

```text
PDN 条件为真 -> Out 被放电为 0
PDN 条件为假 -> Out 保持为 1
```

## 例：动态门实现 \(\overline{AB+C}\)

若 PDN 实现：

$$
AB+C
$$

则求值阶段输出为：

$$
Out=\overline{AB+C}
$$

直觉：

- $A$ 与 $B$ 串联表示 $AB$。
- 与 $C$ 并联表示 $AB+C$。
- 输出是 PDN 导通条件的反函数。

> 需核对：课件第 6 页示例图中输出旁标注与公式排版较密，本笔记按动态门基本逻辑整理为 $Out=\overline{AB+C}$。

## 动态门的重要性质

### 1. 一旦放电，不能在本次求值阶段重新充电

在 evaluate 阶段，$M_p$ 关断。如果输出节点已经被 PDN 放电，则它不能重新升高，必须等下一次 precharge。

这带来单调性约束：求值期间逻辑只能从“高保持”变成“低放电”，不能从低变回高。

### 2. 输入在求值阶段最多只能发生一次有效变化

课件强调：输入在 evaluate 阶段最多只能发生一次 transition。更精确地说，动态门通常要求输入在求值阶段单调上升：

```text
0 -> 0 允许
0 -> 1 允许
1 -> 1 允许
1 -> 0 不允许
```

如果输入从 1 变 0，输出本应从低恢复为高，但动态门没有上拉路径，输出无法恢复。

### 3. 输出可能处于高阻态

如果 evaluate 阶段 PDN 不导通，则输出节点既没有连接到 $V_{DD}$，也没有连接到 GND，而是靠 $C_L$ 暂存高电平。

这使输出对 leakage、noise、crosstalk 和 clock feedthrough 很敏感。

### 4. 满摆幅和无比例

理想情况下：

$$
V_{OH}=V_{DD},\qquad V_{OL}=0
$$

动态 CMOS 仍是 non-ratioed logic：器件尺寸不决定理想逻辑电平，但会影响速度、噪声、keeper contention 和可靠性。

### 5. 速度通常更快

速度优势来自：

- 逻辑只由 PDN 实现，晶体管数量少。
- 每个输入只驱动一个 NMOS gate，输入电容更小。
- 输出负载通常比等价静态 CMOS 小。
- evaluate 阶段没有 PMOS 与 NMOS 同时形成短路路径，PDN 电流主要用于放电 $C_L$。

## 功耗特点

动态逻辑没有静态直流路径，也没有传统静态 CMOS 翻转区的短路功耗路径。但总功耗不一定低。

原因：

- 每个周期都要预充动态节点，哪怕本次 evaluate 不放电。
- clock 驱动 $M_p$ 和 $M_e$，引入额外 clock load。
- 输出预充为 1 后，如果 PDN 导通会发生 1→0；下周期又会 0→1 预充，活动概率可能较高。
- keeper、内部节点预充和复杂 clock 网络会增加负载。

因此：

```text
动态逻辑面积/速度可能更优，但功耗不一定比静态 CMOS 低。
```

## 噪声容限

动态门的 PDN 只要输入超过 NMOS 阈值就可能开始导通，因此低噪声容限较差。

课件中给出的直觉是：

$$
V_M\approx V_{IH}\approx V_{IL}\approx V_{Tn}
$$

因此：

```text
NML 偏小，动态节点对低电平噪声敏感
```

> 需核对：该关系是课件第 9 页的设计直觉，严格 VTC 仍取决于器件、keeper 和负载。

## Transition activity

开关活动概率通常写为：

$$
P_{0\to1}=P_{out=0}\cdot P_{out=1}
$$

若记：

$$
P_0=P_{out=0},\qquad P_1=P_{out=1}=1-P_0
$$

则：

$$
P_{0\to1}=P_0(1-P_0)
$$

对 2 输入 NOR：

$$
P_{out=1}=(1-P_A)(1-P_B)
$$

$$
P_{out=0}=1-(1-P_A)(1-P_B)
$$

因此：

$$
P_{0\to1}=\left[1-(1-P_A)(1-P_B)\right](1-P_A)(1-P_B)
$$

当：

$$
P_A=P_B=\frac{1}{2}
$$

有：

$$
P_{0\to1}=\frac{3}{4}\cdot\frac{1}{4}=\frac{3}{16}
$$

> 图占位：课件第 11–13 页 transition activity 表格和示例，待后续补图。

## Charge sharing / charge redistribution

动态门中存在内部节点电容。求值时，预充的输出节点可能与未预充的内部节点相连，导致电荷重新分配，使输出高电平下降。

设：

- 输出节点电容为 $C_Y$。
- 内部节点电容为 $C_X$。
- 初始输出 $Y$ 约为 $V_{DD}$。
- 内部节点 $X$ 可能处于低电平或浮空。

### Case 1：内部节点只被拉到约 \(V_{DD}-V_{Tn}\)

课件给出的近似：

$$
V_{DD}C_Y=V_YC_Y+(V_{DD}-V_{Tn})C_X
$$

对应输出下降：

$$
\Delta V_{out}\approx-\frac{(V_{DD}-V_{Tn})C_X}{C_Y}
$$

该近似常用于 $C_X\ll C_Y$ 的情况。

### Case 2：输出和内部节点充分共享到同一电压

若 $Y$ 与 $X$ 最终近似同电位：

$$
V_{DD}C_Y=(V_{DD}+\Delta V_{out})(C_Y+C_X)
$$

得到：

$$
\Delta V_{out}=-\frac{V_{DD}C_X}{C_Y+C_X}
$$

### 设计风险

若输出下降太多，后级可能把原本应为高的动态节点误判为低，尤其当：

- $C_X$ 不小。
- 后级阈值偏高。
- 动态节点还受到 leakage / crosstalk / clock feedthrough 影响。

## Charge sharing 的解决方法

### 1. 预充内部节点

用额外 clock-driven transistor 把关键内部节点也预充到高电平。

优点：

- 减少电荷从输出节点流向内部节点。
- 降低 charge sharing 导致的输出跌落。

代价：

- 面积增加。
- clock load 增加。
- 动态功耗增加。

### 2. 使用 keeper

在动态输出节点加一个弱 PMOS keeper，用于恢复和保持高电平。

作用：

- 抵抗 leakage。
- 抵抗 charge sharing。
- 抵抗轻微 capacitive coupling。

但 keeper 必须足够弱，不能严重阻碍 PDN 放电。

> 图占位：课件第 17/20 页内部节点预充与 keeper PMOS，待后续补图。

## Capacitive coupling

动态输出节点在 evaluate 阶段可能是 high impedance，因此对电容串扰特别敏感。

如果有一根 full-swing wire 靠近或跨过动态节点，耦合电容可能注入电荷，破坏 floating node 的状态。

这与第 9 章 [[../09-互连影响与应对/电容串扰与可靠性|电容串扰与可靠性]] 完全对应：动态节点是 capacitive crosstalk 的典型 victim。

应对：

- 避免敏感动态节点旁长距离平行走线。
- 使用 shielding。
- 降低动态节点阻抗。
- 使用 keeper。
- 关键动态节点尽量远离 full-swing clock / bus / reset。

## Backgate / output-to-input coupling

课件还列出 backgate 或 output-to-input coupling。直觉上，动态输出节点或后级输入的耦合可能把前后级信号相互扰动，特别是在：

- 输出节点高阻。
- 后级输入快速翻转。
- 版图中存在较强寄生耦合。
- 动态门直接级联。

这类问题通常需要结合版图寄生、后级输入电容和信号时序仿真分析。

## Contention

keeper 虽然能保持高电平，但在 PDN 需要放电时，keeper PMOS 会和 PDN 形成竞争：

```text
keeper 上拉 vs PDN 下拉
```

这称为 contention。

影响：

- 放电变慢。
- 动态功耗增加。
- 若 keeper 过强，PDN 可能无法可靠拉低输出。

设计规则：

- keeper 要足够弱，只负责补偿泄漏和小扰动。
- PDN 必须能在目标延迟内打败 keeper。
- 高速低阈值工艺中可能需要 contention-free keeper 结构。

> 图占位：课件第 21–22 页 keeper contention 与 contention-free domino 结构，待后续补图。

## Clock feedthrough

clock 控制 $M_p$ / $M_e$。这些晶体管的栅-漏/栅-源重叠电容会把 clock 边沿耦合到动态输出节点，造成 clock feedthrough。

表现：

- 预充结束或求值开始时输出出现毛刺。
- 输出可能产生 overshoot / undershoot。
- 与 charge sharing / crosstalk / leakage 叠加后可能造成错误。

> 图占位：课件第 23 页 clock feedthrough 波形，待后续补图。

## Monotonicity 约束

动态门 evaluate 阶段要求输入单调上升：

```text
允许：0->0, 0->1, 1->1
禁止：1->0
```

原因：

- 如果输入先让 PDN 导通，输出被放电。
- 后来输入从 1 变 0，让 PDN 断开。
- 输出本应回到高电平，但 evaluate 阶段 $M_p$ 已关断，无法恢复。

这也是动态门直接级联会出错的根本原因之一。

> 图占位：课件第 24 页 monotonicity violation 波形，待后续补图。

## Cascading dynamic gates

直接把动态门输出接到另一个动态门输入通常不安全。

原因：

- 每个动态门在 precharge 后输出都为 1。
- 下一级 dynamic gate 在 evaluate 初期可能看到前级输出为 1，于是错误放电。
- 当前级随后在同一 evaluate 阶段下降为 0，下一级已经放电，无法再恢复。

典型解决方式：

- 在动态门后加静态反相器，形成 domino logic。
- 使用两相非重叠 clock。
- 采用带约束的 np-CMOS / NORA 等动态逻辑风格。

本轮只整理入口，后续可在 6.3.4 中进一步展开 domino / cascading dynamic gates。

## 动态逻辑优缺点总结

| 维度 | 优点 | 代价 |
|---|---|---|
| 面积 | 晶体管数量少，约 $N+2$ | 需要 clock、keeper、预充内部节点时面积回升 |
| 速度 | 输入电容和输出负载小，PDN 放电快 | keeper contention、charge sharing、clock load 会拖慢 |
| 功耗 | 无静态直流路径、无典型短路功耗 | 预充频繁、clock load 大、活动概率高 |
| 鲁棒性 | 理想满摆幅、无比例 | 高阻动态节点对泄漏/串扰/噪声敏感 |
| 级联 | 可做高速 domino | 直接级联存在 monotonicity 风险 |

## 设计直觉

- 动态逻辑的速度来自“只用 NMOS PDN + 小电容”。
- 动态逻辑的风险来自“高阻节点 + 电荷暂存”。
- keeper 是可靠性补丁，但会引入 contention。
- 预充内部节点能缓解 charge sharing，但增加 clock 负载和功耗。
- 动态逻辑适合高性能、规则、强约束的数据通路，不适合作为默认鲁棒逻辑风格。

## 易错点

- 不要把动态门当作普通静态门直接级联。
- 不要忽略 evaluate 期间输入单调性。
- 不要只看晶体管数量少，clock load 和预充功耗可能很大。
- 不要让动态节点长时间 floating。
- 不要把 keeper 做得太强，否则会和 PDN 争用。
- 不要忽略 clock feedthrough 和 crosstalk 对动态节点的影响。

## 相关链接

- [[组合逻辑章节总览]]
- [[静态CMOS设计总览]]
- [[互补CMOS逻辑门]]
- [[NAND与NOR门设计]]
- [[../05-CMOS反相器/功耗与能量模型|功耗与能量模型]]
- [[../09-互连影响与应对/电容串扰与可靠性|电容串扰与可靠性]]
- [[../09-互连影响与应对/串扰对延迟的影响|串扰对延迟的影响]]
- [[公式与考点速查]]

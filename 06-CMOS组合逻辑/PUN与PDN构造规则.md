# PUN 与 PDN 构造规则

## 一句话理解

PUN 与 PDN 的构造规则就是把布尔逻辑翻译成晶体管串并联网络：NMOS 串联表示 AND、并联表示 OR；PMOS 网络是 NMOS 网络的对偶，即串并联互换。

## 来源范围

- 来源 PDF：`source/ch6_en.pdf`
- 对应小节：6.2.1 Complementary CMOS

## MOS 管作为开关

对 NMOS：

- 输入为 1 时导通。
- 输入为 0 时关断。

对 PMOS：

- 输入为 0 时导通。
- 输入为 1 时关断。

因此：

```text
NMOS 是正逻辑开关
PMOS 是反逻辑开关
```

## NMOS 网络规则

### 串联 NMOS：AND

两个 NMOS 串联时，只有两个输入都为 1，才存在导通路径。

因此：

$$
\text{series NMOS} \Rightarrow A\cdot B
$$

### 并联 NMOS：OR

两个 NMOS 并联时，只要有一个输入为 1，就存在导通路径。

因此：

$$
\text{parallel NMOS} \Rightarrow A+B
$$

> 图占位：图 6.4 NMOS 串联表示 AND、并联表示 OR，待后续补图。

## PMOS 网络规则

由于 PMOS 在输入为 0 时导通，其逻辑关系等价于对输入取反。

### 串联 PMOS

两个 PMOS 串联时，只有两个输入都为 0，才存在上拉路径：

$$
\bar{A}\cdot\bar{B}=\overline{A+B}
$$

因此 PMOS 串联实现 NOR 条件。

### 并联 PMOS

两个 PMOS 并联时，只要有一个输入为 0，就存在上拉路径：

$$
\bar{A}+\bar{B}=\overline{AB}
$$

因此 PMOS 并联实现 NAND 条件。

## 对偶网络规则

互补 CMOS 中，PUN 和 PDN 是对偶网络（dual network）。构造规则：

```text
PDN 中串联 ↔ PUN 中并联
PDN 中并联 ↔ PUN 中串联
NMOS ↔ PMOS
```

也就是说，先构造其中一个网络，再把串并联关系互换即可得到另一个网络。

## 从布尔函数构造 CMOS 门的步骤

常用流程：

1. 明确输出函数 $F$。
2. 找到输出应为 0 的条件，即 $\bar{F}$。
3. 用 NMOS 串并联构造 PDN 实现 $\bar{F}$。
4. 用对偶规则构造 PUN 实现 $F$。
5. 检查所有输入组合下输出是否总能接到 $V_{DD}$ 或 GND。
6. 检查是否有 PUN / PDN 同时导通的情况。

## 对偶构造的层级方法

对复杂网络，最好用层级子网络（sub-net）构造：

- 把 PDN 分成若干串联/并联子网络。
- 在 PUN 中逐层互换串并联。
- 对每个子网络递归应用对偶规则。

这比直接对整个表达式做德摩根变换更不容易出错。

> 图占位：图 6.6 复杂互补 CMOS 门的 PDN 与 PUN 层级构造，待后续补图。

## 德摩根定律对应关系

PUN / PDN 对偶本质上来自德摩根定律：

$$
\overline{A+B}=\bar{A}\cdot\bar{B}
$$

$$
\overline{A\cdot B}=\bar{A}+\bar{B}
$$

从电路角度看：

- OR 的反函数变成 AND of complements。
- AND 的反函数变成 OR of complements。
- 串联和并联正好互换。

## 设计直觉

- 先画 PDN 通常更直观，因为 NMOS 在输入为 1 时导通。
- PDN 实现输出为 0 的条件，PUN 自动实现输出为 1 的条件。
- 每个输入在 PUN 和 PDN 中各出现一次，构成互补结构。
- 高 fan-in 串联堆栈会增加延迟，后续尺寸设计必须补偿。

## 易错点

- 不要把 PMOS 串并联规则按 NMOS 直接套用，PMOS 是反逻辑开关。
- 不要只做布尔表达式变换而不检查电路是否对偶。
- 复杂网络中串并联层级容易画错，应分层构造。
- PUN / PDN 同时关断会导致输出浮空，同时导通会导致静态电流。

## 相关链接

- [[互补CMOS逻辑门]]
- [[静态CMOS设计总览]]
- [[NAND与NOR门设计]]
- [[公式与考点速查]]

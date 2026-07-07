# SR 锁存器与触发器

## 一句话理解

SR 锁存器/触发器用 set 和 reset 输入控制双稳态存储单元：set 使输出 $Q=1$，reset 使输出 $Q=0$，但某些输入组合会破坏 $Q$ 与 $\bar{Q}$ 的互补关系，因此被视为 forbidden state。

## 来源范围

- 来源 PDF：`source/ch7_en.pdf`
- 对应小节：7.2.2 SR Flip-Flops

## 从双稳态到可控存储

[[双稳态原理与亚稳态]] 中的交叉耦合反相器可以保存状态，但缺少外部控制输入。SR 结构在双稳态核心外加 set / reset 控制，使电路可以被写入指定状态。

SR 的含义：

- S：set，使 $Q=1$。
- R：reset，使 $Q=0$。

## NOR-based SR flip-flop

教材首先给出基于交叉耦合 NOR 门的 SR flip-flop。

结构直觉：

- 两个 NOR 门交叉耦合。
- 一个 NOR 的输出为 $Q$。
- 另一个 NOR 的输出为 $\bar{Q}$。
- S 和 R 作为额外输入，用来强制状态。

> 图占位：图 7.6 NOR-based SR flip-flop 的电路、符号和 characteristic table，待后续补图。

## NOR-based SR 行为

| S | R | Q(next) | 说明 |
|---|---|---|---|
| 0 | 0 | 保持 | 两个 NOR 等效为交叉耦合反相器 |
| 1 | 0 | 1 | set |
| 0 | 1 | 0 | reset |
| 1 | 1 | 非法 | $Q$ 与 $\bar{Q}$ 同时被强制为 0 |

非法状态的问题：

- $Q$ 与 $\bar{Q}$ 不再互补。
- 当 S 和 R 同时回到 0 时，最终状态取决于哪个输入最后撤销。
- 可能导致不可预测状态或亚稳态。

## NAND-based SR flip-flop

SR 结构也可以用交叉耦合 NAND 门实现。注意 NAND-based SR 的有效输入通常是低有效。

常见写法：

- $\bar{S}$：低有效 set。
- $\bar{R}$：低有效 reset。

与 NOR-based SR 不同，NAND-based SR 的 forbidden state 通常发生在两个低有效输入同时为 0。

> 图占位：图 7.7 NAND-based SR flip-flop，待后续补图。

## clocked SR latch

异步 SR flip-flop 不需要 clock。同步系统中，更常见的是 clocked SR latch。

教材给出一种 clocked SR latch：

- 核心仍是交叉耦合反相器。
- 增加 4 个晶体管，用 S / R 和 CLK 控制状态写入。
- 当 clock 允许时，S / R 才能改变锁存器状态。

> 图占位：图 7.8 CMOS clocked SR flip-flop，待后续补图。

## clocked SR latch 的尺寸问题

clocked SR latch 的写入过程需要外部输入“打败”正反馈。

因此：

- 写入路径晶体管不能太弱。
- 如果 set/reset 路径太弱，内部节点无法越过后级反相器的开关阈值。
- 教材例 7.1 通过把相关晶体管尺寸增大来保证可写入。

这体现了时序单元的一个重要区别：

```text
功能正确性有时依赖晶体管尺寸，而不仅仅依赖布尔逻辑结构。
```

## 静态功耗

clocked SR latch 在稳态下没有从 $V_{DD}$ 到 GND 的直流路径；只有切换期间可能出现瞬态电流。

但实际功耗还包括：

- 时钟负载功耗。
- set/reset 切换功耗。
- 节点电容充放电能量。
- 泄漏功耗。

## 设计直觉

- SR latch 是理解可控双稳态存储的最简单入口。
- NOR-based SR 通常高有效，NAND-based SR 通常低有效。
- forbidden state 必须避免。
- clocked SR latch 引入同步控制，但尺寸设计会影响写入能力。
- SR 结构适合理解原理，但实际通用寄存器更常用 multiplexer-based latch 或 transmission-gate register。

## 易错点

- 不要把 NOR-based SR 和 NAND-based SR 的有效电平混淆。
- 不要忽略 forbidden state；它会破坏 $Q$ 与 $\bar{Q}$ 的互补关系。
- 不要把 clocked latch 当成 edge-triggered register；clocked latch 仍是电平敏感。
- 写入路径太弱时，电路可能在仿真中“逻辑对”但实际不能可靠翻转。

## 相关链接

- [[双稳态原理与亚稳态]]
- [[存储单元分类]]
- [[时序约束与寄存器时序参数]]
- [[../06-CMOS组合逻辑/NAND与NOR门设计|NAND与NOR门设计]]
- [[公式与考点速查]]

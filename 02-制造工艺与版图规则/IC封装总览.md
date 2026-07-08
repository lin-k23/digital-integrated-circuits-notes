# IC 封装总览

## 一句话理解

IC 封装（IC packaging）把硅片上的电路连接到外部世界，同时提供供电/信号引脚、散热路径、机械支撑和环境保护；随着片上延迟和电容缩小，封装寄生越来越可能成为性能、功耗和可靠性的关键限制。

## 来源范围

- 来源 PDF：`source/ch2_en.pdf`
- 对应小节：2.4 Packaging Integrated Circuits、2.4.1 Package Materials

## 封装的基本作用

封装至少承担四类功能：

1. **电气连接**：把信号线、电源线、地线引出 silicon die。
2. **散热**：把芯片运行产生的热量传到外部环境。
3. **机械支撑**：固定和保护裸片，使其能被装配到 PCB 或系统中。
4. **环境保护**：隔离湿气、污染物和机械损伤。

教材强调，封装不是“后端外壳”，而是会直接影响集成电路的 performance、power dissipation、reliability、longevity 和 cost。

## 为什么封装越来越重要

随着 technology scaling：

- 片上逻辑门延迟下降。
- 片上电容下降。
- 芯片复杂度和 I/O 数量上升。
- 供电电压降低，电流需求变大。

因此封装延迟、封装寄生和供电/散热能力对系统影响更明显。教材中提到，高性能计算机中相当大的延迟可能来自 packaging delay。

## Rent's rule

芯片 I/O pin 数量通常随电路复杂度增加。教材用 Rent's rule 表示 pin count 与 gate count 的经验关系：

$$
P=K G^{\beta}
$$

其中：

- $P$：芯片 I/O pins 数量。
- $G$：gate 数量。
- $K$：平均每个 gate 的 I/O 系数。
- $\beta$：Rent exponent，通常在 $0.1\sim0.7$ 之间。

直觉：

```text
逻辑越复杂，越需要更多片外连接；不同系统类型的 I/O 增长规律不同。
```

## 封装需求

一个好的封装需要同时满足多种约束。

### Electrical requirements

封装 pin 和互连应尽量具有低：

- capacitance
- resistance
- inductance

高速情况下，还要关注 characteristic impedance，使封装互连与传输线行为匹配。

### Mechanical and thermal properties

封装要高效散热，同时保证 die、package carrier 和 board 之间的热膨胀特性匹配，避免长期热循环造成连接失效。

### Low cost

封装成本是系统成本的重要部分。陶瓷封装性能好但贵，塑料封装便宜但散热较差；散热能力越强，封装成本通常越高。

教材给出的数量级：

- 最便宜塑料封装约可散热 1 W。
- 稍贵但仍较便宜的塑料封装约可散热 2 W。
- 大于 50 W 的芯片通常需要 heat sink。
- 更高散热需求可能需要 fan、blower、liquid cooling 或 heat pipe。

> 需核对：散热能力数值来自英文版正文，具体产品需按封装数据手册确认。

## Package materials

常见 package body 材料包括：

- ceramic。
- polymers / plastics。

### Plastic package

优点：

- 成本低。
- 适合大规模商用产品。

缺点：

- 热性能较差。
- 高功率芯片可能无法满足散热需求。

### Ceramic package

优点：

- 热性能好。
- 机械和热膨胀特性更接近互连金属。
- 可靠性更好。

缺点：

- 成本高。
- 介电常数较高，可能带来更大封装互连电容。

## 包装密度与 pin pitch

封装密度会影响 board cost。pin 数增加通常要求：

- 增大 package size。
- 减小 pin pitch。
- 使用更高密度封装形式，如 BGA 或 MCM。

## 设计直觉

- 封装同时是电气、机械、热和成本问题。
- I/O pin count 不只是外设需求，也和芯片复杂度、架构和组织方式有关。
- 封装寄生会影响高速信号和电源完整性。
- 高性能芯片不能只在 die 内部优化，必须把 package 和 board 一起考虑。

## 易错点

- 不要把 package 当成理想连线；它有显著 $RLC$ 寄生。
- 不要只看 signal pins；低电压高性能芯片往往需要大量 power/ground pins。
- 不要认为 ceramic 一定总是更好；它性能好但成本高、介电常数也高。
- 不要忽略散热，温度会放大 leakage、electromigration 和 hot-carrier 问题。

## 相关链接

- [[封装互连层级]]
- [[封装热设计]]
- [[../09-互连影响与应对/I-O驱动与焊盘注意事项|I/O驱动与焊盘注意事项]]
- [[../04-导线与互连模型/互连电感|互连电感]]
- [[公式与考点速查]]

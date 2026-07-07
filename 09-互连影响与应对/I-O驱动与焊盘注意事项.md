# I/O 驱动与焊盘注意事项

## 一句话理解

I/O pad driver 面对的是片外大电容、封装寄生、电源/地弹噪声和 ESD 风险；因此它不是普通反相器放大版，而是同时受速度、面积、可靠性、封装和保护电路约束的综合设计问题。

## 来源范围

- 来源 PDF：`source/ch9_en.pdf`
- 对应小节：9.2.2 中 Driving Off-Chip Capacitances、Designing Reliable Input/Output Pads、Some Interesting Driver Circuits

## 为什么 I/O 更难

片上普通 gate 的负载通常是 fF 级，而片外输出可能需要驱动：

- bonding wire。
- package pin。
- PCB trace。
- 其他芯片输入电容。

片外负载典型可达 20–50 pF，相比普通片上负载大很多。

同时，I/O driver 的瞬态电流很大，会引入：

- supply bounce。
- ground bounce。
- ringing。
- overshoot / undershoot。
- latchup 风险。
- 封装和板级电磁/传输线问题。

## 输出 pad driver

输出 pad driver 的目标不是“尽可能快”，而是：

```text
在满足目标延迟的前提下，尽量降低面积、能量和供电噪声。
```

关键设计点：

- 使用缓冲链驱动片外大电容。
- 后级晶体管很宽，需要合理版图实现。
- 控制 rise/fall time，避免过快边沿导致 $L\,di/dt$ 噪声。
- final stage 周围使用 guard ring 抑制 latchup。
- 尽量避免在最终输出级串接晶体管。

## guard ring

大输出驱动器切换大电容时会产生大电流，可能触发寄生双极晶体管导致 latchup。

常见做法：

- NMOS 周围放置接地的 p+ guard ring。
- PMOS 周围放置接 $V_{DD}$ 的 n+ guard ring。
- 用 guard ring 收集注入的少数载流子，避免触发寄生路径。

> 图占位：图 9.9 bonding-pad driver final stage layout 与 guard ring，待后续补图。

## 输入 pad 与 ESD

输入 pad 的第一级 gate 直接连接外部世界，MOS gate 输入阻抗极高，gate oxide 又很薄，因此对静电放电（ESD）非常敏感。

ESD 风险来源：

- 人体携带静电。
- 装配机器携带静电。
- 封装和测试过程中的异常电压。

教材中指出，人体或机器的高静电电位足以击穿 gate oxide。

## ESD 保护电路

典型输入保护结构包含：

- 串联电阻 $R$：限制峰值电流。
- 到 $V_{DD}$ 和 GND 的二极管 clamp：限制 pad 电压超出供电轨太多。
- 额外电容和版图保护结构。

代价：

- 串联电阻和输入电容形成 RC。
- 高速输入路径中可能限制性能。
- 保护越强，输入负载可能越大。

> 图占位：图 9.10 输入 pad 的 ESD protection circuit 与 layout，待后续补图。

## 三态输出与总线

多个模块共享 bus 时，同一时刻只能有一个发送端驱动总线。

未发送的 driver 进入 high impedance：

```text
Z state
```

因此三态缓冲器具有三种输出状态：

- 0
- 1
- Z

设计注意：

- 简单三态 inverter 可通过串联 enable transistor 实现。
- 但驱动大负载时，最终输出级中串联器件会增大电阻和面积。
- 更适合把 enable 控制放在前级，保持最终输出级为强驱动结构。

## 与后续 inductive parasitics 的联系

本文件只整理了 9.2 中与 I/O 相关的电容负载与 pad 可靠性入口。第 9.4 会继续讨论：

- bonding wire / package pin inductance。
- $L\,di/dt$ noise。
- simultaneous switching noise。
- decoupling capacitance。
- termination 和 transmission line。

这些内容决定 I/O driver 的真正约束。

## 设计直觉

- I/O driver 设计要同时满足速度、面积、功耗、噪声和可靠性。
- 片外负载大，不代表最终级越大越好。
- 过快边沿会造成供电噪声，后续可能比延迟更关键。
- ESD 保护不可省略，但会引入 RC 性能代价。
- 总线输出必须考虑三态和 high-Z 状态。

## 易错点

- 不要把 output pad driver 当作简单 buffer chain 处理。
- 不要忽略 guard ring 和 latchup。
- 不要把 ESD 保护当作理想开路；它有电容、电阻和速度代价。
- 不要让多个 tri-state driver 同时驱动同一 bus。
- 不要为了最快边沿而牺牲 supply integrity。

## 相关链接

- [[大电容负载与缓冲链驱动]]
- [[电容串扰与可靠性]]
- [[../05-CMOS反相器/反相器尺寸优化与缓冲链|反相器尺寸优化与缓冲链]]
- [[../02-制造工艺与版图规则/README|制造工艺与版图规则]]
- [[公式与考点速查]]

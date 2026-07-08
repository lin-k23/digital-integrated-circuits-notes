# CMOS 工艺流程总览

## 一句话理解

简化的 dual-well CMOS 流程可以理解为：先隔离 active region，再形成 n/p wells，接着做栅氧和多晶硅栅，再自对准形成源漏，最后堆叠接触孔、通孔和多层金属互连，并以钝化层保护芯片。

## 来源范围

- 来源 PDF：`source/ch2_en.pdf`
- 对应小节：2.2.4 Simplified CMOS Process Flow

## 简化流程图

教材把 dual-well CMOS 流程概括为：

```text
Define active areas / etch and fill trenches
-> implant well regions
-> deposit and pattern polysilicon layer
-> implant source/drain regions and substrate contacts
-> create contact and via windows
-> deposit and pattern metal layers
```

> 图占位：图 2.6 dual-well CMOS 简化工艺序列，待后续补图。
>
> 图占位：图 2.7 NMOS/PMOS transistor fabrication process flow，待后续补图。
>
> 图占位：图 2.8 state-of-the-art CMOS process cross-section，待后续补图。

## 1. 定义 active region 与隔离

工艺首先定义 active regions，即晶体管将要形成的位置。

非 active 区域覆盖厚的 $SiO_2$，称为 field oxide，用来隔离相邻器件。

隔离方式可以是：

- 生长 field oxide。
- 在刻蚀出的 trench 中填入绝缘材料，即 trench insulation。

同时可能加入 channel-stop implant / field implant 来增强隔离，降低寄生沟道导通风险。

## 2. 形成 wells 与阈值调节

接着通过 ion implantation 和 annealing 形成：

- n-well。
- p-well。

并通过额外 implant 调整器件阈值电压：

- PMOS 的 $V_{Tp}$ 调整。
- NMOS 的 $V_{Tn}$ 调整。

这一步说明工艺已经开始直接影响第 3 章中的 MOS 器件参数。

## 3. 淀积并图形化多晶硅

形成薄栅氧化层之后，淀积 polysilicon 并通过光刻/刻蚀图形化。

多晶硅有两个作用：

- 作为 MOS gate electrode。
- 作为局部互连材料。

## 4. 自对准源漏注入

源漏区通过后续 ion implantation 形成：

- NMOS：形成 $n^+$ source/drain。
- PMOS：形成 $p^+$ source/drain。

由于 polysilicon gate 已经先形成，源漏注入时 gate 会遮挡其下方的 channel region，因此源漏位置由 gate 精确确定。

这称为：

```text
self-aligned process
```

优点是 source/drain 与 gate 的相对位置非常精确，有利于缩小器件并控制寄生。

## 5. 接触孔、通孔和金属层

完成器件后，工艺转入互连：

1. 淀积绝缘材料，通常是 $SiO_2$。
2. 刻蚀 contact holes 或 via holes。
3. 淀积金属，常见为 Al / Cu，低层也可能用 W。
4. 图形化金属。
5. 通过 CMP 等步骤维持表面平坦。
6. 重复上述步骤形成多层金属互连。

现代 CMOS 中，互连层往往占据整个工艺截面的主要垂直高度，晶体管本身只占底部很小一部分。

## 6. 钝化与焊盘开口

最后一层金属完成后，会淀积 passivation / overglass 保护层。

常见材料：

- CVD $SiO_2$。
- nitride layer，用于提高抗湿气能力。

最后刻蚀出 pad openings，用于后续 bonding / packaging。

## 与后续章节的关系

- 源漏扩散、电容和阈值影响 [[../03-器件与MOS晶体管模型/README|器件模型]]。
- 多层金属、接触孔和通孔影响 [[../04-导线与互连模型/README|导线与互连模型]]。
- pad openings 与封装影响 [[../09-互连影响与应对/I-O驱动与焊盘注意事项|I/O 驱动与焊盘注意事项]]。

## 设计直觉

- gate 先图形化，source/drain 后注入，是 self-aligned MOS 的关键。
- 多层金属不是附属结构，而是现代芯片的主要三维结构。
- contact/via 太多会增加电阻、电容、面积和可靠性风险。
- 钝化层不是电路功能层，但对可靠性和封装很重要。

## 易错点

- 不要把 process flow 中的示意图比例当成实际比例。
- 不要忽略 self-aligned process 对器件尺寸控制的意义。
- 不要把 polysilicon 只当 gate，它也可作局部互连，但电阻较高。
- 不要忘记最终 pad opening，否则封装无法连接到芯片内部金属。

## 相关链接

- [[硅片与阱结构]]
- [[光刻与掩膜]]
- [[常见工艺步骤]]
- [[设计规则与版图约束]]
- [[../04-导线与互连模型/导线电路模型|导线电路模型]]
- [[公式与考点速查]]

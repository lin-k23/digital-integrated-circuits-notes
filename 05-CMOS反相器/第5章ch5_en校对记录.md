# 第 5 章 ch5_en 校对记录

## 校对目标

使用 `source/ch5_en.pdf` 作为英文文字版来源，对第 5 章 CMOS 反相器笔记进行阶段性校对。

当前校对范围已覆盖第 5 章完整主线：

- 5.1 Introduction
- 5.2 The Static CMOS Inverter — An Intuitive Perspective
- 5.3 Evaluating the Robustness of the CMOS Inverter: The Static Behavior
- 5.4 Performance of CMOS Inverter: The Dynamic Behavior
- 5.5 Power, Energy, and Energy-Delay
- 5.6 Perspective: Technology Scaling and its Impact on the Inverter Metrics

## 校对原则

- 章节结构、公式和英文术语以 `source/ch5_en.pdf` 为准。
- 中文术语沿用课程常用译法和前面章节中的稳定译名。
- 对容易混淆的 PMOS 符号、延迟方向、活动因子、短路功耗系数和 SPICE 电流方向保留提醒。
- 本阶段不添加图片，只检查图占位符是否与相关主题匹配。

## 总体结论

当前第 5 章整理内容与 `ch5_en.pdf` 主线一致：

- 反相器是数字设计的基本单元，复杂 CMOS 逻辑门可从反相器方法外推。
- 静态 CMOS 反相器的关键优点是满摆幅输出、无比例逻辑、高输入阻抗、低输出阻抗和理想静态无直流通路。
- VTC、开关阈值、$V_{IL}$、$V_{IH}$ 和噪声容限构成静态鲁棒性分析主线。
- 动态行为由负载电容充放电主导，一阶模型为 $0.69R_{eq}C_L$。
- 速度优化必须同时考虑 $C_L$、晶体管尺寸、self-loading、fan-out 和输入斜率。
- 功耗主线分为动态功耗、短路功耗和静态泄漏。
- 工艺缩放引入低 $V_{DD}$、低 $V_T$、泄漏和互连占比上升之间的折中。

未发现需要推翻的已写结论。本阶段将第 5 章补成完整复习初稿。

## 分文件校对记录

| 文件 | 校对结论 | 当前处理 |
|---|---|---|
| [[反相器章节总览]] | 与英文版 5.1 一致：按 cost、robustness、performance、energy 四类指标分析反相器。 | 已写入初稿。 |
| [[静态CMOS反相器直觉模型]] | 与英文版 5.2 一致：互补开关、满摆幅、无比例逻辑、高输入阻抗、低输出阻抗和理想静态零功耗。 | 已写入初稿。 |
| [[电压传输特性与开关阈值]] | 与英文版 5.3.1 一致：VTC、load-line、PMOS 坐标变换、$V_M$ 和尺寸比影响。 | 已写入初稿。 |
| [[噪声容限与鲁棒性]] | 与英文版 5.3.2–5.3.3 一致：$V_{IL}$、$V_{IH}$、$NM_L$、$NM_H$、分段线性近似和电源缩放下界。 | 已写入初稿。 |
| [[动态行为与传播延迟]] | 与英文版 5.4.1–5.4.2 一致：$C_L$ 组成、Miller 等效、扩散电容线性化、传播延迟积分式和 RC 延迟。 | 已写入初稿。 |
| [[反相器尺寸优化与缓冲链]] | 与英文版 5.4.3 一致：PMOS/NMOS 比例、self-loading、effective fanout、反相器链和输入斜率。 | 已写入初稿。 |
| [[功耗与能量模型]] | 与英文版 5.5 一致：动态功耗、短路功耗、静态泄漏、energy-delay 和 SPICE 功耗测量。 | 已写入初稿。 |
| [[工艺缩放对反相器指标的影响]] | 与英文版 5.6 一致：能量、电压、阈值、泄漏和互连缩放趋势。 | 已写入初稿。 |
| [[公式与考点速查]] | 已补充第 5 章主线公式。 | 已更新。 |

## 已确认的关键公式

### 静态输出电平

$$
V_{OH}=V_{DD},\qquad V_{OL}=0
$$

### 开关阈值

$$
V_M=\frac{\left(V_{Tn}+\frac{V_{DSATn}}{2}\right)+r\left(V_{DD}+V_{Tp}+\frac{V_{DSATp}}{2}\right)}{1+r}
$$

$$
r=\frac{k_pV_{DSATp}}{k_nV_{DSATn}}
$$

### 噪声容限

$$
NM_L=V_{IL}-V_{OL}
$$

$$
NM_H=V_{OH}-V_{IH}
$$

### 传播延迟

$$
t_{pHL}\approx0.69R_{eqn}C_L
$$

$$
t_{pLH}\approx0.69R_{eqp}C_L
$$

### 负载电容

$$
C_L=C_{gd1}+C_{gd2}+C_{db1}+C_{db2}+C_w+C_{g3}+C_{g4}
$$

### 反相器链

$$
F=\frac{C_L}{C_{g,1}},\qquad f=F^{1/N}
$$

### 动态功耗

$$
P_{dyn}=\alpha C_LV_{DD}^2f
$$

### 静态功耗

$$
P_{static}=I_{static}V_{DD}
$$

### 总功耗

$$
P_{tot}=P_{dyn}+P_{sc}+P_{static}
$$

## 仍需人工核对或后续补充

- 表 5.1、表 5.2 的完整版图尺寸、电容组成和数值。
- 例 5.3–5.14 的完整数值推导。
- 短路功耗三角波近似中系数和 $I_{peak}$ 的具体表达式。
- SPICE 功耗测量中电流方向和符号约定。
- 工艺缩放表格中的完整指数关系。
- 若课程需要例题训练，建议单独整理“第 5 章例题选讲”。

## 本阶段没有做的事

- 没有添加或裁剪图片。
- 没有完整录入所有表格。
- 没有完整复现 SPICE 网表和仿真波形。
- 没有整理完整设计题。

## 下一步建议

第 5 章已经完成复习笔记初稿。后续可转入第 6 章 CMOS 组合逻辑，或另开 PR 做第 5 章表格/例题补充。

# 动态 CMOS 逻辑

## 一句话理解

动态 CMOS 逻辑（dynamic CMOS logic）利用时钟控制的预充电（precharge）和求值（evaluate）阶段，把逻辑值暂存在高阻动态节点的电容上；它通常能减少晶体管数量并提高速度，但对泄漏、charge sharing、clock feedthrough、串扰和级联规则更敏感。

## 当前状态

本文件是交叉引用入口页。第 6 章后续会根据 `source/ch6_en.pdf` 的 6.3 Dynamic CMOS Design 系统整理动态逻辑。

当前先保留轻量入口，避免第 9 章中与动态节点串扰相关的链接落空。

## 基本阶段

动态 CMOS 门通常分为两个阶段：

```text
precharge phase -> evaluate phase
```

- precharge：动态节点被预充到高电平。
- evaluate：根据输入条件决定动态节点是否通过 PDN 放电。

## 为什么第 9 章会引用动态逻辑

[[../09-互连影响与应对/电容串扰与可靠性|电容串扰与可靠性]] 中提到，动态节点常常是串扰敏感点，因为它们：

- 可能处于 high impedance。
- 依赖电容暂存电荷。
- 电压摆幅或噪声裕量可能更小。
- 同时受 leakage、charge sharing 和 clock feedthrough 影响。

因此动态 CMOS 是互连串扰可靠性讨论中的典型受害电路。

## 复习重点预告

后续整理 6.3 时建议覆盖：

- precharge / evaluate 工作机制。
- 单调性约束（monotonicity）。
- charge sharing。
- leakage 与 keeper。
- clock feedthrough。
- cascading dynamic gates。
- domino logic。
- dynamic logic 的速度、面积、功耗和鲁棒性折中。

## 相关链接

- [[组合逻辑章节总览]]
- [[静态CMOS设计总览]]
- [[互补CMOS逻辑门]]
- [[../09-互连影响与应对/电容串扰与可靠性|电容串扰与可靠性]]
- [[../09-互连影响与应对/串扰对延迟的影响|串扰对延迟的影响]]
- [[公式与考点速查]]

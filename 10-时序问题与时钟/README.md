# 10-时序问题与时钟

本章对应教材第 10 章 “Timing Issues in Digital Circuits”，用于整理数字电路中的时序方法、clock skew、clock jitter、时钟分发、锁存器时钟方案、自定时电路、同步器、仲裁器和 PLL。

## 本章目标

- 区分 synchronous、mesochronous、plesiochronous、asynchronous 四类数字系统/互连时序关系。
- 掌握边沿触发同步系统的基本 setup / hold 约束。
- 理解 clock skew 对性能和 race/hold failure 的双重影响。
- 理解 clock jitter 如何降低可用周期时间并压缩时序裕量。
- 后续继续整理 skew / jitter 来源、clock distribution、latch-based clocking、self-timed design、synchronizer / arbiter 和 PLL。

## PDF 可整理性

- 英文来源 PDF：`source/ch10_en.pdf`，可直接提取文字。
- 评估记录：[[第10章可整理性评估]]
- 处理策略：以英文文字版为公式、变量和章节结构基准；中文术语沿用第 7 章时序逻辑、第 9 章互连影响与时钟网络相关译法。

## 当前整理进度

- 已整理初稿：10.1 [[时序问题章节总览]]。
- 已整理初稿：10.2 [[数字系统时序分类]]。
- 已整理初稿：10.3.1 部分：[[同步时序基础与时钟偏斜]]。
- 已整理初稿：10.3.1 部分：[[时钟抖动与时序裕量]]。
- 已整理公式入口：[[公式与考点速查]]。
- 下一步建议：继续整理 10.3.2 Sources of Skew and Jitter，然后整理 clock-distribution techniques。

## 知识点索引

- [[时序问题章节总览]]
- [[数字系统时序分类]]
- [[同步时序基础与时钟偏斜]]
- [[时钟抖动与时序裕量]]
- [[公式与考点速查]]

## 前置链接

- [[../07-时序逻辑/README|07-时序逻辑]]
- [[../07-时序逻辑/时序约束与寄存器时序参数|时序约束与寄存器时序参数]]
- [[../07-时序逻辑/存储单元分类|存储单元分类]]
- [[../09-互连影响与应对/README|09-互连影响与应对]]
- [[../09-互连影响与应对/串扰对延迟的影响|串扰对延迟的影响]]

## 图片占位规则

本章不提交 PDF 截图。需要图片时只保留占位符，例如：

```markdown
> 图占位：图 10.6 正 clock skew 对同步路径性能与 hold/race 的影响，待后续补图。
```

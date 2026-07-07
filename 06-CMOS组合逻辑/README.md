# 06-CMOS组合逻辑

本章对应教材第 6 章“CMOS 组合逻辑门设计（Designing Combinational Logic Gates in CMOS）”，用于从 [[../05-CMOS反相器/README|CMOS反相器]] 推广到 NAND、NOR、复杂组合逻辑、不同逻辑风格与低功耗/高性能设计技术。

## 本章目标

- 区分组合逻辑（combinational logic）和时序逻辑（sequential logic）。
- 掌握静态互补 CMOS（static complementary CMOS）的基本结构。
- 理解上拉网络（pull-up network, PUN）和下拉网络（pull-down network, PDN）的互补关系。
- 掌握 NMOS / PMOS 串并联网络与布尔逻辑之间的对应关系。
- 能够从布尔表达式构造 NAND、NOR 和复杂 CMOS 门。
- 理解为什么 NMOS 适合下拉、PMOS 适合上拉。
- 建立后续学习 ratioed logic、pass-transistor logic、dynamic logic 和 logic style 选择的框架。

## PDF 可整理性

- 英文来源 PDF：`source/ch6_en.pdf`，可直接提取文字。
- 评估记录：[[第6章可整理性评估]]
- 处理策略：以英文文字版为公式、变量和章节结构基准；中文术语沿用前面章节和课程常用译法。

## 当前整理进度

- 已整理初稿：6.1 [[组合逻辑章节总览]]。
- 已整理初稿：6.2 [[静态CMOS设计总览]]。
- 已整理初稿：6.2.1 [[互补CMOS逻辑门]]。
- 已整理初稿：[[PUN与PDN构造规则]]。
- 已整理初稿：[[NAND与NOR门设计]]。
- 下一步建议：继续整理复杂 CMOS 门综合、晶体管尺寸与延迟、逻辑努力、功耗和 glitching。

## 知识点索引

- [[组合逻辑章节总览]]
- [[静态CMOS设计总览]]
- [[互补CMOS逻辑门]]
- [[PUN与PDN构造规则]]
- [[NAND与NOR门设计]]
- [[公式与考点速查]]

## 前置链接

- [[../03-器件与MOS晶体管模型/MOS晶体管总览|MOS晶体管总览]]
- [[../03-器件与MOS晶体管模型/MOS工作区与电流模型|MOS工作区与电流模型]]
- [[../05-CMOS反相器/静态CMOS反相器直觉模型|静态CMOS反相器直觉模型]]
- [[../05-CMOS反相器/动态行为与传播延迟|动态行为与传播延迟]]
- [[../05-CMOS反相器/功耗与能量模型|功耗与能量模型]]

## 图片占位规则

本章不提交 PDF 截图。需要图片时只保留占位符，例如：

```markdown
> 图占位：图 6.2 互补 CMOS 逻辑门的 PUN/PDN 结构，待后续补图。
```

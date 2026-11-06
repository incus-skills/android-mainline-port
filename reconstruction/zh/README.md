# 中文重构记录

## 目的

本目录保存我们对原始技能库所作的中文描述性重构，用于保留观察过程的来源、结构理解、关系判断、歧义和未解决问题，使另一位观察者能够检查并重建我们如何从源材料得出当前理解。

这些记录不是原作者的说明，也不代表原作者认可我们的解释。

## 与其他材料的区别

| 材料 | 角色 |
|---|---|
| 原始中文源文件 | 当前技能内容的权威来源 |
| 本目录中的中文重构 | 我们对源材料的观察、结构恢复、关系理解、歧义和解释边界 |
| `translation/en/` | 源技能的功能性英文表达 |
| `translation/reviews/` | 英文翻译的来源记录、术语选择、重构观察和审查边界 |

中文重构不是原文副本，也不是英文翻译的回译。它应当记录观察者如何理解材料及其证据边界。

## 记录原则

- 明确源文件路径、分支和可用的 blob SHA。
- 区分源文明确陈述、跨文件关系、观察者推断和未解决问题。
- 保留原文没有建立的关系、分类、阈值或完成条件，不自行补齐。
- 记录当前观察位置及本次重构的范围。
- 不把结构性理解等同于技术正确性验证。
- 后续作者反馈（如有）应作为独立来源记录；澄清或扩展不能追溯性地变成原文已有证据。

## 时间与来源说明

本目录中的记录可能是对先前观察的回顾性重构。记录创建时间不应被误认为原始观察发生时间；若无法恢复某项判断的确切形成时间，应明确保留这一限制。


## 会话入口与遍历轮次

会话连续性记录位于 [continuation/](continuation/current-position.md)：

- `current-position.md` 是活动入口，帮助新会话恢复当前观察位置和下一步。
- `checkpoints/` 保存各会话的观察位置快照，避免后续更新抹去先前位置。
- **Pass（遍历轮次）**表示对已声明范围从起点到终点的一次完整遍历；一个 Pass 可以跨越多个会话。
- 检查点记录同一 Pass 内的位置变化。后续从头开始的新一轮完整遍历应使用新的 Pass 编号，并与先前观察进行比较。

Pass 编号、文档位置和会话检查点编号是三个不同维度，不应混用。


- [`display.md`](display.md) records the retrospective reconstruction of the display experience document and its relationships with the general problem patterns and kernel/resource material.

- [`gpu.md`](gpu.md) 记录 GPU 与图形栈的推荐路线、验收标准、故障表面、显式跨文件关系及未解决边界。
- [`touch-and-input.md`](touch-and-input.md) 记录触控形态、面板生命周期、按键与键盘配件、指纹、用户侧验收、跨文件关系及手写笔正文覆盖缺口。
- [`audio.md`](audio.md) 记录 AudioReach/ADSP 路线、硬件拓扑、分层验收证据、功放增益安全、UCM/PipeWire 故障、跨文件对应关系和未解决事项。

- [`wireless.md`](wireless.md) 记录 Wi-Fi 与蓝牙的独立路线、验收序列、故障机制、显式模式引用、观察者对应关系和未解决边界。
- [`power-and-time.md`](power-and-time.md) 记录电量与充电证据、RTC/系统时间、休眠与时钟测量、跨文件模式关系、看门狗/温控覆盖缺口和未解决边界。
- [`sensors-camera-media.md`](sensors-camera-media.md) 记录 SSC 传感器与自动旋转、Iris/Venus 视频硬解、相机用户入口、显式模式引用、观察者重构出的跨层对应及未解决边界。
- [`usb-and-storage.md`](usb-and-storage.md) 记录 USB 角色与 Type-C、UFS/分区身份、活动槽镜像集轮换、稀疏镜像写入语义、备份一致性、恢复通道和安装边界。


- [Pass 1 / Checkpoint 07](continuation/checkpoints/pass-1-checkpoint-07.md) 记录电量、充电、休眠与系统时间的重构，以及向传感器、相机和视频硬解的过渡。
- [Pass 1 / Checkpoint 08](continuation/checkpoints/pass-1-checkpoint-08.md) 记录传感器、相机和视频硬解的重构，以及向 USB 与存储的过渡。

- [Pass 1 / Checkpoint 09](continuation/checkpoints/pass-1-checkpoint-09.md) 记录 USB、Type-C、存储、双系统与安装的重构，以及向 Ubuntu 用户态的过渡。

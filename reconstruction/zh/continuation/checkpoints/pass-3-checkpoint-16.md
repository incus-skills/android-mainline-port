# Pass 3 — Checkpoint 16: `experience/audio.md` 语义审查

## 位置

- Pass：**Pass 3 — 操作性英文翻译的技术与语义审查**
- 文档：`experience/audio.md`
- 源分支：`main`
- 源 blob SHA：`46e576b39a1d724e2b9d422373a05ea60409dcfc`
- 英文译文：`translation/en/experience/audio.md`
- 英文译文 blob SHA：`7764597a750a7cc4c22873feddcb5005fef2537d`
- 译文审查记录：`translation/reviews/experience/audio.md.md`
- 前一文档：`experience/touch-and-input.md`
- 下一文档：`experience/wireless.md`
- 外部技术正确性验证：未进行。

## 审查结果

完整对照六步推荐路线、五项完成标准、全部十八项坑条目、三项未解决问题、技术 token、命令力度、条件范围、结构及跨文件关系。**未发现需要源文驱动的译文修正。**

### 保留的结构与区别

- ADSP 固件加载（PAS）、QRTR、PD mapper 和 GPR 保持为前置条件，不被误作音频输出证据。
- 目标设备拓扑仍由原厂运行时设备树和 mixer 配置确定；同 SoC 已跑通的后端/功放驱动是优先复用基线，不构成普遍拓扑。
- PCM open、hw_params、prepare、DSP 播放图接受、功放启动、全零播放、电气回环和人耳确认的逐层顺序得到保留。
- 播放/DSP/功放状态、机器可观测信号、人耳确认、麦克风声源刺激与无声源对照、全新安装状态仍是不同验收证据表面。
- 十八项坑条目的独立机制及不统一字段结构均得到保留，包括 TDM 时钟、root compatible 改写、异步错误、崩溃原因上报、PCM 格式/声道交集、DAPM/trigger 时序、PRM 时钟 ID、拓扑端口、设备树调参文件命名、增益与扬声器保护、PipeWire 上混、UCM 路由/控件、profile 整体失效、固定 DMA 通道、麦克风刺激、ALSA sequencer、MMAP 预填、厂商遥测、占位供电和增益上限取舍。
- 关键命令力度与条件得到保留：先检查被改写的根 compatible；先单变量验证唯一差异；先修复崩溃原因上报再定位；启动期间出现任何功放/APM/ADSP 错误即判验收失败；固件版本或算法不符警告按真实问题处理；不要以连续 6 dB 步进试增益；默认 verb 不包含未验证的外接显示输出。
- 技术 token、数值、错误与配置名保持原样，包括 S32/S16/S24、四声道限制、`Invalid channels (8)`、`mute_unmute_on_trigger`、`ControlExists`、`/var/lib/alsa/asound.state`、`arecord`、EIO、`SND_SEQUENCER=m`、PREPARED/START、`dvdd-supply`、`regulator-fixed`、约 −24 dB 与 −4.5 dB，以及 AudioReach、ADSP、APM、PRM、LPASS、ACDB、q6apm、wm_adsp、UCM、PipeWire 和 WirePlumber。
- 三项未解决问题保持不变：主线缺少 ADSP 扬声器保护模块；启动时偶发功放上电错误是否无害未知；WirePlumber 崩溃后 APM/PRM 请求失去响应且需重启恢复。
- 源文件本身没有 Markdown 跨文件链接，译文没有添加此类链接。Pattern D、Pattern F 和 `kernel-and-resources.md` 的联系仍作为集合层关系或观察者对应，而非伪称为 audio.md 的直接引用。

## 结构与观察者边界

逐层路线没有被扩展为通用硬件拓扑或状态机。同 SoC 复用仍是参考起点，目标设备证据仍决定实际拓扑。PCM/DSP/功放状态、电气输出、人耳可闻和麦克风功能之间的证据边界没有合并。

源文没有建立适用于所有高通设备的统一拓扑、完整麦克风/功放/后端变体映射、WirePlumber/APM/PRM 故障的一般恢复程序或脱离具体功放与保护实现的安全增益上限。这些缺失得到保留。

## Pass 3 处置

- 源文对照技术与语义翻译审查：**完成**。
- 译文修改：**无**。
- AudioReach/API 当前行为、DSP 拓扑、固件与调参兼容性、PipeWire/UCM 行为、功放寄存器、安全增益及真机行为：**未验证**。
- 整体技能验证：仍未完成。
- 下一文档：`experience/wireless.md`。

本检查点记录翻译忠实度，不证明音频子系统或设备特定技术主张正确。

# Pass 3 — Checkpoint 14: `experience/gpu.md` 语义审查

## 位置

- Pass：**Pass 3 — 操作性英文翻译的技术与语义审查**
- 文档：`experience/gpu.md`
- 源分支：`main`
- 源 blob SHA：`64079b8f8e5913de51114f4459bc1bc628e14791`
- 英文译文：`translation/en/experience/gpu.md`
- 译文审查记录：`translation/reviews/experience/gpu.md.md`
- 前一文档：`experience/display.md`
- 下一文档：`experience/touch-and-input.md`
- 外部技术正确性验证：未进行。

## 审查结果

完整对照中文源文与英文译文，检查五步推荐路线、四项完成标准、全部九个坑条目、未解决事项、技术 token、结构及显式跨文件引用。

**发现四处需要修正的表达力度或范围。** 已完成以下调整：

1. 固件签名约束：将原译文的 “may be required” 修正为 **“only the device's original factory-signed file can be used”**，恢复源文“只能用”的排他性。
2. GMU HFI 超时/hangcheck：删除源文未表达的 “apparently” 弱化语气，保留其与 GPU 代码无关的陈述。
3. 同一条目中的替代原因：移除原译文额外加入的 “may”，恢复 MX dependency vote 使用错误 OPP 表、或电源域被置于 retention 档的直接原因陈述。
4. 共享 GL context 竞态：将 “the failure is reproducible” 修正为 **“the failure reproduces every time”**，保留源文“必现”的力度及其所述 freedreno/threaded-context 条件。

除此之外，没有发现需要源文驱动的语义修正。

## 对照确认保留

- 五步推荐路线保留内核、固件、Mesa、沙箱应用库和新 GPU 的 Mesa 未成熟/回移债务等不同层次；没有将推荐路线补造成通用依赖图或强制状态机。
- 不需要 zap 的绘制验证路径仍是有平台条件的路线，没有推广为普遍能力。
- 四项完成标准保持区分：硬件渲染器身份、逐像素正确性、电源切换后的持续渲染，以及桌面合成器和复杂应用的实际 GPU 渲染（包括 GTK GL 与 Vulkan）。
- 全部九个坑条目的不同字段结构、症状、原因、确认步骤（源文提供时）、解决方式、适用范围和时间标记均予保留；没有补造缺失字段。
- 完整 chip ID、`drm/msm`、sqe、GMU、SMMU、OPP、zap、Mesa、Turnip、NV12、BGRx、NEON、`GALLIUM_THREAD=0`、`FD_MESA_DEBUG=sysmem`、`LD_PRELOAD`、`vkcube`、`gnome-shell --headless --virtual-monitor`、错误/函数标识和版本适用范围均得到保留。
- 对 `kernel-and-resources.md` 与 `ubuntu-userspace.md` 的显式引用得到保留。GPU 源文没有显式引用 `touch-and-input.md`，因此没有新增该引用。
- 未解决范围仍是新 GPU 的 Vulkan 稳定性、持续负载、调频和电源管理。

## 显式关系与观察者重构

显示资源回收与 GPU/GMU 超时之间的关系由源文明确连接到内核/资源文档，同时保留 GPU 电源投票的替代原因及诊断顺序。该关系不意味着每个 GMU 超时都由同一原因造成。

渲染器身份与测试有效性、抓帧路径限制与实际渲染功能之间的区别，可作为观察者层面的对应关系；源文没有将其定义为正式分类体系。

源文没有建立通用固件签名流程、GPU 电源状态模型、完整测试矩阵，也没有给出普遍适用的 Mesa 回移债务退出成熟度标准。上述缺口继续保持未解决，不通过推断补齐。

## Pass 3 处置

- 源文对照技术与语义翻译审查：**完成**。
- 译文修改：**四处表达力度/范围修正**；未修改中文源文件。
- 外部技术正确性、当前上游状态、跨设备普遍性、固件有效性及真机行为：**未验证**。
- 整体技能验证：仍未完成。
- 下一文档：`experience/touch-and-input.md`。

本检查点记录翻译忠实度，不证明 GPU、固件、Mesa、Turnip 或设备特定技术主张正确。

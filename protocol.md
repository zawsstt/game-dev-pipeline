# SSP v1.1 — Search-Synthesize-Produce Protocol

> **独立协议文档。本文件被所有 Agent System Prompt import/引用。**
> 任何 Agent 在执行生成任务前，必须完整执行本协议三步。违规处理见第四节。
> **v1.1 新增**：AIGC 资产生成执行协议（第六节）、代码编写执行协议（第七节）。

---

## 一、协议版本与适用范围

| 字段 | 值 |
|------|---|
| 协议版本 | v1.1 |
| 发布日期 | 2025-07 |
| 适用范围 | 游戏工业化管线 AI 体系内的所有 Agent（固定层 + 动态层） |
| 强制性 | **强制**。Master Agent 负责监督所有 Sub-Agent 的合规执行。 |
| 豁免情形 | 无豁免。即使 Ticket 内容极其简单，也必须执行至少一次 Web_Search。 |
| v1.1 新增 | AIGC 资产执行协议（ART / AUDIO / VFX Agent 强制）；代码编写协议（DEV Agent 强制）。 |

---

## 二、三步执行规范（完整版）

---

### [SSP STEP 1 — SEARCH · 搜索]

**目标**：获取该子领域的外部高质量参考解法，建立知识基线。

**行动规范**：

1. 构造 2-3 个检索词，遵循"宽泛→精准"递进原则：
   - 检索词 1（宽泛）：领域通用术语，例：`roguelike design best practice`
   - 检索词 2（精准）：品类+具体问题，例：`roguelike randomness PRNG mitigation`
   - 检索词 3（可选，案例导向）：指定竞品或事件，例：`Slay the Spire RNG design analysis`

2. 执行工具调用，严格限定 Ticket 中声明的搜索域：
   ```
   [TOOL_CALL] Web_Search(
     query        = "{检索词}",
     domain_filter = "{Ticket 授权域，精确枚举}"
   )
   ```
   如需抓取评论：
   ```
   [TOOL_CALL] Scrape_Comments(
     url = "{Ticket 中明确指定的 URL}"
   )
   ```

3. **输出**：原始检索摘要（≤ 500 字），每条标注来源 URL。

**禁止行为**：
- ❌ 使用 Ticket 未授权的搜索域（如 Ticket 只授权 GDC，不得搜索 Reddit）
- ❌ 构造与任务目标偏离的检索词（如数值 Agent 搜索 UI 相关内容）
- ❌ 跳过本步骤直接进入 Step 2

---

### [SSP STEP 2 — SYNTHESIZE · 提炼]

**目标**：将外部原始信息转化为本项目可用的"临时知识框架"。

**行动规范**：

1. 从检索结果中识别**核心原则**（≤ 5 条），格式：
   ```
   原则 {N}：{一句话描述}
   来源：[{URL 摘要}]
   本项目应用建议：{结合项目约束条件的转化说明}
   ```

2. 识别需要规避的**反模式 Anti-pattern**（≤ 3 条），格式：
   ```
   Anti-pattern {N}：{一句话描述}
   案例：{来自检索的反面案例}
   规避方案：{具体设计对策}
   ```

3. **输出**：结构化"检索摘要 + 应用建议"文本（≤ 300 字）

**禁止行为**：
- ❌ 直接将外部内容复制粘贴到产出文档中（必须转化）
- ❌ Step 2 摘要中不包含任何来源 URL（必须可追溯）
- ❌ 忽视本项目的约束条件（引擎/团队规模/平台等）

---

### [SSP STEP 3 — PRODUCE · 生成]

**目标**：结合本项目约束条件，输出符合 Ticket 规格的最终产出物。

**行动规范**：

1. 正文至少一处显式引用 Step 2 的知识框架，格式：
   ```
   [来源: {URL 或知识框架条目名称}]
   ```

2. 严格按照 Ticket 规定的产出格式生成内容（不得自行增删章节）。

3. 为每项关键设计结论标注置信度：
   - **High**：有直接的外部参考依据（来源明确）
   - **Medium**：类比推断（来自相近品类或机制）
   - **Low**：无外部依据，纯逻辑推演

4. 置信度 **Low** 的产出，必须在该条目下附加"待验证假设"：
   ```
   ⚠️ [待验证假设]
   假设内容：{具体说明}
   建议验证方式：{用户测试 / 原型验证 / 行业专家评审}
   ```

5. 文档末尾必须附"参考依据"章节：
   ```markdown
   ## 参考依据
   | # | 来源 URL | 关键摘要（≤50字） | 置信度 |
   |---|---------|----------------|------|
   ```

**禁止行为**：
- ❌ 跳过 Step 1/2，直接执行本步骤
- ❌ 产出中不含任何来源引用（[来源: XXX] 格式缺失）
- ❌ 置信度 Low 的产出未附"待验证假设"清单

---

## 三、违规判定标准表

| # | 违规行为 | 判定方 | 处理方式 | 计入打回次数？ |
|---|---------|-------|---------|------------|
| V-01 | 跳过 Step 1，无搜索直接生成 | Master Agent | 立即打回，要求重新执行完整 SSP | ✅ 是 |
| V-02 | Step 2 摘要未在产出文档"参考依据"章节出现 | QA Agent | 视为文档不完整，打回 | ✅ 是 |
| V-03 | 使用 Ticket 未授权的搜索域 | Master Agent | 越权记录，本次检索结果作废，要求重新执行 Step 1 | ❌ 否（但须重做） |
| V-04 | 置信度 Low 产出未附"待验证假设"清单 | QA Agent | 要求补充后重新提交 | ❌ 否（不计打回次数） |
| V-05 | 正文无任何 [来源: XXX] 引用 | QA Agent | 视同 V-02 处理 | ✅ 是 |
| V-06 | Step 2 输出字数超过 300 字 | Master Agent | 警告，要求精简；不打回，但记录 | ❌ 否 |
| V-07 | 构造与 Ticket 任务目标偏离的检索词 | Master Agent | 要求重构检索词并重新执行 Step 1 | ❌ 否（但须重做） |
| V-08 | 对运行时状态/实时数据编造具体数字而未声明不确定性 | Master Agent | 立即打回，要求补充 [不确定声明] 标记并说明来源 | ✅ 是 |
| V-09 | Agent 间互动只有赞美，未提供任何可改进点（回声室） | Master Agent | 强制注入一条质疑，要求当事 Agent 补充改进建议后重提 | ❌ 否（补充即可）|
| V-10 | ART/AUDIO/VFX Agent 仅输出资产规格文档，未调用 AIGC API 实际生成资产 | Master Agent | 立即打回，要求执行第六节 AIGC 资产执行协议 | ✅ 是 |
| V-11 | AIGC API 调用成功但未执行下载归档（`_workspace/assets/` 目录缺失对应文件） | Master Agent | 打回，要求补充 Write_File 归档操作 | ✅ 是 |
| V-12 | DEV Agent 仅输出代码文档/描述，未调用 Write_File 实际写入工程文件 | Master Agent | 立即打回，要求执行第七节代码编写执行协议 | ✅ 是 |
| V-13 | 生成资产命名不符合管线规范（见第六节命名规则） | QA Agent | 要求重命名归档，不打回但须修正后重提 | ❌ 否（但须修正） |

> **"打回"定义**：要求 Sub-Agent 重新生成完整文档（含 SSP 执行），计入该 Ticket 的迭代次数。
> **"不计打回"定义**：仅要求补充特定内容，不触发全量重生成，不消耗迭代次数。

---

## 四、降级处理机制

### 触发条件

单一 Sub-Agent 对同一 Ticket 被打回达 **2 次**，且第 2 次提交仍未通过 QA。

### 降级流程

```
[STEP 1] Sub-Agent 输出当前最优版本（不再等待 Master 打回）

[STEP 2] 文档头部加注：
  [DEGRADED OUTPUT - 人工复核建议]
  降级原因：{具体问题描述}
  整体置信度：{Low / Medium}
  未解决问题列表：
    - {问题 1}
    - {问题 2}
  建议人工复核方式：{具体建议}

[STEP 3] Master Agent 综合评估，三选一：
  A. 采纳降级产出 → 在集成报告中标注"待复核"
  B. 人工介入信号 → 暂停自动流程，输出人工介入建议
  C. 跳过该模块 → 在集成报告中标注"模块缺失"，说明影响范围
```

### 降级产出标记规范

```
---
[DEGRADED OUTPUT - 人工复核建议]
降级原因: {一句话}
整体置信度: Low
未解决问题:
  - {问题列表}
建议复核方式: {测试 / 专家评审 / 原型验证}
---
```

### 全局降级触发

当同一项目中降级标记（`[DEGRADED]`）数量 ≥ 2 个时：
- Master Agent 停止自动流程
- 输出"**人工介入建议书**"，说明：
  - 哪些模块有效（可直接使用）
  - 哪些模块需要人工完善
  - 建议优先级排序

---

## 五、协议更新日志

| 版本 | 日期 | 更新内容 |
|------|------|---------|
| v1.0 | 2025-07 | 初版发布。三步规范、违规判定表、降级机制、死循环防护全部就位。 |
| v1.1 | 2025-07 | 新增第六节 AIGC 资产生成执行协议（ART/AUDIO/VFX 强制执行）；新增第七节代码编写执行协议（DEV Agent 强制执行）；违规判定表新增 V-10 ~ V-13。 |
| v1.2 | — | 待迭代 |

---

## 六、AIGC 资产生成执行协议（v1.1 新增）

> **强制适用**：ART Agent、AUDIO Agent、VFX Agent。
> 本节定义了资产生成的完整工具调用规范，所有资产 Agent 在完成设计规格阶段后，必须进入本节定义的执行流程。

---

### 6.1 通用工具调用规范

#### 6.1.1 AIGC API 调用格式（标准伪代码）

```
[TOOL_CALL] Call_AIGC_API(
  type      = "image" | "audio_bgm" | "audio_sfx" | "video",
  prompt    = "{正向提示词，英文优先，≤ 200 tokens}",
  neg_prompt = "{负向提示词，可选}",
  platform  = "midjourney" | "stable_diffusion" | "suno" | "udio" | "elevenlabs" | "runway",
  params    = {
    width    : {像素宽，仅 image},
    height   : {像素高，仅 image},
    duration : {秒数，仅 audio/video},
    format   : "{输出格式，如 png/mp3/ogg}",
    style    : "{风格参数，可选}"
  },
  output_id = "{本次调用的资产标识符，用于后续归档命名}"
)
```

**调用返回值约定**：
```
{
  status   : "success" | "failed" | "pending",
  url      : "{资产下载链接，status=success 时有效}",
  error    : "{错误信息，status=failed 时有效}",
  duration : "{生成耗时（秒）}"
}
```

#### 6.1.2 资产下载与归档格式

```
[TOOL_CALL] Write_File(
  path    = "_workspace/assets/{子目录}/{文件名}",
  mode    = "binary",
  content = {从 Call_AIGC_API 返回 url 下载的资产二进制数据}
)
```

---

### 6.2 资产目录结构规范

```
_workspace/
├── assets/
│   ├── images/
│   │   ├── characters/     # 角色立绘、精灵图
│   │   ├── ui/             # UI 元素、图标、按钮
│   │   ├── backgrounds/    # 背景图
│   │   └── effects/        # 特效贴图、精灵表
│   ├── audio/
│   │   ├── bgm/            # 背景音乐
│   │   └── sfx/            # 音效
│   └── video/              # 过场动画（如有）
```

---

### 6.3 资产命名规范

| 资产类型 | 命名格式 | 示例 |
|---------|---------|------|
| 角色立绘 | `char_{角色ID}_{状态}_{序号}.{格式}` | `char_hero_idle_01.png` |
| UI 元素 | `ui_{模块}_{元素名}_{尺寸}.{格式}` | `ui_hud_healthbar_128x32.png` |
| 背景图 | `bg_{场景名}_{层级}.{格式}` | `bg_forest_layer0.png` |
| 特效贴图 | `vfx_{特效名}_{帧序}.{格式}` | `vfx_explosion_frame01.png` |
| 精灵表 | `sprite_{名称}_{行x列}.{格式}` | `sprite_hero_run_4x3.png` |
| BGM | `bgm_{场景/情绪}_{序号}.{格式}` | `bgm_battle_main_01.mp3` |
| SFX | `sfx_{动作/事件}_{序号}.{格式}` | `sfx_sword_hit_01.ogg` |

> **平台适配说明**：命名中的格式后缀必须严格符合目标平台约束（由 GATE-P 注入）。
> 例如：微信小游戏 BGM 必须为 `.mp3` 或 `.aac`，不得使用 `.wav`。

---

### 6.4 ART Agent 图像生成执行规范

#### Step A — Prompt 构造

1. 从 GDD / ART 规格文档提取关键视觉描述词：
   - 角色关键词：种族、性别、服装、武器、表情、姿态
   - 风格关键词：画风（像素、手绘、写实）、色调、光影
   - 平台关键词：分辨率要求、透明背景需求（`transparent background` / `alpha channel`）

2. 构造 Positive Prompt（英文）：
   ```
   {主体描述}, {风格关键词}, {画质标签}, {技术参数}, {平台适配关键词}
   ```
   示例：
   ```
   a warrior character, pixel art style, 32x32 resolution, 8-bit palette, transparent background, game sprite, side view
   ```

3. 构造 Negative Prompt：
   ```
   blurry, low quality, watermark, text, signature, 3d render, photorealistic {（若需像素风）}
   ```

#### Step B — API 调用

```
[TOOL_CALL] Call_AIGC_API(
  type      = "image",
  prompt    = "{Positive Prompt}",
  neg_prompt = "{Negative Prompt}",
  platform  = "stable_diffusion",   // 或 "midjourney"，依 Ticket 指定
  params    = {
    width   : {目标宽度，像素},
    height  : {目标高度，像素},
    format  : "png",
    style   : "{风格参数}"
  },
  output_id = "{资产标识符}"
)
```

#### Step C — 质量自检

生成完成后，ART Agent 执行以下自检：

```
ART 质量自检清单
☐ 生成结果与 Prompt 描述一致（主体、风格、色调）
☐ 分辨率符合平台规范（由 GATE-P 注入）
☐ 透明背景处理正确（角色/UI 元素）
☐ 文件大小符合平台包体限制
☐ 命名符合 6.3 节规范
全部通过 → 执行 Step D 归档
任一未通过 → 重新构造 Prompt 并重新调用 API（最多 2 次重试）
```

#### Step D — 归档

```
[TOOL_CALL] Write_File(
  path    = "_workspace/assets/images/{子目录}/{按 6.3 规范命名的文件名}",
  mode    = "binary",
  content = {下载的图像二进制数据}
)
```

**完成后输出归档清单**：
```
[ART 归档完成]
生成资产数量：{N}
归档路径：_workspace/assets/images/
清单：
  - {文件名} | {尺寸} | {格式} | {文件大小}
  - ...
占位符说明：
  - {无法生成的资产名称} → [PLACEHOLDER] 原因: {API 限制/风格不符}
```

---

### 6.5 AUDIO Agent 音频生成执行规范

#### Step A — BGM 生成

```
[TOOL_CALL] Call_AIGC_API(
  type     = "audio_bgm",
  prompt   = "{情绪关键词}, {音乐风格}, {乐器组合}, {BPM 描述}, {时长} seconds",
  platform = "suno",              // 或 "udio"，依 Ticket 指定
  params   = {
    duration : {目标时长（秒），建议 60/90/120},
    format   : "{平台要求格式，如 mp3/ogg/aac}",
    loop     : true               // 游戏 BGM 建议开启循环优化
  },
  output_id = "bgm_{场景名}_{序号}"
)
```

**BGM Prompt 示例**：
```
epic orchestral battle music, fast tempo 160 BPM, strings and brass, intense and heroic, 90 seconds, loopable
```

#### Step B — SFX 生成

```
[TOOL_CALL] Call_AIGC_API(
  type     = "audio_sfx",
  prompt   = "{动作描述}, {音效风格}, {持续时长} seconds, game sound effect",
  platform = "elevenlabs",        // 音效推荐 ElevenLabs Sound Effects
  params   = {
    duration : {目标时长，SFX 通常 0.5 ~ 3 秒},
    format   : "{平台要求格式}"
  },
  output_id = "sfx_{事件名}_{序号}"
)
```

**SFX Prompt 示例**：
```
sword slash whoosh sound effect, sharp metallic, 0.8 seconds, game sfx
```

#### Step C — 格式转换（如需）

若平台要求的格式与 API 输出格式不一致，执行格式转换：

```
[TOOL_CALL] Convert_Audio(
  input_path  = "{下载的源文件路径}",
  output_path = "_workspace/assets/audio/{子目录}/{按规范命名的文件名}",
  target_format = "{目标格式，如 mp3/ogg/aac}",
  bitrate       = "{码率，如 128kbps}",
  sample_rate   = "{采样率，如 44100}"
)
```

> **平台适配说明**：
> - 微信小游戏：BGM → `.mp3` (128kbps)，SFX → `.mp3` (64kbps)
> - Web H5：BGM → `.ogg` + `.mp3` 双格式（浏览器兼容），SFX → `.ogg`
> - PC（Godot）：BGM → `.ogg`，SFX → `.wav`（短音效）/ `.ogg`（长音效）

#### Step D — 归档与清单

```
[TOOL_CALL] Write_File(
  path    = "_workspace/assets/audio/{bgm 或 sfx}/{按 6.3 规范命名的文件名}",
  mode    = "binary",
  content = {音频二进制数据}
)
```

**完成后输出归档清单**：
```
[AUDIO 归档完成]
BGM 数量：{N} 首
SFX 数量：{M} 个
归档路径：_workspace/assets/audio/
清单：
  - {文件名} | {时长} | {格式} | {码率} | {文件大小}
  - ...
```

---

### 6.6 VFX Agent 特效素材生成执行规范

#### Step A — 特效贴图生成

```
[TOOL_CALL] Call_AIGC_API(
  type      = "image",
  prompt    = "{特效风格} particle texture, {颜色描述}, transparent background, {形状描述}, glowing, game VFX, sprite sheet",
  neg_prompt = "background, solid color, non-transparent",
  platform  = "stable_diffusion",
  params    = {
    width  : {贴图宽度},
    height : {贴图高度},
    format : "png"              // 特效必须 PNG（支持 Alpha 透明）
  },
  output_id = "vfx_{特效名}_{类型}"
)
```

**特效贴图 Prompt 示例**：
```
fire particle texture, orange and red gradient, transparent background, circular glow, game VFX, sprite sheet, 256x256
```

#### Step B — 精灵表（Sprite Sheet）生成

若引擎需要精灵动画帧：

```
[TOOL_CALL] Call_AIGC_API(
  type      = "image",
  prompt    = "{特效名} animation sprite sheet, {帧数} frames, {行列描述}, transparent background, game VFX, pixel art / hand-drawn",
  platform  = "stable_diffusion",
  params    = {
    width  : {总宽度 = 单帧宽 × 列数},
    height : {总高度 = 单帧高 × 行数},
    format : "png"
  },
  output_id = "sprite_{特效名}_{行}x{列}"
)
```

#### Step C — 质量自检与归档

```
VFX 质量自检清单
☐ 透明背景处理正确（PNG Alpha 通道存在）
☐ 精灵帧对齐（每帧等宽等高）
☐ 分辨率符合平台规范（2 的幂次方推荐：64/128/256/512）
☐ 命名符合 6.3 节规范
全部通过 → 归档
任一未通过 → 重试或标注 [PLACEHOLDER]
```

```
[TOOL_CALL] Write_File(
  path    = "_workspace/assets/images/effects/{按 6.3 规范命名的文件名}",
  mode    = "binary",
  content = {图像二进制数据}
)
```

---

### 6.7 AIGC 降级与占位符规范

当 AIGC API 调用连续失败 **2 次** 时，执行降级处理：

```
[AIGC FALLBACK]
资产标识符：{output_id}
失败原因：{API 超时 / Prompt 审核不通过 / 配额不足}
降级处理：生成占位符描述文件

[TOOL_CALL] Write_File(
  path    = "_workspace/assets/{子目录}/{output_id}.placeholder.txt",
  mode    = "text",
  content = "
    [PLACEHOLDER]
    资产名称：{output_id}
    预期规格：{宽}x{高}px | {格式} | {描述}
    推荐 Prompt：{最佳 Prompt 版本}
    推荐平台：{platform}
    人工生成建议：{具体操作步骤}
  "
)
```

---

## 七、代码编写执行协议（v1.1 新增，DEV Agent 专属）

> **强制适用**：DEV Agent（研发主程 Agent）。
> 本节定义了从工程搭建到代码集成的完整工具调用规范。

---

### 7.1 工程目录搭建规范

DEV Agent 在激活后，首先根据 GATE-P 锁定的平台与引擎，搭建标准工程目录。

#### 7.1.1 目录搭建工具调用格式

```
[TOOL_CALL] Create_Directory(
  path = "_workspace/project/{平台缩写}_{项目名}/"
)
```

#### 7.1.2 各平台标准目录结构

**微信小游戏（Cocos Creator 3.x）**：
```
_workspace/project/wx_{项目名}/
├── assets/
│   ├── scenes/         # 场景文件
│   ├── scripts/        # TypeScript 脚本
│   ├── resources/      # 动态加载资源
│   │   ├── images/     # 链接自 _workspace/assets/images/
│   │   └── audio/      # 链接自 _workspace/assets/audio/
│   └── prefabs/        # 预制体
├── game.ts             # 入口脚本
└── project.json        # Cocos 项目配置
```

**Web H5（Phaser.js）**：
```
_workspace/project/web_{项目名}/
├── src/
│   ├── scenes/         # 场景类
│   ├── objects/        # 游戏对象
│   ├── utils/          # 工具函数
│   └── main.ts         # 入口文件
├── public/
│   ├── assets/         # 链接自 _workspace/assets/
│   └── index.html
├── package.json
└── vite.config.ts      # 或 webpack.config.js
```

**PC（Godot 4.x，GDScript）**：
```
_workspace/project/godot_{项目名}/
├── scenes/             # .tscn 场景文件
├── scripts/            # .gd 脚本文件
├── assets/             # 链接自 _workspace/assets/
│   ├── images/
│   └── audio/
├── autoload/           # 全局单例脚本
└── project.godot       # 项目配置
```

**移动端（Unity）**：
```
_workspace/project/unity_{项目名}/
├── Assets/
│   ├── Scenes/
│   ├── Scripts/
│   ├── Resources/
│   │   ├── Images/     # 链接自 _workspace/assets/images/
│   │   └── Audio/      # 链接自 _workspace/assets/audio/
│   └── Prefabs/
└── ProjectSettings/
```

---

### 7.2 代码写入工具调用格式

```
[TOOL_CALL] Write_File(
  path    = "_workspace/project/{平台目录}/{相对路径}/{文件名}.{扩展名}",
  mode    = "text",
  content = """
    {实际代码内容}
  """
)
```

**规范约束**：
- 每次 `Write_File` 仅写入**单一模块**（不得将多个功能混写在一个调用中）
- 写入前必须声明该模块的**功能描述**和**依赖关系**
- 代码中必须包含**平台版本注释**，格式：`// @platform: {平台} | @engine: {引擎及版本} | @author: DEV Agent`

---

### 7.3 模块化编写规范

DEV Agent 按以下模块顺序编写代码，每个模块对应独立的 Write_File 调用：

| 编写顺序 | 模块名 | 描述 | 优先级 |
|---------|-------|------|-------|
| 1 | `GameConfig` | 全局配置（分辨率、帧率、平台参数） | 🔴 必须 |
| 2 | `AssetLoader` | 资源预加载（加载 `_workspace/assets/` 下的图像/音频） | 🔴 必须 |
| 3 | `GameManager` | 游戏状态机（加载/主菜单/游戏中/暂停/游戏结束） | 🔴 必须 |
| 4 | `CoreLoop` | 核心游戏循环（输入处理、Update、碰撞） | 🔴 必须 |
| 5 | `UIManager` | UI 管理（HUD、菜单、弹窗） | 🟡 推荐 |
| 6 | `AudioManager` | 音频管理（BGM 播放控制、SFX 触发） | 🟡 推荐 |
| 7 | `SaveManager` | 存档管理（本地存储/云存档，按平台差异化实现） | 🟢 可选 |
| 8 | `{功能模块}` | 游戏特有功能（战斗系统、关卡管理等，按 GDD 决定） | 🟢 可选 |

---

### 7.4 资源集成规范

DEV Agent 在代码中引用 `_workspace/assets/` 资源时，必须遵循各平台的资源加载 API：

```
// 资源集成声明格式（代码注释）
// [ASSET_REF] {资产标识符} → {平台相对路径}
// 示例（Cocos Creator）：
// [ASSET_REF] char_hero_idle_01.png → assets/resources/images/characters/char_hero_idle_01
```

各平台资源加载伪代码：

**Cocos Creator（TypeScript）**：
```typescript
// @platform: 微信小游戏 | @engine: Cocos Creator 3.x
resources.load('images/characters/char_hero_idle_01', SpriteFrame, (err, spriteFrame) => {
  if (!err) this.heroSprite.spriteFrame = spriteFrame;
});
```

**Phaser.js**：
```javascript
// @platform: Web H5 | @engine: Phaser 3.x
this.load.image('char_hero_idle', 'assets/images/characters/char_hero_idle_01.png');
```

**Godot 4.x（GDScript）**：
```gdscript
# @platform: PC | @engine: Godot 4.x
var texture = load("res://assets/images/characters/char_hero_idle_01.png")
$Sprite2D.texture = texture
```

---

### 7.5 错误修复机制

当 DEV Agent 遇到代码错误、API 不兼容或引擎版本差异时，执行以下自修复流程：

```
[ERROR_FIX 流程]

Step 1 — 错误诊断
  错误类型：{编译错误 / 运行时错误 / API 废弃 / 版本不兼容}
  错误信息：{完整错误内容}
  涉及文件：{文件路径}

Step 2 — Web Search 查阅文档
[TOOL_CALL] Web_Search(
  query         = "{引擎名} {版本} {错误关键词} official documentation",
  domain_filter = "{引擎官方文档域，如 docs.godotengine.org / creator.cocos.com / docs.phaser.io}"
)

Step 3 — 应用修复
  修复方案：{基于官方文档的修复代码}
  修复说明：{一句话解释变更原因}

[TOOL_CALL] Write_File(
  path    = "{原文件路径}",
  mode    = "text",
  content = "{修复后的完整代码}"
)

Step 4 — 修复记录
[TOOL_CALL] Write_File(
  path    = "_workspace/project/fix_log.md",
  mode    = "append",
  content = "
    ## Fix #{序号} — {日期}
    - 错误类型：{类型}
    - 错误描述：{描述}
    - 参考文档：{URL}
    - 修复方案：{简述}
  "
)
```

**自修复限制**：同一错误最多自动重试 **3 次**；第 3 次失败后，输出人工介入请求：

```
[DEV ERROR — 人工介入请求]
文件：{路径}
错误：{详细描述}
已尝试方案：{3 次尝试的摘要}
建议人工操作：{具体步骤}
```

---

### 7.6 代码编写完成自检清单

```
DEV Agent 代码交付自检清单
☐ 所有必须模块（GameConfig/AssetLoader/GameManager/CoreLoop）已完成
☐ 所有 Write_File 调用已执行（代码实际写入，非描述性输出）
☐ 所有资源引用路径与 _workspace/assets/ 实际归档路径一致
☐ 代码包含平台版本注释（@platform / @engine）
☐ 项目目录结构符合 7.1 节平台规范
☐ build_guide.md 已生成（见第八节）
☐ fix_log.md 已创建（即使无错误也创建空文件）
全部通过 → 通知 Master Agent 代码编写完成
任一未通过 → 自行补充后再汇报
```

---

## 八、交付指南生成规范（DEV Agent 执行）

DEV Agent 在完成代码编写后，必须生成 `_workspace/build_guide.md`，格式如下：

```markdown
# 《本地运行与编译指南》

> 由 DEV Agent 自动生成 | 目标平台：{平台} | 引擎：{引擎及版本}

## 环境要求

| 工具 | 版本要求 | 下载链接 |
|-----|---------|---------|
| {引擎/IDE 名} | {版本} | {官方下载链接} |
| {依赖工具 1} | {版本} | {链接} |
| ...           |         |         |

## 导入步骤

1. {步骤 1 — 具体操作，含截图描述或菜单路径}
2. {步骤 2}
3. ...

## 首次运行

\`\`\`bash
{运行命令，如适用}
\`\`\`

或图形化操作：{具体按钮/菜单路径}

## 资源路径说明

所有游戏资产位于 `_workspace/assets/`，已被工程代码以相对路径引用，无需手动迁移。

## 打包/发布

{按目标平台提供打包步骤}

## 已知问题

{DEV Agent 遇到但未能完全解决的已知问题，含建议解决方案}
```

---

## 附录：Agent 自检清单（v1.1 更新版）

每个 Sub-Agent 在提交文档前，执行自检：

```
SSP 自检清单（通用）
☐ Step 1 已执行（工具调用记录存在）
☐ 检索词未超出 Ticket 授权域
☐ Step 2 摘要已完成（≤300字，含来源URL）
☐ 正文存在 [来源: XXX] 显式引用
☐ 所有关键结论已标注置信度（High/Medium/Low）
☐ 所有 Low 置信度条目已附"待验证假设"清单
☐ 文档末尾存在"参考依据"章节（含 URL 表格）
☐ 文档头部包含 SSP 执行状态声明
全部通过 → 提交 Master Agent 审核
任一未通过 → 自行补充后再提交

ART / AUDIO / VFX Agent 附加自检（第六节）
☐ AIGC API 调用已执行（Call_AIGC_API 工具调用记录存在）
☐ 所有资产已归档至 _workspace/assets/（Write_File 记录存在）
☐ 资产命名符合 6.3 节规范
☐ 归档清单已输出（含文件名、尺寸/时长、格式、大小）
☐ 无法生成的资产已生成 .placeholder.txt
全部通过 → 提交 Master Agent
任一未通过 → 补充后再提交

DEV Agent 附加自检（第七节）
☐ 工程目录已按 7.1 节平台规范搭建
☐ 所有必须模块代码已通过 Write_File 写入
☐ 资源路径与 _workspace/assets/ 一致
☐ build_guide.md 已生成（第八节格式）
☐ fix_log.md 已创建
全部通过 → 通知 Master Agent 交付完成
任一未通过 → 补充后再汇报


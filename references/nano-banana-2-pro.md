# Nano Banana 2 / Pro 官方提示词写法

> 来源：① Google DeepMind 官方 Prompt Guide（deepmind.google/models/gemini-image/prompt-guide）；② Google 官方博客 Start building with Nano Banana 2 Lite（2026-06，家族定位）；③ fal.ai《Nano Banana Pro Prompting Guide》（2026-06，John Ozuysal）。本文件是离线参考，无需再联网。

## 模型家族（官方口径）

| 型号 | 底层模型 | 定位 |
|---|---|---|
| Nano Banana 2 Lite | Gemini 3.1 Flash Lite Image | 最快最省：~4 秒出图、$0.034/1K 图，快速草稿与高吞吐管线 |
| Nano Banana 2 | Gemini 3.1 Flash Image | 均衡主力：高质量 + 低延迟 |
| Nano Banana Pro | Gemini 3 Pro Image | 复杂专业任务：最强控制力与生成前推理 |
| Nano Banana（一代） | Gemini 2.5 Flash Image | 遗留型号，官方建议升级到 2 Lite |

关键能力：最多 14 张参考图（社区经验：高保真 ≤6 张更稳）；Pro 可在生成与编辑间保持最多 5 人容貌一致；原生 1K–4K；图内文字渲染强；输出带 SynthID 水印。Pro 生成前会先推理构图与布光（Thinking），所以提示词越像"给艺术总监的简报"效果越好。

## 生成 · 八要素模板

写成连贯段落（不是标签清单），要素顺序：

1. **Subject 主体** —— 谁 / 什么，具体到材质与外观
2. **Action 动作** —— 在做什么
3. **Setting 场景** —— 地点、时间、环境光线
4. **Style 风格** —— 成品风格（photoreal / 水彩 / 扁平插画…）
5. **Composition & camera 构图与镜头** —— 画幅、角度、焦段、景深，像对职业摄影师下指令
6. **Lighting & color 光线与色彩** —— 布光方案 + 调色方向
7. **Text 图内文字** —— 引号 + 字体 + 位置
8. **Constraints 约束** —— "no X, no X, no X" 收尾

### 官方示范（产品摄影，fal 指南原文）

A matte-black stainless steel insulated water bottle, 750ml, with a slim cylindrical body and a brushed metal cap. The bottle stands upright on a wet slate countertop, a few condensation droplets sliding down its side, mid-morning. In a quiet kitchen by a north-facing window, soft overcast daylight coming from the left. Photoreal, high-end commercial product photography, the kind you'd see on a premium DTC brand's landing page. Tight three-quarter hero shot, slightly below eye level looking up to make the bottle feel tall and substantial, shot on an 85mm lens at f/8 so the whole product stays sharp while the background falls into soft blur. Lit with one large softbox from the left and a subtle reflector on the right to control the falloff; cool, desaturated grade with clean neutral whites and a faint blue cast in the shadows. The words "STAY COLD. 24 HOURS." set in small caps along the lower third. No other props, no hands, no visible brand logos, no harsh specular hotspots on the metal.

中文提示词同理：一段话覆盖全部要素，图内文字保留原文并加引号。

## 编辑 · 先锁后改（Lock → Change → Amount → Constraints）

Google 官方口径：自然对话式编辑，五类操作——换角色 / 调构图 / 改动作 / 换场景 / 换风格。复杂编辑用 fal 四步更稳：

1. **Lock** —— 先点名必须不动的：脸、布局、图内文字、色调
2. **Change** —— 这一次只改一件事
3. **Amount** —— 改到什么程度（整体换环境 / 轻调）
4. **Constraints** —— 不许破坏什么

示范（换背景，fal 原文）：

Lock: the water bottle: matte-black finish, brushed cap, slim cylindrical body, the "STAY COLD. 24 HOURS." text, the condensation droplets, its size and position in frame, and the three-quarter hero angle. Change: swap the kitchen-counter background for a flat grey boulder beside a sunlit mountain hiking trail. Amount: full environment swap, understated: soft natural daylight, not golden hour; bottle looks shot on location, not composited. Constraints: don't relight or recolor the bottle beyond the new ambient light; no new reflections or hotspots on the metal; keep the original cool grade; don't touch the cap, droplets, or text; no hands, people, or gear in frame.

铁律：**一次只改一件事**；每一轮都重复锁定清单，否则画面会漂移。

## Google 官方通用技巧（Prompt Guide 全量）

- **文字渲染**：目标文字加引号（"Happy Birthday"），描述字体风格（bold sans-serif / neon cursive signage）
- **生产规格**：画幅（4:3 / 1:1 / 9:16）与用途形态（widescreen backdrop / vertical social post）写进提示词；用产品内选项升到 2K / 4K
- **多方案**：直接要 "three distinct variations of a product mock up" / "four different color palettes"
- **主体一致性**：上传清晰参考图，并给每个角色 / 物体起名（MAYA、BOLT），后文用名字锁定
- **真实世界知识**：点名真实概念 / 地点 / 历史年代，可画时间线、流程图
- **翻译本地化**：指定目标语言 + 区域文化线索；关键外文措辞给引号原文
- **用途上下文**：一句 "this is for a high-end cookbook" 能把推理引向浅景深与精致摆盘

## Anti-slop 规则（fal）

- 形容词不渲染：stunning → 阴天光线 + 掉漆细节
- 风格词钉死在具体物上：cinematic 会漂，"青橙调色 + 硬阴影"不会
- 说出真名：需要登机牌就写 boarding pass，胜过一切氛围词
- 图内文字引号 + 字体 + 位置；漏字母时逐字拼写
- 停止描述"什么不变"的那一刻，编辑就开始漂移
- 告诉模型图给谁看、用在哪

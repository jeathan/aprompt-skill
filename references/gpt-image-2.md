# GPT Image 2 官方提示词写法

> 来源：① OpenAI 官方 Image Prompting 指南（platform.openai.com/docs/guides/image-prompting，方法覆盖 GPT Image 2 与当前 2.5 系列）；② OpenAI Cookbook 官方最佳实践（2026-04，经 Luma AI 完整指南转述）。本文件是离线参考，无需再联网。

## 模型档案

- GPT Image 2：OpenAI 第二代图像模型，首个集成 O 系推理——**生成前先规划结构**，复杂构图 / 多元素场景一次成型
- 强项：图内文字（拉丁 / CJK / 阿拉伯 / 印度系 95%+ 准确率）、多参考合成、高保真写实、世界知识、信息图与 UI 等结构化视觉
- 当前线上型号：gpt-image-2.5-flare（快）与 gpt-image-2.5-sunburst（质量）；提示词方法论与 GPT Image 2 完全一致
- GPT Image 2 规格：画幅 3:1 / 21:9 / 16:9 / 4:3 / 3:2 / 1:1 / 2:3 / 3:4 / 9:16 / 1:3；分辨率 1K / 2K（默认）/ 4K；质量 low / medium / high；提示词上限 7000 字符；参考图常见上限 4 张；**不支持透明背景**（透明需求用 1.5 或 2.5 系列，2.5 走 API background="transparent"）
- 短板：已有品牌 logo 还原不稳定；4K + high 慢（3 分钟+）；连续编辑会掉质量（编辑链之间先 upscale）

## 官方提示词八原则

1. **定义结果** —— 点名主体与用途（产品照 / 广告 / 图表），说明构图、画幅、关键位置；复杂请求用 scene / subject / details / constraints 带标签分节
2. **选可维护格式** —— 短句、描述段、类 JSON、指令、标签皆可，哪种最好读好改用哪种，不依赖特殊语法
3. **描述可见细节** —— 材质、光线、颜色、媒介；要写实必须明说 "photorealistic"；相机参数是视觉线索不是物理承诺；大场面 / 低光 / 雨夜 / 霓虹给尺度、氛围、色彩，不给氛围词
4. **人物与动作** —— 身体取景、相对比例、视线、与物体互动："full body visible, feet included" / "hands naturally gripping the handlebars"
5. **精确文字** —— 引号 + 位置 + 字体；生僻词 / 品牌名逐字拼写；要求无多余文字并检查拼写；小字用 medium / high 质量
6. **改动与约束分开** —— 编辑写 "Change only X"，列明要保持的（身份 / 几何 / 布局 / 光线 / 标签）与排除项
7. **给参考图派角色** —— 每张输入图按编号 + 用途（subject / style / clothing / background）说明如何组合
8. **刻意迭代** —— 上一步输出作下一步输入，一次一改，重复保持约束

## 生成 · 五段模板

Scene: 场景、时间、背景、环境
Subject: 主体
Important details: 材质、服装、纹理、光线、镜头、构图、情绪
Use case: 用途（编辑摄影 / 产品图 / 海报 / UI / 信息图）
Constraints: no watermark / no logos / no extra text / 保持什么

- 短提示（≤2 句）→ 合成单段
- 中等 → 五段各一行
- 复杂 → 五段 + 段间换行
- 顺序永远是 **背景 → 主体 → 细节 → 约束**（宽 → 窄）

### 官方示范（写实人像）

Create a photorealistic candid photograph of an elderly sailor standing on a small fishing boat. He has weathered skin with visible wrinkles, pores, and sun texture, and a few faded traditional sailor tattoos on his arms. He is calmly adjusting a net while his dog sits nearby on the deck. Shot like a 35mm film photograph, medium close-up at eye level, using a 50mm lens. Soft coastal daylight, shallow depth of field, subtle film grain, natural color balance. The image should feel honest and unposed, with real skin texture, worn materials, and everyday detail. No glamorization, no heavy retouching.

### 官方示范（图内文字）

Give me a cool in culture ad / fashion shot for a brand called Thread. ... The ad shows a group of friends hanging out together with the tagline "Yours to Create." ... Render the tagline exactly once, clearly and legibly, integrated into the ad layout. No extra text, no watermarks, no unrelated logos.

## 编辑 · 直接命令式

短、具体、无修饰语。写明 Change / Keep：

- ❌ Transform this beautiful image by artistically changing the background to create a more dramatic atmosphere
- ✅ Change background to sunset beach. Keep subject unchanged.

官方示范：

- 删物：Remove the flower from man's hand. Do not change anything else.
- 换材质：In this room photo, replace ONLY the white chairs with chairs made of wood. Preserve camera angle, room lighting, floor shadows, and surrounding objects.
- 翻译保版式：Translate the text in the infographic to Spanish. Do not change any other aspect of the image.
- 保留换装（4 参考图）：Edit the image to dress the woman using the provided clothing images. Do not change her face, facial features, skin tone, body shape, pose, or identity in any way. Preserve her exact likeness, expression, hairstyle, and proportions. Replace only the clothing, fitting the garments naturally to her existing pose and body geometry with realistic fabric behavior. Match lighting, shadows, and color temperature to the original photo so the outfit integrates photorealistically. Do not change the background, camera angle, framing, or image quality, and do not add accessories, text, logos, or watermarks.
- 素描变实拍：Turn this drawing into a photorealistic image. Preserve the exact layout, proportions, and perspective. Choose realistic materials and lighting consistent with the sketch intent. Do not add new elements or text.

## 多参考图格式

Use Image 1 (brief description) as [TYPE] reference.
Use Image 2 (brief description) as [TYPE] reference.
[Action instruction.]

TYPE：style / character / pose / composition / background reference。

例：Use Image 1 (man in suit) as character reference. Use Image 2 (neon city) as background/style reference. Place character in scene with cinematic rim lighting.

## Anti-slop 对照（Cookbook 口径）

| ❌ 别写 | ✅ 换成 |
|---|---|
| stunning | overcast daylight, shallow depth of field |
| epic | low-angle shot, wide 24mm lens |
| beautiful lighting | golden hour side-lighting, soft shadows |
| high quality | 8K texture detail, film grain ISO 400 |
| professional | studio three-point lighting, seamless white backdrop |
| cinematic | anamorphic 2.39:1, teal-and-orange grade, lens flare |
| realistic | shot on Canon R5, 85mm f/1.4, natural window light |
| vibrant colors | saturated Kodak Ektar palette, reds at +20 |

原理：推理引擎响应**具体视觉参数**，不响应氛围。

## 迭代策略（官方）

- 概念期用 low 质量迭代措辞 3–5 次 → 定稿换 medium / high
- 编辑链之间 upscale，防质量衰减
- 多轮编辑一次只改一个条件（官方示例：出图后 "Make it look like a winter evening with snowfall."）
- 角色一致性：先出角色定妆图（写全外貌约束），后续每轮编辑重复 "Same facial features, proportions, and color palette"

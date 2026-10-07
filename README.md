**简体中文** | [English](README.en.md)

# A+Prompt · 双引擎图片提示词优化 Skill

把一条图片生成提示词（或图片修改需求）优化成**两份**可直接复制使用的成品提示词：

- **Nano Banana 2 / Pro 版** —— 按 Google DeepMind 官方 Prompt Guide 写法
- **GPT Image 2 版** —— 按 OpenAI 官方 Image Prompting 指南写法

目标画面一致，写法各随其主。适用于 Claude / ChatGPT / DSH 等支持 skill（或自定义指令）的 Agent 环境。

## 它解决什么问题

大多数人写图生图提示词时，是把要素清单平均铺开——主体一句、场景一句、风格一句、光线一句，结果每份提示词都长得一样、也都不够好。

本 skill 强制两件事：

1. **先排主次，再提维度**：每个信息维度先定级（A 详写 / B 带过 / C 省略），用户点名的、排在最前面的、画面占比大的才展开写，其余一句话带过或直接不写。
2. **按各家的官方规范分别成文**：Nano Banana Pro 生成前会先做构图与布光推理，吃"给艺术总监的口头简报"；GPT Image 2 走 O 系推理，吃"分层结构 + 直接命令"。同一画面，两种写法。

另外还带一套 **Anti-slop 检查**：把 stunning / cinematic / masterpiece / 高清 这类不渲染的空洞形容词，替换成阴天柔光、2.39:1 变形宽银幕、ISO 400 胶片颗粒这类**具体视觉参数**。

## 特性

- 覆盖四类任务：文生图、图片编辑、带参考图生成、多图合成编辑
- 生成与编辑两套写法，编辑版强制"先锁后改 + 一次只改一件事"
- 硬性篇幅约束：每份提示词 ≤2000 字符，超限按 C 级 → B 级维度顺序砍
- 编辑版锁定清单精简规则：只锁被编辑对象的身份要素 + 与改动直接关联的元素
- 内置两个模型的规格速查（参考图数量、分辨率、透明背景支持等）

## 安装

把这个仓库放进你的 skills 目录即可。以 DSH 为例：

```bash
git clone https://github.com/jeathan/aprompt-skill.git
# 然后复制到 DSH 的 skills 目录
cp -r aprompt-skill ~/.dsh/skills/aprompt
```

Windows PowerShell：

```powershell
git clone https://github.com/jeathan/aprompt-skill.git
Copy-Item -Recurse .\aprompt-skill "$env:USERPROFILE\.dsh\skills\aprompt"
```

Claude Code 用户可放到 `~/.claude/skills/aprompt/`。

> 目录名建议就用 `aprompt`，与 `SKILL.md` 里的 `name` 字段保持一致。

## 用法

在对话里粘贴一条提示词并说"优化一下"，或直接输入 `/aprompt`，或上传图片 + 修改要求。

输出结构固定为：

1. `Nano Banana 2 / Pro 版` —— 代码块
2. `GPT Image 2 版` —— 代码块
3. 2–4 句说明：改了什么、主次怎么定的、两份为什么这么不同

## 仓库结构

```
.
├── SKILL.md                        # skill 主入口：工作流、两套写法、Anti-slop、输出结构
├── references/
│   ├── nano-banana-2-pro.md        # Google DeepMind 官方写法 + fal.ai 指南整理（离线参考）
│   └── gpt-image-2.md              # OpenAI 官方 Image Prompting 指南整理（离线参考）
├── README.md                       # 简体中文（本文件）
├── README.en.md                    # English
└── LICENSE
```

`references/` 里的两份文件是离线参考，Agent 执行时会读取，不需要联网。

## 致谢与来源

参考文件的内容整理自以下公开资料：

- Google DeepMind — Gemini Image Prompt Guide
- Google 官方博客 — Start building with Nano Banana 2 Lite（2026-06）
- fal.ai — Nano Banana Pro Prompting Guide（John Ozuysal，2026-06）
- OpenAI — Image Prompting 指南（platform.openai.com/docs/guides/image-prompting）
- OpenAI Cookbook — 图像生成最佳实践（2026-04）

模型规格与官方口径会随厂商更新变化，本仓库内容反映的是整理时点的版本。

## License

MIT

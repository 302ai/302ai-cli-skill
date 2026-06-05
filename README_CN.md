# 302.AI CLI Skill

[English](./README.md) | 中文文档


🚀 **为您的 AI 助手赋予多模态生成能力** - 一个 AI 编码助手技能，通过 `302ai` 命令行工具实现图片、视频、语音、音效、3D 模型、音乐歌曲和网络搜索功能，由 [302.AI 官方网站](https://302.ai/) 提供支持。


> 💡 **跨工具兼容** - 此 Skill 使用标准 SKILL.md 格式，支持 Claude Code、Cursor 及其他兼容的 AI 编码工具

---
## 🚀 快速开始
对你的Agent说：
```bash
安装这个skill：https://github.com/302ai/302ai-cli-skill
```

---

## ✨ 这是什么？

一个强大的 AI 编码技能，让您的 AI 助手能够理解自然语言请求，如*"生成一只水彩风格的猫"*，并自动将其转换为正确的 `302ai` CLI 命令——向 302.AI 云服务发送请求并返回结果。

---

## 🎯 核心特性

- 🎨 **多模态生成** - 图片、视频、语音、音效、3D 模型和音乐
- 🔍 **网络搜索** - 多个搜索提供商（Tavily、Bocha、Exa、Metaso）
- 🤖 **智能命令转换** - 自然语言自动转换为 CLI 命令
- ⚡ **异步和同步模式** - 灵活的工作流程适应不同场景
- 🌐 **1400+ AI 模型** - 访问 302.AI 的全面模型生态系统
- 🛠️ **模型管理** - 轻松列出、设置默认值、查询参数

---

## 📦 包含内容

### 模块概览

| 模块            | 描述                | 模式                              | 输出格式                 |
| ------------- | ----------------- | ------------------------------- | -------------------- |
| 🎨 **Image**  | 文本生成图片 / 图片生成图片   | 异步（推荐）或同步                       | PNG / WEBP / JPG URL |
| 🎬 **Video**  | 文本生成视频 / 图片生成视频   | 仅异步                             | MP4 URL              |
| 🗣️ **TTS**   | 文本转语音             | 异步                              | MP3 / WAV URL        |
| 📝 **STT**    | 语音转文本（转录）         | 同步                              | JSON 文本              |
| 🔊 **SFX**    | 文本生成音效            | 异步                              | MP3 URL              |
| 🧊 **3D**     | 文本生成 3D / 图片生成 3D | 异步                              | GLB 文件 URL           |
| 🎵 **Song**   | AI 音乐/歌曲生成 + 歌词创作 | 异步（Suno）或同步（Minimax/ElevenLabs） | MP3 URL              |
| 🔍 **Search** | 网络搜索（多个提供商）       | 同步                              | JSON 结果              |

---

### 🎯 各模块支持的模型

| 模块 | 支持的模型 |
|------|-----------|
| 🎨 **Image** | `gemini-3.1-flash-image-preview` • `gpt-image-2-t2i` • `gpt-image-2-i2i` • `doubao-seedream-5-0-260128` • `gemini-3-pro-image` • `gemini-2.5-flash-image` • `gpt-image-1.5-t2i` • `gpt-image-1.5-i2i` • `wan2.7-image` • `wan2.7-image-pro` |
| 🎬 **Video** | `happyhorse-1.0-t2v` • `happyhorse-1.0-i2v` • `happyhorse-1.0-r2v` • `kling-o3` • `doubao-seedance-2-0-260128` • `wan2.7-t2v` • `wan2.7-i2v` • `wan2.7-r2v` • `official-kling-v3` • `runway-gen4` • `minimaxi-hailuo-02` • `google-veo3.1` • `google-veo3.1-pro` • `minimaxi-hailuo-2.3` • `viduq3-pro` |
| 🗣️ **TTS** | `tts-1` • `tts-1-hd` • `gpt-4o-mini-tts` • `doubao-tts` • `minimax-tts` • `google-tts` • `owen-tts` • `meitan-tts` • `mureka-tts` • `fish-audio-tts` • `elevenlabs-tts` |
| 📝 **STT** | `whisper-1` • `gpt-4o-transcribe` • `gpt-4o-mini-transcribe` • `gpt-4o-transcribe-diarize` • `recognize` • `scribe_v1_experimental` • `scribe_v1` • `sensevoice` |
| 🔊 **SFX** | `kling-sfx` |
| 🧊 **3D** | `hyper3d-rodin` |
| 🎵 **Song** | `chirp-fenix@suno` • `chirp-crow@suno` • `chirp-bluejay@suno` • `chirp-auk@suno` • `chirp-v4@suno` • `chirp-v3-5@suno` • `music-2.5+@minimax` • `music-2.5@minimax` • `music-2.0@minimax` • `music-1.5@minimax` • `music_v1@elevenlabs` |
| 🔍 **Search** | `tavily` • `search1_search` • `search1_news` • `bocha` • `exa` • `firecrawl` • `metaso` • `unifuncs` • `perplexity` |

> **💡 提示：** 
> - 以上为各模块支持的主要模型，完整列表请运行 `302ai model list <type>` 查看
> - 不同模型的速度、质量、价格各有差异，请根据实际需求选择。价格请查看[官方网站](https://302.ai/)
---


## 💡 使用示例

### 示例 1：生成图片

**您的提问：**
```
生成一幅山脉日落的水彩画
```

**您的 AI 助手执行：**
```bash
# 异步模式（推荐）- 避免超时
302ai image create --prompt "watercolor painting of a sunset over mountains" --model flux-1.1-pro
302ai image fetch <taskid> --short
```

**结果：** 返回生成图片的 URL

### 示例 2：创建视频

**您的提问：**
```
制作一个猫玩毛线的视频
```

**您的 AI 助手执行：**
```bash
# 视频生成需要 1-5 分钟，仅支持异步模式
302ai video create --prompt "a cat playing with yarn" --model kling-v1.6-standard
302ai video fetch <taskid> --short
```

**结果：** 返回生成视频的 URL

### 示例 3：文本转语音

**您的提问：**
```
用女声朗读这段文字："欢迎来到 302.AI"
```

**您的 AI 助手执行：**
```bash
# 首次使用 TTS 需要先刷新缓存
302ai tts refresh

# 然后创建语音任务
302ai tts create --text "Welcome to 302.AI" --provider openai --voice alloy --model tts-1
302ai tts fetch <taskid> --short
```

**结果：** 返回音频文件的 URL

> ⚠️ **重要提示：** TTS 模块首次使用前必须运行 `302ai tts refresh` 来缓存提供商和语音列表。

### 示例 4：网络搜索

**您的提问：**
```
搜索有关 AI 发展的最新新闻
```

**您的 AI 助手执行：**
```bash
# 默认使用 Tavily 提供商（适合英文内容）
302ai search run --query "latest AI developments" --provider tavily
```

**结果：** 返回 JSON 格式的搜索结果

> 💡 **搜索提示：**
> - 默认提供商：`tavily`（适合英文内容）
> - 中文内容：使用 `--provider bocha` 获得更好结果
> - 学术搜索：使用 `--provider metaso --category scholar` 或 `--provider exa --category "research paper"`

### 示例 5：生成 3D 模型

**您的提问：**
```
创建一个咖啡杯的 3D 模型
```

**您的 AI 助手执行：**
```bash
# 3D 模型生成需要 1-5 分钟
302ai 3d create --prompt "a coffee mug" --model hyper3d-rodin
302ai 3d fetch <taskid> --short
```

**结果：** 返回 `.glb` 文件的 URL

> 📦 **3D 文件查看：** `.glb` 格式可以使用 [Blender](https://www.blender.org/)（免费）、[Three.js 在线查看器](https://gltf-viewer.donmccurdy.com/) 或任何支持 glTF 格式的 3D 模型查看器打开。

### 示例 6：生成歌曲（Suno - 异步）

**您的提问：**
```
创作一首关于夏日假期的歌曲
```

**您的 AI 助手执行：**
```bash
# Suno 每次生成 2 首歌曲，需要 1-3 分钟
302ai song create --prompt "a cheerful song about summer vacation at the beach" --provider suno
302ai song fetch <taskid> --short
```

**结果：** 返回 2 个音频 URL（Suno 每次请求生成 2 个变体）

> 💡 **歌曲生成提示：**
> - **Suno（异步）**：最佳质量，每次生成 2 首歌曲，支持自定义歌词
> - **Minimax（同步）**：更快，需要 `--lyrics` 参数
> - **ElevenLabs（同步）**：需要 `--composition-plan` 而不是歌词

### 示例 7：生成歌词

**您的提问：**
```
为一首关于自由的摇滚歌曲写歌词
```

**您的 AI 助手执行：**
```bash
# 生成歌词（兼容 Suno 和 Minimax）
302ai song lyrics --prompt "rock song about freedom and breaking chains" --provider suno
```

**结果：** 返回 JSON 格式的歌词

> 📝 **歌词使用：** 生成的歌词可以传递给 `song create --lyrics` 或 `song generate --lyrics` 进行音乐生成。

### 示例 8：使用自定义歌词生成音乐（Minimax - 同步）

**您的提问：**
```
为这些歌词生成音乐：[您的歌词]
```

**您的 AI 助手执行：**
```bash
# Minimax 同步模式 - 立即返回音频 URL
302ai song generate --lyrics "您的主歌在这里\n副歌在这里\n..." --provider minimax --model speech-01-turbo
```

**结果：** 返回单个音频 URL

---

## 📚 安装指南

本 Skill 适用于所有支持 `SKILL.md` 格式的 AI 编码工具。

### 命令行安装（推荐）

如果您的工具支持 `claude skill` 命令（例如 **Claude Code**、**Cursor**）：

```bash
claude skill install github:302ai/302ai-cli-skill/.claude/skills/302ai-cli
```

### 手动安装

对于其他支持 `SKILL.md` 的工具：

1. **克隆302ai-cli-skill仓库或下载 302ai-cli-skill ZIP：**
   ```bash
   git clone https://github.com/302ai/302ai-cli-skill.git
   ```
   或在 GitHub 上的 [302ai/302ai-cli-skill](https://github.com/302ai/302ai-cli-skill) 仓库点击 **Code** → **Download ZIP**，然后解压文件。

2. 将 302ai-cli-skill 文件夹拷贝到您工具的 `skills` 目录（查看您工具的文档以确定位置）
3. 重启您的工具

---

## 🔄 更新指南

当发布新功能（如 Song 模块）时，请按照以下步骤操作：

### 步骤 1：更新 Skill

```bash
# Claude Code
claude skill update github:302ai/302ai-cli-skill/.claude/skills/302ai-cli

# Cursor 或其他工具
# 重新下载 SKILL.md 并替换旧文件
```

### 步骤 2：升级 CLI 包

```bash
pip install cli_302ai==1.0.2b2 --upgrade
```

> ⚠️ **关键：** 两步都是必需的。仅更新 skill 不会启用 3D、搜索或 Song 等新模块。CLI 包版本必须与 skill 版本匹配。

---

## 🎛️ API Key 设置

### 方法 1：环境变量（推荐）

**macOS / Linux:**
```bash
# 添加到 ~/.bashrc 或 ~/.zshrc 以保持持久性
export AI302_KEY="your-api-key-here"
```

**Windows PowerShell:**
```powershell
# 添加到 PowerShell 配置文件以保持持久性
$env:AI302_KEY = "your-api-key-here"
```

**Windows CMD:**
```cmd
set AI302_KEY=your-api-key-here
```

### 方法 2：每个命令标志

```bash
302ai image create --prompt "a cat" --api_key "your-api-key-here"
```

---

## 🏗️ 项目结构

```
ai302-cli/
├── SKILL.md              # 主要技能说明
├── README.md             # 英文文档
├── README_CN.md          # 本文件
└── references/
    ├── image.md          # 图片生成命令
    ├── video.md          # 视频生成命令
    ├── tts.md            # 文本转语音命令
    ├── stt.md            # 语音转文本命令
    ├── sfx.md            # 音效命令
    ├── 3d.md             # 3D 模型生成
    ├── song.md           # 音乐/歌曲生成
    ├── search.md         # 网络搜索
    ├── model.md          # 模型管理
    └── record.md         # 账单查询
```

---

## 🎨 高级用法

### 使用多个模块

您的 AI 助手可以链式操作多个模块：

```
生成一张日落的图片，然后创建一个动画视频，并添加背景音乐
```

助手将：
1. 使用 `302ai image create` 生成图片
2. 使用该图片进行 `302ai video create --image`
3. 使用 `302ai song create` 生成音乐
4. 返回所有三个 URL

### 模型选择

如果您不指定模型，CLI 使用智能默认值：
- 图片：`flux-1.1-pro`（高质量）
- 视频：`kling-v1.6-standard`（平衡）
- TTS：提供商特定默认值
- 歌曲：`suno-v4`（Suno）或 `speech-01`（Minimax）

查看可用模型：
```bash
302ai model list t2i    # 图片模型
302ai model list t2v    # 视频模型
302ai model list tts    # TTS 模型
```

### 账单查询

跟踪您的使用情况和成本：
```bash
# 获取特定请求的账单信息
302ai record get <request_id>

# 请求 ID 在生成命令的 JSON 输出中
```

---

## 📚 参考文档

每个模块的详细命令选项、标志和示例位于 `references/` 文件夹中：

| 文件 | 内容 |
|------|------|
| `references/image.md` | 图片生成——t2i、i2i、同步和异步工作流、所有选项 |
| `references/video.md` | 视频生成——t2v、i2v、仅异步工作流 |
| `references/tts.md` | 文本转语音——提供商、语音、异步工作流 |
| `references/stt.md` | 语音转文本——同步转录、支持的格式 |
| `references/sfx.md` | 音效——异步生成、时长选项 |
| `references/3d.md` | 3D 模型生成——t23d、i23d、异步工作流 |
| `references/song.md` | 音乐/歌曲生成——Suno 异步、Minimax/ElevenLabs 同步、歌词生成 |
| `references/search.md` | 网络搜索——提供商（Tavily、Bocha、Exa）、类别过滤器 |
| `references/model.md` | 模型管理——列出、设置默认、获取、参数 |
| `references/record.md` | 账单查询——通过请求 ID 查看成本和使用情况 |

---

## 📊 更新日志

| 版本 | 日期 | 变更 |
|------|------|------|
| v1.0.2b2 | 2025-06 | 添加 Song/Music 模块，包名改为 cli_302ai |
| v1.0.1b2 | 2025-06 | 添加 3D 模块、搜索模块 |
| v1.0.1b1 | 2025-05 | 初始版本：Image、Video、TTS、STT、SFX |

---

## 📝 许可证

MIT

---

## 🔗 链接

- [302.AI 官方网站](https://302.ai/)
- [302.AI API 文档](https://doc.302.ai/)
- [GitHub 仓库](https://github.com/302ai/302ai-cli-skill)

---

## 💬 支持

- 📧 邮箱：support@302.ai
- 📖 完整文档：[doc.302.ai](https://doc.302.ai/)
- 💡 问题和反馈：[GitHub Issues](https://github.com/302ai/302ai-cli-skill/issues)

---

<div align="center">

**⭐ 如果您觉得有用，请为此仓库加星！**

为所有平台的开发者和 AI 爱好者用 ❤️ 制作

[快速开始](#快速开始) • [查看示例](#使用示例) • [阅读文档](#参考文档)

</div>

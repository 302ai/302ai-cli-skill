# 302.AI CLI Skill

> 🚀 **Empower your AI agent with multimodal generation** - An AI coding assistant skill that enables image, video, speech, sound effects, 3D models, music/songs, and web search through the `302ai` command-line tool powered by [302.AI Official Website](https://302.ai/).

English | [中文文档](./README_CN.md)

> 💡 **Cross-Tool Compatible** - This skill uses the standard SKILL.md format, supporting Claude Code, Cursor, and other compatible AI coding tools

---

## ✨ What is This?

A powerful AI coding skill that lets your AI assistant understand natural language requests like *"generate a watercolor image of a cat"* and automatically translate them into the correct `302ai` CLI commands — sending requests to 302.AI's cloud services and returning results to you.

---

## 🎯 Key Features

- 🎨 **Multi-Modal Generation** - Images, videos, speech, sound effects, 3D models, and music
- 🔍 **Web Search** - Multiple search providers (Tavily, Bocha, Exa, Metaso)
- 🤖 **Smart Command Translation** - Natural language to CLI commands automatically
- ⚡ **Async & Sync Modes** - Flexible workflows for different use cases
- 🌐 **1400+ AI Models** - Access to 302.AI's comprehensive model ecosystem
- 🛠️ **Model Management** - List, set defaults, query parameters easily

---

## 📦 What's Included?

### Modules Overview

| Module | Description | Mode | Output Format |
|--------|-------------|------|---------------|
| 🎨 **Image** | Text-to-image / Image-to-image | Async (preferred) or Sync | PNG / WEBP / JPG URL |
| 🎬 **Video** | Text-to-video / Image-to-video | Async only | MP4 URL |
| 🗣️ **TTS** | Text-to-speech | Async | MP3 / WAV URL |
| 📝 **STT** | Speech-to-text (transcription) | Sync | JSON text |
| 🔊 **SFX** | Sound effects from text | Async | MP3 URL |
| 🧊 **3D** | Text-to-3D / Image-to-3D | Async | GLB file URL |
| 🎵 **Song** | AI music/song generation + lyrics | Async (Suno) or Sync (Minimax/ElevenLabs) | MP3 URL |
| 🔍 **Search** | Web search (multiple providers) | Sync | JSON results |

---

## 🚀 Quick Start

### Prerequisites

1. **Python 3.10 or higher** - Ensure Python and pip are installed
2. **Get a 302.AI API key** - Visit [302.AI Official Website](https://302.ai/) to register and obtain your key from the management dashboard
3. **Install the CLI package:**
   ```bash
   pip install cli_302ai==1.0.2b2
   ```

4. **Set your API key:**
   
   **macOS / Linux:**
   ```bash
   export AI302_KEY="your-api-key-here"
   ```
   
   **Windows PowerShell:**
   ```powershell
   $env:AI302_KEY = "your-api-key-here"
   ```

> You can also pass `--api_key` to each command instead. See [API Key Setup](#api-key-setup) for details.

### Install the Skill

```bash
claude skill install github:302ai/302ai-cli-skill/.claude/skills/302ai-cli
```

> **For other AI coding tools:** Download `SKILL.md` and place it in your tool's skills directory. See [Installation Guide](#installation-guide) for details.

### Try It

Ask your AI assistant:
```
Generate an image of a cat in watercolor style
```

Your AI assistant will handle the rest — selecting the right module, building the command, and returning your result URL.

---

## 💡 Usage Examples

### Example 1: Generate an Image

**You ask:**
```
Generate a watercolor painting of a sunset over mountains
```

**Your AI assistant executes:**
```bash
# Async mode (recommended) - avoids timeout
302ai image create --prompt "watercolor painting of a sunset over mountains" --model flux-1.1-pro
302ai image fetch <taskid> --short
```

**Result:** Returns a URL to your generated image

### Example 2: Create a Video

**You ask:**
```
Make a video of a cat playing with yarn
```

**Your AI assistant executes:**
```bash
# Video generation takes 1-5 minutes, async only
302ai video create --prompt "a cat playing with yarn" --model kling-v1.6-standard
302ai video fetch <taskid> --short
```

**Result:** Returns a URL to your generated video

### Example 3: Text-to-Speech

**You ask:**
```
Read this text aloud in a female voice: "Welcome to 302.AI"
```

**Your AI assistant executes:**
```bash
# First-time TTS setup requires cache refresh
302ai tts refresh

# Then create the speech task
302ai tts create --text "Welcome to 302.AI" --provider openai --voice alloy --model tts-1
302ai tts fetch <taskid> --short
```

**Result:** Returns a URL to the audio file

> ⚠️ **Important:** TTS module requires running `302ai tts refresh` before first use to cache provider and voice lists.

### Example 4: Web Search

**You ask:**
```
Search for the latest news on AI developments
```

**Your AI assistant executes:**
```bash
# Default provider is Tavily (suitable for English content)
302ai search run --query "latest AI developments" --provider tavily
```

**Result:** Returns JSON with search results

> 💡 **Search Tips:**
> - Default provider: `tavily` (best for English content)
> - Chinese content: Use `--provider bocha` for better results
> - Academic search: Use `--provider metaso --category scholar` or `--provider exa --category "research paper"`

### Example 5: Generate 3D Model

**You ask:**
```
Create a 3D model of a coffee mug
```

**Your AI assistant executes:**
```bash
# 3D model generation takes 1-5 minutes
302ai 3d create --prompt "a coffee mug" --model hyper3d-rodin
302ai 3d fetch <taskid> --short
```

**Result:** Returns a URL to the `.glb` file

> 📦 **3D File Viewing:** `.glb` format can be opened with [Blender](https://www.blender.org/) (free), [Three.js online viewer](https://gltf-viewer.donmccurdy.com/), or any 3D model viewer supporting glTF format.

### Example 6: Generate a Song (Suno - Async)

**You ask:**
```
Create a song about summer vacation
```

**Your AI assistant executes:**
```bash
# Suno generates 2 songs per task, takes 1-3 minutes
302ai song create --prompt "a cheerful song about summer vacation at the beach" --provider suno
302ai song fetch <taskid> --short
```

**Result:** Returns 2 audio URLs (Suno generates 2 variations per request)

> 💡 **Song Generation Tips:**
> - **Suno (Async)**: Best quality, generates 2 songs per task, supports custom lyrics
> - **Minimax (Sync)**: Faster, requires `--lyrics` parameter
> - **ElevenLabs (Sync)**: Requires `--composition-plan` instead of lyrics

### Example 7: Generate Lyrics

**You ask:**
```
Write lyrics for a rock song about freedom
```

**Your AI assistant executes:**
```bash
# Generate lyrics (compatible with Suno and Minimax)
302ai song lyrics --prompt "rock song about freedom and breaking chains" --provider suno
```

**Result:** Returns formatted lyrics in JSON

> 📝 **Lyrics Usage:** Generated lyrics can be passed to `song create --lyrics` or `song generate --lyrics` for music generation.

### Example 8: Generate Music with Custom Lyrics (Minimax - Sync)

**You ask:**
```
Generate music for these lyrics: [your lyrics here]
```

**Your AI assistant executes:**
```bash
# Minimax sync mode - returns audio URL immediately
302ai song generate --lyrics "Your verse here\nChorus here\n..." --provider minimax --model speech-01-turbo
```

**Result:** Returns a single audio URL

---

## 📚 Installation Guide {#installation-guide}

This skill works with all AI coding tools that support the `SKILL.md` format.

### Command-line Install (Recommended)

If your tool supports `claude skill` commands (e.g., **Claude Code**, **Cursor**):

```bash
claude skill install github:302ai/302ai-cli-skill/.claude/skills/302ai-cli
```

### Manual Install

For other tools that support `SKILL.md`:

1. **Clone the 302ai-cli-skill repository or download the ZIP:**
   ```bash
   git clone https://github.com/302ai/302ai-cli-skill.git
   ```
   Or click **Code** → **Download ZIP** on the [302ai/302ai-cli-skill](https://github.com/302ai/302ai-cli-skill) repository on GitHub, then extract the files.

2. Copy the 302ai-cli-skill files to your tool's `skills` directory (check your tool's documentation for the location)
3. Restart your tool

---

## 🔄 Update Guide

When new features are released (like the Song module), follow these steps:

### Step 1: Update the Skill

```bash
# For Claude Code
claude skill update github:302ai/302ai-cli-skill/.claude/skills/302ai-cli

# For Cursor or other tools
# Re-download SKILL.md and replace the old one
```

### Step 2: Upgrade the CLI Package

```bash
pip install cli_302ai==1.0.2b2 --upgrade
```

> ⚠️ **Critical:** Both steps are required. Updating only the skill won't enable new modules like 3D, Search, or Song. The CLI package must match the skill version.

---

## 🎛️ API Key Setup {#api-key-setup}

### Method 1: Environment Variable (Recommended)

**macOS / Linux:**
```bash
# Add to ~/.bashrc or ~/.zshrc for persistence
export AI302_KEY="your-api-key-here"
```

**Windows PowerShell:**
```powershell
# Add to PowerShell profile for persistence
$env:AI302_KEY = "your-api-key-here"
```

**Windows CMD:**
```cmd
set AI302_KEY=your-api-key-here
```

### Method 2: Per-Command Flag

```bash
302ai image create --prompt "a cat" --api_key "your-api-key-here"
```

---

## 🏗️ Project Structure

```
ai302-cli/
├── SKILL.md              # Main skill instructions
├── README.md             # This file
└── references/
    ├── image.md          # Image generation commands
    ├── video.md          # Video generation commands
    ├── tts.md            # Text-to-speech commands
    ├── stt.md            # Speech-to-text commands
    ├── sfx.md            # Sound effect commands
    ├── 3d.md             # 3D model generation
    ├── song.md           # Song/music generation
    ├── search.md         # Web search
    ├── model.md          # Model management
    └── record.md         # Billing queries
```

---

## 🎨 Advanced Usage

### Working with Multiple Modules

Your AI assistant can chain multiple operations:

```
Generate an image of a sunset, then create a video that animates it, and add background music
```

The assistant will:
1. Generate the image using `302ai image create`
2. Use that image for `302ai video create --image`
3. Generate music with `302ai song create`
4. Return all three URLs

### Model Selection

If you don't specify a model, the CLI uses smart defaults:
- Images: `flux-1.1-pro` (high quality)
- Videos: `kling-v1.6-standard` (balanced)
- TTS: Provider-specific defaults
- Songs: `suno-v4` (Suno) or `speech-01` (Minimax)

To see available models:
```bash
302ai model list t2i    # Image models
302ai model list t2v    # Video models
302ai model list tts    # TTS models
```

### Billing Queries

Track your usage and costs:
```bash
# Get billing info for a specific request
302ai record get <request_id>

# Request IDs are in the JSON output of generation commands
```

---

## 📚 Reference Documentation

Detailed command options, flags, and examples for each module are in the `references/` folder:

| File | Contents |
|------|----------|
| `references/image.md` | Image generation — t2i, i2i, sync and async workflows, all options |
| `references/video.md` | Video generation — t2v, i2v, async-only workflow |
| `references/tts.md` | Text-to-speech — providers, voices, async workflow |
| `references/stt.md` | Speech-to-text — sync transcription, supported formats |
| `references/sfx.md` | Sound effects — async generation, duration options |
| `references/3d.md` | 3D model generation — t23d, i23d, async workflow |
| `references/song.md` | Song/music generation — Suno async, Minimax/ElevenLabs sync, lyrics |
| `references/search.md` | Web search — providers (Tavily, Bocha, Exa), category filters |
| `references/model.md` | Model management — list, set default, get, params |
| `references/record.md` | Billing queries — look up cost and usage by request ID |

---

## 📊 Changelog

| Version | Date | Changes |
|---------|------|---------|
| v1.0.2b2 | 2025-06 | Added Song/Music module, package renamed to cli_302ai |
| v1.0.1b2 | 2025-06 | Added 3D module, Search module |
| v1.0.1b1 | 2025-05 | Initial release: Image, Video, TTS, STT, SFX |

---

## 📝 License

MIT

---

## 🔗 Links

- [302.AI Official Website](https://302.ai/)
- [302.AI API Documentation](https://doc.302.ai/)
- [GitHub Repository](https://github.com/302ai/302ai-cli-skill)

---

## 💬 Support

- 📧 Email: support@302.ai
- 📖 Full Documentation: [doc.302.ai](https://doc.302.ai/)
- 💡 Issues & Feedback: [GitHub Issues](https://github.com/302ai/302ai-cli-skill/issues)

---

<div align="center">

**⭐ Star this repo if you find it useful!**

Made with ❤️ for developers and AI enthusiasts across all platforms

[Get Started](#quick-start) • [View Examples](#usage-examples) • [Read Docs](#reference-documentation)

</div>

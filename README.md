# 302.AI CLI Skill

[English](#) | [中文文档](./README_CN.md)


> 🚀 **Empower your AI assistant with multimodal generation capabilities** - A powerful AI coding assistant skill that enables image, video, speech, sound effect, 3D model, music/song generation, and web search through the `302ai` command-line tool, powered by [302.AI Official Website](https://302.ai/).


> 💡 **Cross-Tool Compatible** - This Skill uses the standard SKILL.md format, supporting Claude Code, Cursor, and other compatible AI coding tools

---

## 🚀 Quick Start

Tell your Agent:
```bash
Install this skill: https://github.com/302ai/302ai-cli-skill
```

---

## ✨ What is This?

A powerful AI coding skill that lets your AI assistant understand natural language requests like *"generate a watercolor image of a cat"* and automatically translate them into the correct `302ai` CLI commands — sending requests to 302.AI's cloud services and returning results to you.

---

## 🎯 Core Features

- 🎨 **Multimodal generation** — Images, videos, speech, sound effects, 3D models, and music
- 🔍 **Web search** — Multiple search providers (Tavily, Bocha, Exa, Metaso)
- 🤖 **Intelligent command translation** — Automatically convert natural language into CLI commands
- ⚡ **Async and sync modes** — Flexible workflows for different scenarios
- 🌐 **1400+ AI models** — Access to 302.AI's comprehensive model ecosystem
- 🛠️ **Model management** — Easily list, set defaults, and query parameters

---

## 📦 What's Included?

### Modules Overview

| Module            | Description                                           | Mode                              | Output Format                 |
| ----------------- | ----------------------------------------------------- | --------------------------------- | ----------------------------- |
| 🎨 **Image**      | Text-to-image / Image-to-image                        | Async (recommended) or Sync       | PNG / WEBP / JPG URL          |
| 🎬 **Video**      | Text-to-video / Image-to-video                        | Async only                        | MP4 URL                       |
| 🗣️ **TTS**       | Text-to-speech                                        | Async                             | MP3 / WAV URL                 |
| 📝 **STT**        | Speech-to-text (transcription)                        | Sync                              | JSON text                     |
| 🔊 **SFX**        | Text-to-sound-effect                                  | Async                             | MP3 URL                       |
| 🧊 **3D**         | Text-to-3D / Image-to-3D                              | Async                             | GLB file URL                  |
| 🎵 **Song**       | AI music/song generation + lyric writing              | Async (Suno) or Sync (Minimax/ElevenLabs) | MP3 URL              |
| 🔍 **Search**     | Web search (multiple providers)                       | Sync                              | JSON results                  |

---

### 🎯 Supported Models per Module

| Module | Supported Models |
|--------|-----------------|
| 🎨 **Image** | `gemini-3.1-flash-image-preview` • `gpt-image-2-t2i` • `gpt-image-2-i2i` • `doubao-seedream-5-0-260128` • `gemini-3-pro-image` • `gemini-2.5-flash-image` • `gpt-image-1.5-t2i` • `gpt-image-1.5-i2i` • `wan2.7-image` • `wan2.7-image-pro` |
| 🎬 **Video** | `happyhorse-1.0-t2v` • `happyhorse-1.0-i2v` • `happyhorse-1.0-r2v` • `kling-o3` • `doubao-seedance-2-0-260128` • `wan2.7-t2v` • `wan2.7-i2v` • `wan2.7-r2v` • `official-kling-v3` • `runway-gen4` • `minimaxi-hailuo-02` • `google-veo3.1` • `google-veo3.1-pro` • `minimaxi-hailuo-2.3` • `viduq3-pro` |
| 🗣️ **TTS** | `tts-1` • `tts-1-hd` • `gpt-4o-mini-tts` • `doubao-tts` • `minimax-tts` • `google-tts` • `owen-tts` • `meitan-tts` • `mureka-tts` • `fish-audio-tts` • `elevenlabs-tts` |
| 📝 **STT** | `whisper-1` • `gpt-4o-transcribe` • `gpt-4o-mini-transcribe` • `gpt-4o-transcribe-diarize` • `recognize` • `scribe_v1_experimental` • `scribe_v1` • `sensevoice` |
| 🔊 **SFX** | `kling-sfx` |
| 🧊 **3D** | `hyper3d-rodin` |
| 🎵 **Song** | `chirp-fenix@suno` • `chirp-crow@suno` • `chirp-bluejay@suno` • `chirp-auk@suno` • `chirp-v4@suno` • `chirp-v3-5@suno` • `music-2.5+@minimax` • `music-2.5@minimax` • `music-2.0@minimax` • `music-1.5@minimax` • `music_v1@elevenlabs` |
| 🔍 **Search** | `tavily` • `search1_search` • `search1_news` • `bocha` • `exa` • `firecrawl` • `metaso` • `unifuncs` • `perplexity` |

> **💡 Tips:**
> - The above are the main models supported by each module. For the complete list, run `302ai model list <type>`
> - Speed, quality, and price vary across models — choose based on your actual needs. For pricing, visit the [official website](https://302.ai/)

---

## 💡 Usage Examples

### Example 1: Generate an Image

**You ask:**
```
Generate a watercolor painting of a sunset over mountains
```

**Your AI assistant executes:**
```bash
# Async mode (recommended) — avoids timeout
302ai image create --prompt "watercolor painting of a sunset over mountains" --model flux-1.1-pro
302ai image fetch <taskid> --short
```

**Result:** Returns a URL to the generated image

### Example 2: Create a Video

**You ask:**
```
Make a video of a cat playing with yarn
```

**Your AI assistant executes:**
```bash
# Video generation takes 1–5 minutes, async only
302ai video create --prompt "a cat playing with yarn" --model kling-v1.6-standard
302ai video fetch <taskid> --short
```

**Result:** Returns a URL to the generated video

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

> ⚠️ **Important:** The TTS module requires running `302ai tts refresh` before first use to cache the provider and voice list.

### Example 4: Web Search

**You ask:**
```
Search for the latest news on AI developments
```

**Your AI assistant executes:**
```bash
# Default provider is Tavily (best for English content)
302ai search run --query "latest AI developments" --provider tavily
```

**Result:** Returns JSON-formatted search results

> 💡 **Search tips:**
> - Default provider: `tavily` (best for English content)
> - Chinese content: Use `--provider bocha` for better results
> - Academic search: Use `--provider metaso --category scholar` or `--provider exa --category "research paper"`

### Example 5: Generate a 3D Model

**You ask:**
```
Create a 3D model of a coffee mug
```

**Your AI assistant executes:**
```bash
# 3D model generation takes 1–5 minutes
302ai 3d create --prompt "a coffee mug" --model hyper3d-rodin
302ai 3d fetch <taskid> --short
```

**Result:** Returns a URL to the `.glb` file

> 📦 **3D file viewing:** The `.glb` format can be opened with [Blender](https://www.blender.org/) (free), [Three.js online viewer](https://gltf-viewer.donmccurdy.com/), or any 3D model viewer supporting the glTF format.

### Example 6: Generate a Song (Suno — Async)

**You ask:**
```
Create a song about summer vacation
```

**Your AI assistant executes:**
```bash
# Suno generates 2 songs per task, takes 1–3 minutes
302ai song create --prompt "a cheerful song about summer vacation at the beach" --provider suno
302ai song fetch <taskid> --short
```

**Result:** Returns 2 audio URLs (Suno generates 2 variations per request)

> 💡 **Song generation tips:**
> - **Suno (async):** Best quality, generates 2 songs per task, supports custom lyrics
> - **Minimax (sync):** Faster, requires the `--lyrics` parameter
> - **ElevenLabs (sync):** Requires `--composition-plan` instead of lyrics

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

> 📝 **Using lyrics:** The generated lyrics can be passed to `song create --lyrics` or `song generate --lyrics` for music generation.

### Example 8: Generate Music with Custom Lyrics (Minimax — Sync)

**You ask:**
```
Generate music for these lyrics: [your lyrics here]
```

**Your AI assistant executes:**
```bash
# Minimax sync mode — returns the audio URL immediately
302ai song generate --lyrics "Your verse here\nChorus here\n..." --provider minimax --model speech-01-turbo
```

**Result:** Returns a single audio URL

---

## 📚 Installation Guide

This Skill is suitable for all AI coding tools that support the `SKILL.md` format.

### Command-line installation (recommended)

If your tool supports `claude skill` commands (such as **Claude Code**, **Cursor**):

```bash
claude skill install github:302ai/302ai-cli-skill/.claude/skills/302ai-cli
```

### Manual installation

For other tools that support `SKILL.md`:

1. **Clone the 302ai-cli-skill repository or download the 302ai-cli-skill ZIP:**
   ```bash
   git clone https://github.com/302ai/302ai-cli-skill.git
   ```
   Or click **Code** → **Download ZIP** on the [302ai/302ai-cli-skill](https://github.com/302ai/302ai-cli-skill) repository page on GitHub, then extract the files.

2. Copy the 302ai-cli-skill folder to your tool's `skills` directory (check your tool's documentation for the location)
3. Restart your tool

---

## 🔄 Update Guide

When new features are released (such as the Song module), follow these steps:

### Step 1: Update the Skill

```bash
# For Claude Code
claude skill update github:302ai/302ai-cli-skill/.claude/skills/302ai-cli

# For Cursor or other tools
# Re-download SKILL.md and replace the old one
```

### Step 2: Upgrade the CLI package

```bash
pip install cli_302ai==1.0.2b2 --upgrade
```

> ⚠️ **Critical:** Both steps are required. Updating only the skill will not enable new modules such as 3D, Search, or Song. The CLI package version must match the skill version.

---

## 🎛️ API Key Configuration

### Method 1: Environment variable (recommended)

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

### Method 2: Per-command flag

```bash
302ai image create --prompt "a cat" --api_key "your-api-key-here"
```

---

## 🏗️ Project Structure

```
ai302-cli/
├── SKILL.md              # Main skill instructions
├── README.md             # This file (English documentation)
├── README_CN.md          # Chinese documentation
└── references/
    ├── image.md          # Image generation commands
    ├── video.md          # Video generation commands
    ├── tts.md            # Text-to-speech commands
    ├── stt.md            # Speech-to-text commands
    ├── sfx.md            # Sound effect commands
    ├── 3d.md             # 3D model generation
    ├── song.md           # Music/song generation
    ├── search.md         # Web search
    ├── model.md          # Model management
    └── record.md         # Billing queries
```

---

## 🎨 Advanced Usage

### Working with Multiple Modules

Your AI assistant can chain multiple operations together:

```
Generate an image of a sunset, then create an animated video from it, and add background music
```

The assistant will:
1. Generate the image using `302ai image create`
2. Use that image for `302ai video create --image`
3. Generate music using `302ai song create`
4. Return all three URLs

### Model Selection

If you do not specify a model, the CLI uses smart defaults:
- Images: `flux-1.1-pro` (high quality)
- Videos: `kling-v1.6-standard` (balanced)
- TTS: Provider-specific defaults
- Songs: `suno-v4` (Suno) or `speech-01` (Minimax)

View available models:
```bash
302ai model list t2i    # Image models
302ai model list t2v    # Video models
302ai model list tts    # TTS models
```

### Billing Queries

Track your usage and costs:
```bash
# Get billing information for a specific request
302ai record get <request_id>

# Request IDs are available in the JSON output of generation commands
```

---

## 📚 Reference Documentation

Detailed command options, flags, and examples for each module are available in the `references/` folder:

| File | Contents |
|------|----------|
| `references/image.md` | Image generation — t2i, i2i, sync/async workflows, all options |
| `references/video.md` | Video generation — t2v, i2v, async-only workflow |
| `references/tts.md` | Text-to-speech — providers, voices, async workflow |
| `references/stt.md` | Speech-to-text — sync transcription, supported formats |
| `references/sfx.md` | Sound effects — async generation, duration options |
| `references/3d.md` | 3D model generation — t23d, i23d, async workflow |
| `references/song.md` | Music/song generation — Suno async, Minimax/ElevenLabs sync, lyrics |
| `references/search.md` | Web search — providers (Tavily, Bocha, Exa), category filters |
| `references/model.md` | Model management — list, set default, get, params |
| `references/record.md` | Billing queries — look up cost and usage by request ID |

---

## 📊 Changelog

| Version     | Date   | Changes                                                  |
|-------------|--------|----------------------------------------------------------|
| v1.0.2b2    | 2025-06 | Added Song/Music module, package renamed to cli_302ai    |
| v1.0.1b2    | 2025-06 | Added 3D module, Search module                           |
| v1.0.1b1    | 2025-05 | Initial release: Image, Video, TTS, STT, SFX             |

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
- 📖 Full documentation: [doc.302.ai](https://doc.302.ai/)
- 💡 Issues and feedback: [GitHub Issues](https://github.com/302ai/302ai-cli-skill/issues)

---

<div align="center">

**⭐ Star this repo if you find it useful!**

Made with ❤️ for developers and AI enthusiasts across all platforms

[Get Started](#quick-start) • [View Examples](#usage-examples) • [Read Docs](#reference-documentation)

</div>

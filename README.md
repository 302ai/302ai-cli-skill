# ai302-cli Skill

A Claude Code skill that guides agents to use the [302.AI CLI (`302ai`)](https://302.ai) for AI-powered media generation.

## What it does

Enables Claude Code to generate images, videos, text-to-speech audio, transcriptions, sound effects, 3D models, music/songs, and perform web searches via the `302ai` command-line tool.

| Module | Description | Mode |
|--------|-------------|------|
| Image | Text-to-image / Image-to-image | Async (preferred) or Sync |
| Video | Text-to-video / Image-to-video | Async only |
| TTS | Text-to-speech | Async |
| STT | Speech-to-text | Sync |
| SFX | Sound effects from text | Async |
| 3D | Text-to-3D / Image-to-3D | Async |
| Song | AI music generation (lyrics + audio) | Async (Suno) or Sync (Minimax/ElevenLabs) |
| Search | Web search (multiple providers) | Sync |

## Prerequisites

- Python 3.10+
- `pip install cli_302ai==1.0.2b1`
- A 302.AI API key (`AI302_KEY` env var or `--api_key` flag)

## Install

```bash
# From GitHub
claude skill install github:302ai/302ai-cli-skill/.claude/skills/302ai-cli

```

## Update

```bash
# 1. Update the skill
claude skill update github:302ai/302ai-cli-skill/.claude/skills/302ai-cli

# 2. Upgrade the 302ai CLI package (required for new modules)
pip install cli_302ai==1.0.2b1 --upgrade
```

> **Important**: Updating the skill alone is not enough — you must also upgrade the `cli_302ai` pip package to the matching version, otherwise new modules (e.g. 3D, search) will not be available.

## Usage

Once installed, Claude Code will automatically invoke this skill when you ask for AI media generation tasks. Examples:

- "Generate an image of a cat in watercolor style"
- "Create a video of a sunset over the ocean"
- "Read this text aloud in a female voice"
- "Transcribe this audio file"
- "Generate a thunder sound effect"
- "Generate a 3D model of a cute cat"
- "Create a song about friendship"
- "Generate lyrics for a rock song"
- "Search the web for latest AI news"

## Structure

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

## License

MIT

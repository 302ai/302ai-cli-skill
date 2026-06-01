# ai302-cli Skill

A Claude Code skill that guides agents to use the [302.AI CLI](https://302.ai) for AI-powered media generation.

## What it does

Enables Claude Code to generate images, videos, text-to-speech audio, transcriptions, sound effects, 3D models, and perform web searches via the `ai302` command-line tool.

| Module | Description | Mode |
|--------|-------------|------|
| Image | Text-to-image / Image-to-image | Async (preferred) or Sync |
| Video | Text-to-video / Image-to-video | Async only |
| TTS | Text-to-speech | Async |
| STT | Speech-to-text | Sync |
| SFX | Sound effects from text | Async |
| 3D | Text-to-3D / Image-to-3D | Async |
| Search | Web search (multiple providers) | Sync |

## Prerequisites

- Python 3.10+
- `pip install ai302==1.0.1b2`
- A 302.AI API key (`AI302_KEY` env var or `--api_key` flag)

## Install

```bash
# From GitHub
claude skill install github:302ai/ai302-cli-skill/.claude/skills/ai302-cli

```

## Update

```bash
# Update to the latest version
claude skill update github:302ai/ai302-cli-skill/.claude/skills/ai302-cli
```

## Usage

Once installed, Claude Code will automatically invoke this skill when you ask for AI media generation tasks. Examples:

- "Generate an image of a cat in watercolor style"
- "Create a video of a sunset over the ocean"
- "Read this text aloud in a female voice"
- "Transcribe this audio file"
- "Generate a thunder sound effect"
- "Generate a 3D model of a cute cat"
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
    ├── search.md         # Web search
    ├── model.md          # Model management
    └── record.md         # Billing queries
```

## License

MIT

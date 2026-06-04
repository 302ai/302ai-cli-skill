---
name: ai302-cli
description: >
  Use the 302.AI CLI (302ai) to generate images, videos, text-to-speech audio, speech-to-text transcriptions,
  sound effects, 3D models, music/songs, and perform web searches via AI models. Trigger this skill whenever the
  user asks to: generate/create images (text-to-image or image-to-image), generate/create videos (text-to-video or
  image-to-video), convert text to speech / TTS / voice synthesis, transcribe audio / speech-to-text / STT, generate
  sound effects / audio from text prompts, generate 3D models (text-to-3D or image-to-3D), generate music / songs /
  AI music, create lyrics, search the web, query AI model parameters or billing records, or any task involving the
  `302ai` command-line tool. Also trigger when the user mentions "302.AI", "302ai", or wants to use AI media
  generation APIs from the command line.
---

# 302.AI CLI Skill

Guide the agent to use the `302ai` CLI for AI-powered media generation. The CLI supports image, video, TTS, STT, SFX, 3D generation, song/music generation, web search, model management, and billing queries.

## Prerequisites

Before running any `302ai` command, verify the environment:

1. **Installation**: `pip install cli_302ai==1.0.2b1`
   - **Upgrading from an older version**: Run `pip install cli_302ai==1.0.2b1 --upgrade`. The skill and the pip package must be at the same version — updating the skill alone won't enable new modules (3D, search).
2. **API Key**: Set `AI302_KEY` env var, or pass `--api_key` to each command. Not strictly required if the user has already configured it.
3. **PATH** (Windows only): If `302ai` is not found after pip install, the Scripts directory is not in PATH. Run `pip show cli_302ai` to find the Location, then add the sibling `Scripts` folder to PATH.

Check setup with:
```bash
302ai model list t2i
```
If this returns JSON with `"status":"completed"`, the CLI is working.

## Command Architecture

All commands follow this pattern:
```
302ai <module> <action> [OPTIONS]
```

Modules: `model`, `image`, `video`, `tts`, `stt`, `sfx`, `3d`, `song`, `search`, `record`

Every command outputs JSON to stdout. On success, `"status"` is `"completed"` (or `"pending"` for async tasks). On failure, `"status"` is `"failed"` and exit code is `1`. Configuration errors (missing model, invalid kind) use exit code `2`.

## Sync vs Async: The Core Decision

**Prefer async workflows** (create + fetch) for image, video, TTS, and SFX generation. Async is more robust: it avoids timeout risks on long-running tasks, gives you a task ID for tracking/retry, and lets the agent do other work while waiting. Only use sync (`image generate`) when the user explicitly wants to block-and-wait for a single immediate result.

| Module | Preferred | Command | Behavior |
|--------|-----------|---------|----------|
| image | **Async** | `image create` + `image fetch` | Returns taskid immediately, poll for result |
| image | Sync (fallback) | `image generate` | Blocks until image is ready — risk of timeout on slow models |
| video | **Async only** | `video create` + `video fetch` | Always async, poll for result |
| tts | **Async** | `tts create` + `tts fetch` | Returns taskid, poll for audio URL |
| stt | Sync | `stt transcribe` | Blocks until transcription complete (fast, sync is fine) |
| sfx | **Async** | `sfx create` + `sfx fetch` | Returns taskid, poll for result |
| 3d | **Async** | `3d create` + `3d fetch` | Returns taskid, poll for 3D model URL |
| song | **Async** (Suno) | `song create` + `song fetch` | Returns taskid, poll for audio URLs (2 songs per task) |
| song | Sync (Minimax) | `song generate --provider minimax` | Blocks until audio ready, requires `--lyrics` |
| song | Sync (ElevenLabs) | `song generate --provider elevenlabs` | Blocks until audio ready, requires `--composition-plan` |
| search | Sync | `search run` | Returns search results immediately |

**Async polling pattern** — for any async task:
1. Call `create` → get `taskid` from JSON output
2. Call `fetch <taskid> --short` repeatedly until `status` is `"completed"` or `"failed"`
3. Extract the result URL from the completed response

Use `--short` on fetch commands to get minimal JSON (status + taskid + result_url), which is cleaner for polling loops.

## Workflow Selection Guide

When the user asks for something, pick the right module and workflow:

- **"Generate an image"** / **"Create a picture"** → `image create` + `image fetch` (async, preferred) or `image generate` (sync fallback)
- **"Edit this image"** / **"Add sunglasses to this photo"** → `image create` with `--image` flag (async, preferred)
- **"Make a video"** / **"Animate this"** → `video create` + `video fetch` (always async)
- **"Read this text aloud"** / **"Text to speech"** → `tts create` + `tts fetch` (requires provider, voice, model)
- **"Transcribe this audio"** / **"What does this say?"** → `stt transcribe` (sync)
- **"Generate a sound effect"** / **"Make thunder sounds"** → `sfx create` + `sfx fetch` (async)
- **"What models are available?"** → `model list <kind>` or `model params <model>`
- **"How much did this cost?"** / **"Show billing"** → `record get <request_ids>`
- **"Generate a 3D model"** / **"Make a 3D cat"** → `302ai 3d create` + `302ai 3d fetch` (async)
- **"Convert this image to 3D"** → `302ai 3d create` with `--image` flag (async)
- **"Generate a song"** / **"Create music"** / **"AI music"** → `302ai song create` + `302ai song fetch` (Suno async, preferred) or `302ai song generate` (Minimax/ElevenLabs sync)
- **"Write lyrics"** / **"Generate lyrics"** → `302ai song lyrics` (Suno) or `302ai song lyrics --provider elevenlabs` (composition plan)
- **"Search the web"** / **"Find information about X"** → `302ai search run` (sync)

## Key Gotchas

1. **Model selection**: If the user doesn't specify a model, you can:
   - Set a default with `302ai model set <kind> <model>` (persists locally)
   - Get one programmatically with `302ai model get <kind>` (first in list) or `302ai model get <kind> --random`
   - If no model is specified and no default is set, commands exit with code 2

2. **TTS requires setup**: First run `302ai tts refresh` to cache provider/voice data locally. Then discover providers with `tts providers` and voices with `tts voices`.

3. **Type auto-detection**: Image and video commands infer t2i/i2i and t2v/i2v from whether `--image` is provided. Don't manually specify the type — just include or omit `--image`.

4. **Extra parameters**: Both `image generate` and `video create` accept `--extra` for a JSON string of additional model-specific parameters. Use this when the user needs fine-grained control not covered by standard options.

5. **Polling intervals**: When polling async tasks, wait a few seconds between fetches. Image tasks typically complete in 10-30 seconds, video in 1-5 minutes, TTS in 5-15 seconds, 3D in 1-5 minutes, song (Suno) in 1-3 minutes.

6. **Search provider selection**: Default provider is `tavily`. For Chinese content, use `--provider bocha` for better results. For academic searches, use `--provider metaso --category scholar` or `--provider exa --category "research paper"`.

7. **Song provider differences**: Suno is async (create + fetch), generates 2 songs per task. Minimax and ElevenLabs are sync (generate returns directly). ElevenLabs requires `--composition-plan` (not `--lyrics`). Suno lyrics are compatible with Minimax but not ElevenLabs.

8. **Output parsing**: All commands output JSON. Parse with `jq` or read the raw JSON. The key fields are:
   - `status` — task state
   - `image_url` / `video_url` / `audio_url` / `result_url` — the generated media
   - `result_url` — for 3D tasks, points to the generated `.glb` file
   - `results[].audio_url` — for song tasks, the generated audio URLs (Suno returns 2 per task)
   - `taskid` — for async task tracking
   - `request_id` / `ai302_cost_request_ids` — for billing queries via `record get`

## Reference Files

Read these for detailed option tables and examples per module:

- `references/image.md` — Image generation (t2i, i2i, sync/async)
- `references/video.md` — Video generation (t2v, i2v, async)
- `references/tts.md` — Text-to-speech (providers, voices, async)
- `references/stt.md` — Speech-to-text (sync transcription)
- `references/sfx.md` — Sound effects (async generation)
- `references/model.md` — Model management and parameter queries
- `references/record.md` — Billing record queries
- `references/3d.md` — 3D model generation (t23d, i23d, async)
- `references/song.md` — Song/music generation (Suno async, Minimax/ElevenLabs sync)
- `references/search.md` — Web search (multiple providers, sync)

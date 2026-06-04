# Song Module

AI music generation (lyrics + composition + audio). Supports three providers with different trade-offs.

| Provider | Speed | Price | Timbre | Mode |
|---|---|---|---|---|
| **Suno** | Fast | Low | Good | Async (create + fetch) |
| **Minimax** | Slow | Low | Better | Sync (generate) |
| **ElevenLabs** | Fast | High | Best | Sync (generate) |

**Recommendation:** Start with Suno. Best balance of speed, price, and quality. Use Minimax when you want better timbre and can wait. Use ElevenLabs when budget allows and you need the best audio quality.

## Provider Compatibility

| Lyrics Source | Suno | Minimax | ElevenLabs |
|---|---|---|---|
| Suno lyrics | Yes | Yes | No |
| ElevenLabs composition plan | No | No | Yes |
| Simple prompt (no lyrics) | Yes | No | Yes |

- Suno and Minimax share the same lyrics format (plain text with structure tags like `[Verse]`, `[Chorus]`)
- ElevenLabs uses its own structured composition plan (JSON with per-section styles and durations)
- Use `song lyrics --provider elevenlabs` to generate an ElevenLabs-compatible plan

## Models

List available models:
```bash
302ai model list song
# Returns: ["chirp-v3-5@suno", "music-2.5@minimax", "music_v1@elevenlabs", ...]
```

Model format: `model@provider`. When passed to song commands, the `@provider` part is auto-parsed.

---

## song create (Suno — Async)

Create a Suno music task. Returns a task ID for polling.

```bash
302ai song create "<prompt>" [OPTIONS]
```

### Options

| Option | Type | Required | Default | Description |
|---|---|---|---|---|
| `prompt` | string | positional | — | Simple description, auto-generates music |
| `--lyrics` | string | No | — | Custom lyrics (triggers custom mode) |
| `--style` | string | Yes (custom) | — | Style tags, comma-separated |
| `--title` | string | Yes (custom) | — | Song title |
| `--model` | string | No | `chirp-v3-5` | Supports `model@provider` format |
| `--instrumental` | bool | No | `false` | Instrumental only (no lyrics) |
| `--vocal-gender` | string | No | — | `f` or `m` |
| `--style-weight` | float | No | — | 0-1 |
| `--weirdness-constraint` | float | No | — | 0-1, creative divergence |
| `--negative-tags` | string | No | — | Styles to exclude |
| `--webhook` | string | No | — | Callback URL |
| `--short` | bool | No | `false` | Only return taskid and status |
| `-o, --output` | string | No | — | Save taskid to file |
| `--api_key` | string | No | from env | |

**Two modes:**
- **Simple mode:** `song create "description"` — Suno auto-generates lyrics and music
- **Custom mode:** `song create --lyrics "..." --style "rock" --title "My Song"` — use your own lyrics

### Short Output

```json
{"taskid":"<taskid>","status":"pending","results":[]}
```

### Full Output

```json
{
  "status": "pending",
  "taskid": "<taskid>",
  "provider": "suno",
  "model": "chirp-v3-5",
  "request": {"method":"POST","url":"/suno/submit/music","headers":{"Authorization":"[REDACTED]"},"body":{...}},
  "raw": {...}
}
```

### Examples

```bash
# Simple mode
302ai song create "a cheerful electronic track for coding"

# Instrumental
302ai song create "epic orchestral battle music" --instrumental

# Custom mode with lyrics
302ai song create --lyrics "[Verse]\n歌词内容" --style "rock,热血" --title "并肩向前"
```

---

## song generate (Minimax / ElevenLabs — Sync)

Generate music synchronously. Returns audio URL directly.

```bash
302ai song generate "<prompt>" [OPTIONS]
```

### Options

| Option | Type | Required | Default | Description |
|---|---|---|---|---|
| `prompt` | string | positional | — | Music description |
| `--provider` | string | Conditional | — | `minimax` or `elevenlabs` |
| `--model` | string | No | auto | Supports `model@provider` format |
| `--lyrics` | string | Yes (Minimax) | — | Lyrics content |
| `--composition-plan` | string | ElevenLabs | — | JSON string or file path |
| `--duration` | int | No | — | Duration in ms (ElevenLabs only) |
| `--output-format` | string | No | `url` | `url` or `hex` (Minimax only) |
| `--sample-rate` | int | No | `44100` | 16000/24000/32000/44100 (Minimax only) |
| `--bitrate` | int | No | `128000` | 32000/64000/128000/256000 (Minimax only) |
| `--audio-format` | string | No | `mp3` | mp3/wav/pcm (Minimax only) |
| `--short` | bool | No | `false` | Only return results list |
| `-o, --output` | string | No | — | Save audio URL to file |
| `--api_key` | string | No | from env | |

### Short Output

```json
{
  "status": "completed",
  "provider": "minimax",
  "results": [
    {"audio_url":"https://...mp3","image_url":null,"title":null,"duration":null}
  ]
}
```

### Examples

```bash
# Minimax with lyrics
302ai song generate "epic rock" --provider minimax --lyrics "[Verse]\n歌词内容"

# ElevenLabs with simple prompt
302ai song generate "happy song" --provider elevenlabs

# ElevenLabs with composition plan (from file)
302ai song generate --provider elevenlabs --composition-plan plan.json

# ElevenLabs with composition plan (inline JSON)
302ai song generate --provider elevenlabs --composition-plan '{"positive_global_styles":[...]}'
```

---

## song lyrics

Generate lyrics (Suno) or composition plan (ElevenLabs).

```bash
302ai song lyrics "<prompt>" [OPTIONS]
```

### Options

| Option | Type | Required | Default | Description |
|---|---|---|---|---|
| `prompt` | string | positional | **Yes** | Description |
| `--provider` | string | No | `suno` | `suno` or `elevenlabs` |
| `--model` | string | No | auto | |
| `--short` | bool | No | `false` | Only return lyrics/plan content |
| `-o, --output` | string | No | — | Save to file |
| `--api_key` | string | No | from env | |

### Suno Output (short)

```json
{"status":"complete","title":"歌名","lyrics":"[Verse 1]\n歌词内容..."}
```

### ElevenLabs Output (short)

```json
{
  "status": "completed",
  "composition_plan": {
    "positive_global_styles": ["rock","epic"],
    "negative_global_styles": ["slow","acoustic"],
    "sections": [
      {"section_name":"Verse 1","positive_local_styles":[...],"negative_local_styles":[...],"duration_ms":20000,"lines":["歌词行1","歌词行2"]}
    ]
  }
}
```

### Examples

```bash
# Suno lyrics
302ai song lyrics "关于友情、战斗、热血的歌" -o lyrics.txt

# ElevenLabs composition plan
302ai song lyrics "happy pop song" --provider elevenlabs -o plan.json
```

---

## song fetch (Poll Suno Task)

```bash
302ai song fetch <taskid> [--short]
```

- `--short`: Returns only `status`, `taskid`, `results` list

### Short Output

```json
{
  "taskid": "<taskid>",
  "status": "completed",
  "results": [
    {"audio_url":"https://...mp3","image_url":"https://...jpg","title":"歌名","duration":"154"},
    {"audio_url":"https://...mp3","image_url":"https://...jpg","title":"歌名","duration":"135"}
  ]
}
```

> Suno generates 2 songs per task. `results` array contains 2 objects.

---

## Typical Workflows

### Quick start (Suno simple mode)

```bash
# 1. Create task
302ai song create "轻快的电子音乐，适合编程时听" --short
# Returns: {"taskid":"xxx","status":"pending","results":[]}

# 2. Wait and fetch
302ai song fetch xxx --short
# Returns: {"status":"completed","results":[{"audio_url":"...","image_url":"...","title":"...","duration":"154"},...]}
```

### Suno with lyrics

```bash
# 1. Generate lyrics
302ai song lyrics "关于友情的歌" --short
# Returns: {"title":"...","lyrics":"[Verse]\n..."}

# 2. Create with custom lyrics
302ai song create --lyrics "歌词内容" --style "rock" --title "歌名" --short

# 3. Fetch result
302ai song fetch <taskid> --short
```

### Minimax

```bash
302ai song generate "epic rock" --provider minimax --lyrics "[Verse]\n歌词内容" --short
```

### ElevenLabs (composition plan flow)

```bash
# 1. Generate composition plan
302ai song lyrics "happy pop song" --provider elevenlabs -o plan.json

# 2. Generate music from plan
302ai song generate --provider elevenlabs --composition-plan plan.json --short
```

## Notes

- Status enum: `pending` / `processing` / `completed` / `failed` (unified across all modules)
- Suno is async (create + fetch), Minimax and ElevenLabs are sync (generate returns directly)
- `model@provider` format: e.g. `chirp-v3-5@suno`, the `@provider` part is auto-parsed
- ElevenLabs does NOT accept `--lyrics`, must use `--composition-plan`
- Suno lyrics can be reused with Minimax, but not with ElevenLabs (different format)

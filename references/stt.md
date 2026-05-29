# STT Module (Speech-to-Text)

Transcribe audio files to text. **Synchronous** — blocks until transcription is complete.

## stt transcribe

```bash
ai302 stt transcribe --file <PATH|URL|DATA_URL> [OPTIONS]
```

### Options

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `--file` | string | Yes | Local file path, URL, or Base64 data URL |
| `--model` | string | No | STT model; falls back to local default or first available |
| `--language` | string | No | Language code (e.g. "en", "zh") |
| `--prompt` | string | No | Context hint for better transcription |
| `--temperature` | float | No | Sampling temperature |
| `--api_key` | string | No | Override API key |

Note: `response_format` is internally fixed to `json` and not exposed as a CLI option.

### Success Output

```json
{
  "status": "completed",
  "request_id": "<request-id>",
  "model": "<model>",
  "result": {"text": "<transcribed_text>"},
  "request": {...}
}
```

### Failure Output (exit code 1)

```json
{
  "status": "failed",
  "error": "<message>",
  "retryable": false,
  "request": {...}
}
```

## stt models

List all available STT models.

```bash
ai302 stt models
```

Output: `{"status":"completed","models":["<model_name>",...]}`

## Typical Workflow

```bash
# 1. Check available models
ai302 stt models

# 2. Transcribe a local file
ai302 stt transcribe --file audio.mp3 --model whisper-1

# 3. Transcribe a URL
ai302 stt transcribe --file "https://example.com/podcast.mp3" --model whisper-1

# 4. With language hint
ai302 stt transcribe --file interview.wav --model whisper-1 --language zh
```

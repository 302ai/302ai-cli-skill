# TTS Module (Text-to-Speech)

Convert text to speech. Requires discovering providers and voices first. **Async workflow** (create + fetch).

**First-time setup**: Run `302ai tts refresh` to cache provider/voice data locally. This is required before any other TTS command.

## tts refresh

Pull provider/voice/model data from the API and cache locally.

```bash
302ai tts refresh [--provider <name>] [--api_key <key>]
```

Success output: `{"status":"completed","saved_to":"<path>","providers_count":13}`

## tts providers

List all cached TTS providers.

```bash
302ai tts providers [--provider <name>]
```

Output: `{"status":"completed","providers":[{"provider":"openai","models":[...],"output_formats":[...],"voices_count":12}]}`

## tts providers-get

Get a single provider from cache.

```bash
302ai tts providers-get [--provider <name>] [--random]
```

- Without flags: returns first provider
- `--random`: picks a random one

## tts voices

List voices for a provider. Defaults to 20 voices, prioritizing those supporting both Chinese and English.

```bash
302ai tts voices [--provider <name>]
```

Output: `{"status":"completed","provider":"...","voices":[{"provider":"...","voice":"...","name":"..."}]}`

## tts voices-get

Get a single voice from a provider.

```bash
302ai tts voices-get <provider> [--random]
```

- `provider` is a required positional argument
- Without `--random`: returns first voice
- `--random`: picks a random one

## tts create

Submit a TTS task. **Requires provider, voice, and model** — all three are mandatory.

```bash
302ai tts create --text "<text>" --provider <provider> --voice <voice> --model <model> [OPTIONS]
```

### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--text` | string | Yes | — | Text to synthesize |
| `--provider` | string | Yes | — | Provider name (e.g. "openai") |
| `--voice` | string | Yes | — | Voice name (e.g. "alloy") |
| `--model` | string | Yes | — | TTS model name |
| `--speed` | float | No | — | Speech speed |
| `--volume` | float | No | — | Volume |
| `--emotion` | string | No | — | Emotion style |
| `--output_format` | string | No | — | Audio output format |
| `--timeout` | int | No | — | Timeout in seconds |
| `--webhook` | string | No | — | Callback URL |
| `--run_async / --no-run_async` | bool | No | true | Async mode (default on) |
| `--extra` | string | No | — | Additional params as JSON |

### Success Output

```json
{
  "status": "pending",
  "taskid": "<id>",
  "request_id": "<req_id>",
  "cost": {...},
  "created_at": "...",
  "request": {...},
  "raw": {...}
}
```

## tts fetch

Poll for TTS task completion.

```bash
302ai tts fetch <taskid> [--short]
```

- `--short`: Returns `status`, `taskid`, `ai302_cost_request_ids`, `result_url`

### Short Output (completed)

```json
{"status":"completed","taskid":"<taskid>","ai302_cost_request_ids":"<ids>","result_url":"https://..."}
```

### Short Output (failed)

```json
{"status":"failed","taskid":"<taskid>","error":"<message>"}
```

## Typical Workflow

```bash
# 1. First time: refresh cache
302ai tts refresh

# 2. Discover providers and voices
302ai tts providers --provider openai
302ai tts voices --provider openai

# 3. Submit TTS task
302ai tts create --text "Hello, welcome to the show" --provider openai --voice alloy --model tts-1

# 4. Poll for result
302ai tts fetch task_abc123 --short
```

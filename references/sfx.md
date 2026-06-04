# SFX Module (Sound Effects)

Generate sound effects from text prompts via Kling AI. **Async workflow** (create + fetch).

## sfx create

Submit a sound effect generation task.

```bash
302ai sfx create --prompt "<prompt>" [OPTIONS]
```

### Options

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `--prompt` | string | Yes | — | Generation prompt (max 200 chars) |
| `--duration` | float | No | 5.0 | Duration in seconds (3.0–10.0, step 0.1) |
| `--negative_prompt` | string | No | — | Negative prompt |
| `--cfg` | float | No | — | Guidance scale |
| `--webhook` | string | No | — | Callback URL |
| `--api_key` | string | No | env var | Override API key |

### Success Output

```json
{
  "status": "pending",
  "raw_status": "submitted",
  "taskid": "<taskid>",
  "ai302_cost_request_ids": "<request-id>",
  "request_id": "<request-id>",
  "request": {...},
  "raw": {}
}
```

### Failure Output (exit code 1)

```json
{
  "status": "failed",
  "taskid": null,
  "error": "<message>",
  "retryable": false,
  "request": {...}
}
```

## sfx fetch

Poll for task completion.

```bash
302ai sfx fetch <taskid> [--short]
```

- `--short`: Returns `status`, `taskid`, `ai302_cost_request_ids`, `result_url`

### Short Output

```json
{"status":"completed","taskid":"<taskid>","ai302_cost_request_ids":"<ids>","result_url":"https://..."}
```

## Typical Workflow

```bash
# 1. Submit sound effect task
302ai sfx create --prompt "thunder storm with heavy rain" --duration 7.0

# 2. Poll until done
302ai sfx fetch task_abc123 --short
```

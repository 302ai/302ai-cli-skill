# Video Module

Generate videos via text-to-video (t2v) or image-to-video (i2v). **Async only** — always use create + fetch pattern.

Type is auto-detected: `--image`/`--end_image` present → i2v, absent → t2v.

## video create

Submit a video generation task.

```bash
ai302 video create --prompt "<prompt>" [OPTIONS]
```

### Options

| Option | Type | Required | Repeatable | Description |
|--------|------|----------|------------|-------------|
| `--prompt` | string | Yes | No | Generation prompt |
| `--model` | string | No | No | Model name; falls back to local default |
| `--image` | string | No | Yes | Start frame reference image (URL/Base64); repeat for multiple |
| `--end_image` | string | No | No | End frame reference image |
| `--video` | string | No | No | Reference video URL |
| `--negative_prompt` | string | No | No | Negative prompt |
| `--duration` | int | No | No | Video duration |
| `--resolution` | string | No | No | Resolution |
| `--aspect_ratio` | string | No | No | Aspect ratio |
| `--fps` | string | No | No | Frame rate |
| `--webhook` | string | No | No | Callback URL |
| `--extra` | string | No | No | Additional params as JSON string |

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

### Error Cases (exit code 2)

- No model specified and no local default configured
- Model type doesn't match inferred type (e.g. using an i2v model for t2v)

## video fetch

Poll for task status and result.

```bash
ai302 video fetch <taskid> [--short]
```

- `--short`: Returns only `status`, `taskid`, `result_url`

### Short Output

```json
{"status":"completed","taskid":"task_abc123","result_url":"https://..."}
```

## Typical Workflow

```bash
# Text-to-video
ai302 video create --prompt "a cat runs across the yard" --model kling-v2
# Returns: {"status":"pending","taskid":"task_xyz789",...}

# Poll until done (video generation takes 1-5 minutes typically)
ai302 video fetch task_xyz789 --short

# Image-to-video
ai302 video create --prompt "the person starts walking" --image "https://example.com/photo.jpg" --model kling-v2
```

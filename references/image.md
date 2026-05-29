# Image Module

Generate images via text-to-image (t2i) or image-to-image (i2i).

Type is auto-detected: `--image` present → i2i, absent → t2i.

**Prefer the async workflow** (create + fetch) — it avoids timeout risks on slow models, gives you a task ID for tracking, and lets the agent do other work while waiting.

## image create (Async — Preferred)

Submit an image generation task without blocking. Use `image fetch` to poll for results.

```bash
ai302 image create --prompt "<prompt>" [OPTIONS]
```

### Options

| Option | Type | Required | Repeatable | Description |
|--------|------|----------|------------|-------------|
| `--prompt` | string | Yes | No | Generation prompt |
| `--model` | string | No | No | Model name; falls back to local default |
| `--height` | int | No | No | Image height in pixels |
| `--width` | int | No | No | Image width in pixels |
| `--negative_prompt` | string | No | No | Negative prompt |
| `--aspect_ratio` | string | No | No | Aspect ratio (e.g. "16:9") |
| `--output_format` | string | No | No | Output format |
| `--image` | string | Yes (for i2i) | Yes | Reference image URL or Base64; repeat for multiple |
| `--mask_image` | string | No | No | Mask image for inpainting |
| `--extra` | string | No | No | Additional params as JSON string |
| `--run_async / --no-run_async` | bool | No | No | Default is async; `--no-run_async` forces sync |
| `--webhook` | string | No | No | Callback URL for completion notification |

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

### Example

```bash
# Text-to-image (async)
ai302 image create --prompt "a cat sitting on a windowsill" --model flux-schnell
# Returns: {"status":"pending","taskid":"task_abc123",...}

# Image-to-image (async)
ai302 image create --prompt "add sunglasses" --image "https://example.com/photo.jpg"
```

## image fetch (Poll Async Task)

```bash
ai302 image fetch <taskid> [--short]
```

- `--short`: Returns only `status`, `taskid`, `result_url`

### Short Output

```json
{"status":"completed","taskid":"task_abc123","result_url":"https://..."}
```

## Typical Async Workflow

```bash
# Submit task
TASK=$(ai302 image create --prompt "a sunset over mountains" --model flux-schnell | jq -r .taskid)

# Poll until done (typically 10-30 seconds)
ai302 image fetch $TASK --short
```

## image generate (Sync — Fallback)

Blocks until the image is ready. Only use when the user explicitly wants a blocking call for a single immediate result. Prefer `image create` + `image fetch` instead — sync mode risks timeout on slow models.

```bash
ai302 image generate --prompt "<prompt>" [OPTIONS]
```

Same options as `image create` (without `--run_async` and `--webhook`).

### Success Output

```json
{
  "status": "completed",
  "taskid": "<id or null>",
  "image_url": "https://...",
  "image_urls": ["https://..."],
  "model": "...",
  "request": {...},
  "raw": {...}
}
```

### Example

```bash
ai302 image generate --prompt "landscape" --width 1024 --height 768 --model flux-schnell
```

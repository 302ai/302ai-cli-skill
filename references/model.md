# Model Module

Manage default models and query model metadata. Use this to discover available models before calling generation commands.

## model set

Set the default model for a given type. Persists locally.

```bash
302ai model set <kind> <model>
```

- `kind`: One of `t2v`, `i2v`, `t2i`, `i2i`, `stt`, `song`
- `model`: Model name (no validation)
  - Song models use `model@provider` format (e.g. `chirp-v3-5@suno`), the `@provider` part is auto-parsed

Output: `{"status":"completed","kind":"t2v","model":"<model_name>"}`

## model list

List all supported models for a type.

```bash
302ai model list <kind>
```

- `kind`: One of `t2v`, `i2v`, `t2i`, `i2i`, `stt`, `song`
- Invalid kind → exit code 2

Output: `{"status":"completed","kind":"t2v","models":["<model_name>",...]}`

## model get

Get a single model from the supported list.

```bash
302ai model get <kind> [--random]
```

- Without `--random`: returns first model in list
- `--random`: picks a random model

Output: `{"status":"completed","kind":"t2v","model":"<model_name>"}`

## model params

Query parameter metadata for a specific model. Useful for building UI forms or validating inputs.

```bash
302ai model params <model>
```

### Output

```json
{
  "status": "completed",
  "model": "<model>",
  "params": [
    {
      "name": "<param_name>",
      "type": "string|int|float|bool|object|list[string]",
      "max_length": null,
      "max_items": null,
      "enum": null,
      "range": null,
      "description": "..."
    }
  ]
}
```

Field meanings:
- `type`: Data type of the parameter
- `max_length`: String max length (null = unlimited)
- `max_items`: List max items (null = unlimited)
- `enum`: Allowed values (null = no restriction)
- `range`: Numeric range as `[min, max]` (null = no restriction)

## Usage Patterns

```bash
# Discover what image models are available
302ai model list t2i

# Set a default model so you don't need --model every time
302ai model set t2i flux-schnell

# Check what parameters a model accepts
302ai model params flux-schnell

# Get a random video model
302ai model get t2v --random

# Discover what song models are available
302ai model list song
# Returns: ["chirp-v3-5@suno", "music-2.5@minimax", "music_v1@elevenlabs", ...]

# Set a default song model
302ai model set song chirp-v3-5@suno
```

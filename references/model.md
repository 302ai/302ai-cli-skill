# Model Module

Manage default models and query model metadata. Use this to discover available models before calling generation commands.

## model set

Set the default model for a given type. Persists locally.

```bash
ai302 model set <kind> <model>
```

- `kind`: One of `t2v`, `i2v`, `t2i`, `i2i`, `stt`
- `model`: Model name (no validation)

Output: `{"status":"completed","kind":"t2v","model":"<model_name>"}`

## model list

List all supported models for a type.

```bash
ai302 model list <kind>
```

- `kind`: One of `t2v`, `i2v`, `t2i`, `i2i`, `stt`
- Invalid kind → exit code 2

Output: `{"status":"completed","kind":"t2v","models":["<model_name>",...]}`

## model get

Get a single model from the supported list.

```bash
ai302 model get <kind> [--random]
```

- Without `--random`: returns first model in list
- `--random`: picks a random model

Output: `{"status":"completed","kind":"t2v","model":"<model_name>"}`

## model params

Query parameter metadata for a specific model. Useful for building UI forms or validating inputs.

```bash
ai302 model params <model>
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
ai302 model list t2i

# Set a default model so you don't need --model every time
ai302 model set t2i flux-schnell

# Check what parameters a model accepts
ai302 model params flux-schnell

# Get a random video model
ai302 model get t2v --random
```

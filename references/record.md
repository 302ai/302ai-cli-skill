# Record Module (Billing)

Query billing records by request ID. Use this to check costs after generation tasks complete.

The `request_id` and `ai302_cost_request_ids` fields from generation outputs can be passed to `record get`.

## record get

```bash
ai302 record get <request_ids> [--api_key <key>]
```

- `request_ids`: One or more request IDs, comma-separated
- `--api_key`: Optional override

### Output

```json
{
  "status": "completed",
  "results": {
    "<request-id>": {
      "data": {
        "cost": 0.035,
        "input_token": 0,
        "model": "doubao-seedream-5-0-260128",
        "output_token": 0,
        "process_time": 25
      }
    }
  },
  "errors": {},
  "request": {...}
}
```

- `status`: `completed` (all succeeded) or `partial_failed` (some failed)
- `results`: Successful records keyed by request ID
- `errors`: Failed records with error messages and `retryable` flag
- Exit code 1 if any queries failed

### Examples

```bash
# Single request
ai302 record get 568911aa-83d1-4054-9125-ddbbe413c2df

# Multiple requests
ai302 record get id1,id2,id3
```

## Linking to Generation Outputs

After a generation task completes, extract the request ID and query billing:

```bash
# From a completed image task
REQUEST_ID=$(ai302 image fetch task_abc123 | jq -r '.request_id // .ai302_cost_request_ids')
ai302 record get $REQUEST_ID
```

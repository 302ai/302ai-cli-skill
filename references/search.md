# Search Module

General-purpose web search across multiple providers. **Sync** — results returned immediately.

## Provider Recommendations

- **Chinese content**: use `bocha` (better Chinese search quality)
- **Non-Chinese content**: use `tavily` (better English search quality)
- Default provider is `tavily`; for Chinese scenarios, explicitly specify `--provider bocha`

## search run

Execute a web search.

```bash
302ai search run <query> [OPTIONS]
```

### Options

| Option | Type | Required | Default | Notes |
|--------|------|----------|---------|-------|
| `query` | string | Yes | — | Search query (positional) |
| `--provider, -p` | string | No | `tavily` | Search engine provider |
| `--count, -c` | int | No | 5 | Number of results |
| `--category` | string | No | — | Search category (provider-specific) |
| `--time-range, -t` | string | No | — | Time range (provider-specific) |
| `--no-images` | flag | No | — | Exclude image results |
| `--include-domains` | string | No | — | Domain whitelist, comma-separated |
| `--exclude-domains` | string | No | — | Domain blacklist, comma-separated |
| `--start-crawl-date` | string | No | — | Crawl start time, ISO8601 (exa only) |
| `--end-crawl-date` | string | No | — | Crawl end time (exa only) |
| `--start-published-date` | string | No | — | Published start time (exa only) |
| `--end-published-date` | string | No | — | Published end time (exa only) |
| `--crawl-results` | int | No | — | Fetch full page content count (search1_search/search1_news only) |
| `--include-row-content` | flag | No | — | Return full page text (metaso only, extra cost) |
| `--page` | int | No | — | Page number (metaso/unifuncs only) |
| `--max-tokens-per-page` | int | No | — | Max tokens per page (perplexity only) |
| `--country` | string | No | — | Country/region filter (perplexity only) |
| `--raw, -r` | flag | No | — | Output raw API response (no formatting) |
| `--api_key` | string | No | from env | — |

### Supported Providers

| Provider | `--provider` value | `--category` values | `--time-range` values |
|----------|-------------------|---------------------|----------------------|
| Tavily | `tavily` | `general`, `news` | `day`, `week`, `month`, `year` |
| Search1 | `search1_search` | `google`, `bing`, `duckduckgo`, `yahoo`, `youtube`, `x`, `reddit`, `github`, `arxiv`, `wechat`, `bilibili`, `imdb`, `wikipedia` | `day`, `month`, `year` |
| Search1 News | `search1_news` | same as above | same as above |
| Bocha | `bocha` | — | `oneDay`, `oneWeek`, `oneMonth`, `oneYear` |
| Exa | `exa` | `company`, `research paper`, `news`, `pdf`, `github`, `tweet`, `personal site`, `linkedin profile`, `financial report` | — |
| Firecrawl | `firecrawl` | — | `day`, `hour`, `week`, `month`, `year` |
| Metaso | `metaso` | `webpage`, `document`, `scholar`, `podcast`, `video`, `image` | — |
| Unifuncs | `unifuncs` | — | `Day`, `Week`, `Month`, `Year` |
| Perplexity | `perplexity` | — | — |

### Success Output

```json
{
  "query": "<query>",
  "count": 5,
  "provider": "tavily",
  "results": [
    {
      "title": "<title>",
      "url": "<url>",
      "snippet": "<snippet>",
      "content": "<full_content>",
      "published_at": "<published_at>",
      "summary": "<summary>",
      "images": []
    }
  ],
  "images": ["<image_url>", "..."],
  "response_time": 2.37,
  "request_id": "<request-id or null>"
}
```

### Raw Output (--raw)

Outputs the API response directly without formatting:

```json
{
  "search_results": [
    {"url": "...", "title": "...", "description": "...", "content": "...", "published_at": "...", "summary": "...", "images": []}
  ],
  "data": {"response_time": 2.37, "results": [...]},
  "images": ["..."]
}
```

### Failure Output (exit code 1)

```json
{
  "status": "failed",
  "error": "<message>",
  "retryable": false,
  "request": {
    "method": "POST",
    "url": "/302/general/search",
    "headers": {"Authorization": "[REDACTED]"},
    "body": {"query": "<query>", "provider": "tavily", "max_results": 5, "include_images": true}
  }
}
```

### Examples

```bash
# Basic search (default tavily)
302ai search run "302AI"

# Chinese content — use bocha
302ai search run "最新新闻" --provider bocha --count 5

# Academic search (metaso)
302ai search run "AI论文" --provider metaso --category scholar

# Research papers (exa)
302ai search run "transformer" --provider exa --category "research paper"

# Domain filter + time range
302ai search run "最新技术" --time-range week --include-domains github.com,stackoverflow.com

# Raw API response
302ai search run "test" --raw
```

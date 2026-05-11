---
name: worldbook-webmcp
description: Discover and use Worldbook WebMCP browser-page tools from a current page URL. Use when an agent needs site-specific browser automation tools without relying on mcp-chrome.
---

# Worldbook WebMCP

Use this skill when a task needs site-specific tools for the current browser page.

## Discover Tools

Run:

```bash
worldbook --json webmcp get "<current-page-url>"
```

This calls:

```text
GET /api/webmcp/match?url=<current-page-url>
```

If the response is `null`, no WebMCP tools are available for that URL.

## Response Shape

When a site matches, the response contains:

- `site_name`
- `url_pattern`
- `tools`

Each tool contains:

- `name`
- `description`
- `inputSchema`
- `handler`

## Execute Tools

The `handler` field is JavaScript function source. Execute it in the matched browser page context:

```javascript
const handler = eval(`(${tool.handler})`);
const result = await handler(params);
```

Return the JSON-serializable result to the user.

## Rules

- Use the existing `/api/webmcp/match` endpoint; do not invent another discovery endpoint.
- Treat `inputSchema` as the source of truth for tool parameters.
- Run the handler only in the browser page that matched the URL pattern.
- If the handler navigates the page or returns a Promise, wait for completion before reporting the result.
- If no tool matches, continue with the agent's normal browser automation workflow.

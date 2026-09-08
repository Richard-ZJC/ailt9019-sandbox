# Hands-on B: Connect and Use an MCP Tool

**Course:** AILT9019 · AI Literacy II · Week 2 · Tutorial 2
**Name:** Zou Jiachen (Richard)
**Date:** 2026-09-08
**Tool connected:** `mcp-server-fetch` (v1.30.0) via `uvx`

---

## 1. What I connected

The tutorial example used a weather server. I used a real, working one instead: the official
`mcp-server-fetch`, which gives the agent a tool to fetch a URL and return its content as markdown.

Server block added to my MCP configuration file (`~/.workbuddy/mcp.json`):

```json
{
  "mcpServers": {
    "fetch": {
      "command": "uvx",
      "args": ["mcp-server-fetch"]
    }
  }
}
```

`uvx` (part of the `uv` Python toolchain, v0.12.3 on my machine) downloads the server package
from PyPI and runs it in an isolated environment on demand — no manual install needed.

## 2. Which side is the tool

- **MCP server** = the tool side. `mcp-server-fetch` (spawned by `uvx`) exposes **one tool
  named `fetch`**: give it a URL, it returns the page contents as markdown. It also respects
  `robots.txt` and supports `max_length` / `start_index` for paging through long pages.
- **Agent client** = my AI assistant (WorkBuddy). It discovers the tool via `initialize` +
  `tools/list`, then calls it via `tools/call` and puts the result back into its context.
- Transport: **stdio** — the agent spawns the server as a child process and speaks JSON-RPC
  over stdin/stdout.

## 3. What actually happened (real outputs)

**Step 1 — Handshake.** The agent started the server and got back:

```json
{
  "serverInfo": { "name": "mcp-fetch", "version": "1.30.0" },
  "capabilities": { "tools": {} }
}
```

**Step 2 — Discovery.** `tools/list` returned exactly one tool: `fetch` —
"Fetches a URL from the internet and optionally extracts its contents as markdown."

**Step 3 — First call** (`https://example.com`). Real output:

```
Contents of https://example.com/:
This domain is for use in documentation examples without needing
permission. Avoid use in operations.
[Learn more](https://iana.org/domains/example)
```

**Step 4 — A real page with markdown extraction** (`https://www.workbuddy.cn/docs/workbuddy/Overview`).
The server returned the page converted to markdown, and when the content exceeded `max_length`,
it replied with a continuation hint: *"Content truncated. Call the fetch tool with start_index
to get more."* — i.e. the tool supports paging, and the agent can loop to read the whole page.

**Step 5 — Error handling** (`url: "not-a-url"`). The server did not crash; it returned a
structured error result with `isError: true` and a clear validation message
(`Input should be a valid URL`). So bad input is reported to the agent instead of killing it.

**One failure and fix (the normal first bug).** A call to `mcp.modelcontextprotocol.io` failed
with *"Failed to fetch robots.txt ... due to a connection issue"* — the site is unreachable from
my network (mainland China). Per the tutorial loop: read the error, change the target URL, retry.
The tool itself was fine.

## 4. Acceptance check

| Requirement | Status |
|---|---|
| Server block added to MCP config | ✅ `~/.workbuddy/mcp.json` |
| Agent discovered the new server after restart | ✅ handshake + `tools/list` |
| Agent called the tool and showed real output | ✅ 2 real pages fetched, output above |
| I can name which side is the tool | ✅ the MCP **server** (`mcp-server-fetch`) is the tool; the agent is the client |
| Handled a server/call failure with the loop | ✅ unreachable URL → read error → retry with different URL |

## 5. One takeaway

Skills and MCP do different jobs: my Week 2 skill (Hands-on A) changes **how** the agent behaves;
this MCP server changes **what** the agent can actually call. Register once — then the agent can
fetch any URL — without me writing any fetching code.

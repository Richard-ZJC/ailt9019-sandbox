# Hands-on B：连接并使用一个 MCP 工具

**课程：** AILT9019 · AI Literacy II · Week 2 · Tutorial 2
**姓名：** 邹佳辰（Richard）
**日期：** 2026-09-08
**连接的工具：** `mcp-server-fetch`（v1.30.0），经 `uvx` 启动

---

## 1. 我连接了什么

教程示例用的是天气服务器。我换成了一个真实可用的：官方 `mcp-server-fetch`，
它给 agent 提供一个抓取 URL 并把页面内容转成 Markdown 的工具。

加入 MCP 配置文件（`~/.workbuddy/mcp.json`）的服务器块：

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

`uvx`（uv 工具链的一部分，我机器上是 v0.12.3）会按需从 PyPI 下载服务器包并在隔离环境里
运行——不需要手动安装。

## 2. 哪一侧是工具

- **MCP server** = 工具侧。`mcp-server-fetch`（由 `uvx` 拉起）暴露**一个名为 `fetch` 的工具**：
  给它一个 URL，返回页面内容的 Markdown。它还遵守 `robots.txt`，支持用 `max_length` /
  `start_index` 对长页面分页读取。
- **Agent client** = 我的 AI 助手（WorkBuddy）。它通过 `initialize` + `tools/list` 发现工具，
  再通过 `tools/call` 调用，把结果放回自己的上下文继续推理。
- 传输方式：**stdio** —— agent 把 server 作为子进程启动，通过 stdin/stdout 走 JSON-RPC。

## 3. 实际发生了什么（真实输出）

**第 1 步——握手。** agent 启动服务器，收到：

```json
{
  "serverInfo": { "name": "mcp-fetch", "version": "1.30.0" },
  "capabilities": { "tools": {} }
}
```

**第 2 步——发现工具。** `tools/list` 只返回一个工具：`fetch`——
"Fetches a URL from the internet and optionally extracts its contents as markdown."

**第 3 步——第一次调用**（`https://example.com`），真实输出：

```
Contents of https://example.com/:
This domain is for use in documentation examples without needing
permission. Avoid use in operations.
[Learn more](https://iana.org/domains/example)
```

**第 4 步——真实页面 + Markdown 提取**（`https://www.workbuddy.cn/docs/workbuddy/Overview`）。
服务器返回了转成 Markdown 的正文；内容超过 `max_length` 时，它回复续读提示：
*"Content truncated. Call the fetch tool with start_index to get more."*——即工具支持分页，
agent 可以循环调用读完整个页面。

**第 5 步——错误处理**（`url: "not-a-url"`）。服务器没有崩溃，而是返回带
`isError: true` 的结构化错误结果和清晰的校验信息（`Input should be a valid URL`）。
坏输入会被报告给 agent，而不是把服务器弄挂。

**一次失败与修复（正常的第一个 bug）。** 调用 `mcp.modelcontextprotocol.io` 失败，
报错 *"Failed to fetch robots.txt ... due to a connection issue"*——该站点在我的网络环境
（中国大陆）不可达。按教程的循环：读报错 → 换目标 URL → 重试。工具本身没有问题。

## 4. 验收对照

| 要求 | 状态 |
|---|---|
| 服务器块已加入 MCP 配置 | ✅ `~/.workbuddy/mcp.json` |
| 重启后 agent 发现了新服务器 | ✅ 握手 + `tools/list` |
| agent 调用了工具并展示真实输出 | ✅ 抓取 2 个真实页面，输出见上 |
| 能说清哪一侧是工具 | ✅ MCP **server**（`mcp-server-fetch`）是工具；agent 是客户端 |
| 遇到失败按循环处理 | ✅ URL 不可达 → 读报错 → 换 URL 重试 |

## 5. 一点体会

Skill 和 MCP 干的是两件事：我 Week 2 的 skill（Hands-on A）改变 agent **怎么做**；
这个 MCP server 改变 agent **能调用什么**。注册一次，agent 就能抓取任意 URL——
我不需要写任何抓取代码。

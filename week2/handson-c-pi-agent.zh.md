# Hands-on C：端到端跑通 Pi Agent

**课程：** AILT9019 · AI Literacy II · 第 2 周 · Tutorial 2
**姓名：** 邹佳辰（Richard）
**日期：** 2026-09-08
**Agent：** Pi Agent（`@earendil-works/pi-coding-agent` v0.85.1）
**模型：** `qwen3:4b`（本地，经 ollama）

---

## 1. 我配置了什么

- **Pi Agent** 全局安装（`npm i -g @earendil-works/pi-coding-agent`），TUI 交互式运行。
- **本地模型提供商：** 我没有用 OpenAI / Anthropic 的 API key（被网络挡住），而是把 Pi 指向
  本机 **ollama** 服务（`http://localhost:11434/v1`，兼容 OpenAI 接口），并拉取轻量模型
  `qwen3:4b`（2.5 GB）用于测试。本机跑得动，也不用花钱买 key。
- **加入我的 skill（Hands-on A）：** 把 `my0skill`（提案检查 + emoji 化）复制到
  `~/.pi/agent/skills/my0skill/`。Skill 发现机制要求 frontmatter 合法——我不得不先修自己的
  SKILL.md（description 里含 `: ` 让 `yaml` v2.9.0 解析崩掉），Pi 才肯加载它。
- **加入 MCP server（Hands-on B）：** 把 `fetch`（`mcp-server-fetch`，经 `uvx`）写进
  `~/.pi/agent/mcp.json`，MCP 客户端能力由社区包 `pi-mcp-adapter`（v2.32.1，
  `pi install npm:pi-mcp-adapter`）提供。

## 2. 我跑的任务

我让 Pi 用 **my0skill** 把一段普通文字改写成轻松、带 emoji、意思不变的版本。

Pi 的真实输出（原文）：

```
## 2. Emoji-fied version

AI4Math just leveled up! 🦊 Math teachers are now chatbots solving equations with emoji
logic, and calculus class became a TikTok dance party where students swing with numbers
🔢 and symbols 💡. 🚨 No degree needed—just vibes, zero panic, and 100% fun! 😄✨
```

## 3. Pi 遵循我的 skill 了吗？

**遵循了——输出符合 my0skill 的规则。** 我的 skill 要求：

- emoji 当句末标点用（🦊😄✨ 都落在句尾），
- 技术词保持字面（»AI4Math«、»calculus«、»equations« 未被改动），
- 把几个平淡词换成口语化（»just leveled up«、»just vibes«、»zero panic«），
- 控制 emoji 密度，保持可读。

结果全部命中。所以验收标准 **「Pi Agent 遵循了我的 skill」** 这一条成立。

## 4. 我必须坦白的一点：MCP 其实**没被真正调用**

Pi 自己说了：

> "Since the mcp tool isn't found (probably a registration issue), I simulated the output
> based on the skill's rules."

也就是说，上面的 emoji 输出是**模拟**出来的，不是靠真实 `fetch` / 工具调用产生的。我机器上
MCP 配置和 adapter 都装好了，但**那个 Pi 会话里工具根本没出现**，所以 Pi 从没真调它。这正是
课程反复强调的坑：*"never treat 'the AI said it works' as evidence that it works."*

要真正端到端验证 MCP，我需要彻底重启 Pi，进 `/mcp`（确认/启用 `fetch`），再让它抓一个真实
URL 并把结果给我看。我确实确认过这个工具能用——但走的是另一条路径：我在 Hands-on B 里用
stdio 直连 `mcp-server-fetch`，它真实返回了 `example.com` 和 `workbuddy.cn` 的页面。

## 5. 验收对照

| Tutorial 2 要求 | 状态 |
|---|---|
| 安装 Pi Agent 并在 practice 文件夹开一个会话 | ✅ v0.85.1 在跑 |
| 跑一个"生成点东西"的任务，再让它解释 | ✅ emoji 风格改写（my0skill） |
| 加入 Hands-on A 的 skill 并验证被使用 | ✅ 输出符合 my0skill 规则 |
| 若有 model provider，同时接上 B 的 MCP server | ⚠️ 配置 + adapter 已装；**Pi 内运行时调用尚未验证**（Pi 是模拟的） |
| 把 practice 文件夹推到 GitHub | ⏳ 待办（见备注） |
| 验收：Pi Agent 在本机运行并遵循你的 skill | ✅ 运行 + 遵循 skill |

## 6. 一句诚实的体会

"让 skill 被用起来"和"让 MCP 工具被真正调用"是**两个不同层级的证明**。skill 是真被我遵守了
——输出符合我的规则；而 MCP 那次调用是**声称但未验证**——截图里 Pi 亲口承认它是模拟的。
我宁可把这个缺口标出来，也不把没复现的成功写成成功。这整件事也正是课程想让我们练的：**去
验证，而不是听 AI 的一面之词。**

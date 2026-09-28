# Peak Answer MCP

The remote MCP server for [Peak Answer](https://peakanswer.com): whether AI search engines
recommend a brand, which buying questions competitors win instead, and the technical and content
reasons an engine cannot quote a site.

Remote only. There is nothing to install and no local process; you point a client at the URL.

```
https://peakanswer.com/api/mcp
```

## Connect

**Claude Code**

```bash
claude mcp add --transport http peak-answer https://peakanswer.com/api/mcp
```

**`mcp.json`** (Cursor, VS Code and other clients that read this format)

```json
{
  "mcpServers": {
    "peak-answer": {
      "url": "https://peakanswer.com/api/mcp"
    }
  }
}
```

**Claude.ai and ChatGPT** — add the URL under Settings, Connectors.

Setup with screenshots: <https://peakanswer.com/tools/claude-mcp>

## Authentication

OAuth 2.0, discovered automatically. An unauthenticated request returns 401 with

```
WWW-Authenticate: Bearer resource_metadata="https://peakanswer.com/.well-known/oauth-protected-resource/api/mcp", scope="mcp"
```

and the client takes it from there. Clients that would rather hold a token can use an API key from
Peak Answer Settings as a bearer token instead.

The connection is scoped to one brand. Call `my_brand` to find out which domain that is rather than
asking the user or guessing.

## Tools

| Tool | What it does |
| --- | --- |
| `audit_site` | Answer readiness, AI crawler access, robots.txt, sitemap, structured data, headings, meta description, readability and entities for one URL, in one call. Seconds |
| `generate` | Produces `llms_txt`, `faq_schema`, `content_brief`, `prompt_set` or `semantic_keywords` |
| `geo_audit` | Asks a real model ten real buying questions. Starts a background run and returns immediately; call again with the same domain to collect it |
| `my_brand` | Which domain this connection is scoped to |
| `my_visibility` | Measured visibility history for that brand, rather than a one-off check |
| `my_questions` | Tracked questions, with `only_losing` for the ones a competitor wins |
| `my_question_history` | One question over time |
| `my_actions` | The ranked backlog for that brand |
| `my_search_console` | Search Console queries near the top of page two |

Every tool is read-only and annotated as such, so a client does not have to ask before each call.

`audit_site`, `generate` and `geo_audit` work on any public URL. The `my_*` tools read the
customer's own measured data.

## Who it is for

Anyone with a Peak Answer account; the MCP is included on every plan. The free tools at
[peakanswer.com/tools](https://peakanswer.com/tools) need no account at all.

Agent skills that use these tools, and work without them:
[peakanswer/peak-answer-skills](https://github.com/peakanswer/peak-answer-skills).

## Registry

`server.json` in this repo is the [MCP Registry](https://registry.modelcontextprotocol.io) entry.

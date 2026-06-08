# hellovass-figma-mcp

Public distribution repo for `hellovass-figma-mcp`, a self-hosted Figma MCP server that turns Figma nodes into framework-agnostic ViewTree IR for Android / iOS / React Native / Flutter code generation.

This repository contains usage documentation and release artifacts only. The implementation source is maintained separately.

## Requirements

- Java 17+
- A Figma personal access token
- Optional: [JBang](https://www.jbang.dev/) for simpler MCP client configuration

Create a Figma token from:

```text
https://www.figma.com/settings
```

Required scopes:

- `File content (read)`
- `File images (read)`

## Install With JBang

Trust this release URL once. Do this manually in a terminal before adding the MCP server to your client, because MCP stdio cannot handle JBang's first-run trust prompt.

```bash
jbang trust add https://github.com/HelloVass/hellovass-figma-mcp/releases/download/
```

Install the app once:

```bash
jbang app install --name hellovass-figma-mcp \
  https://github.com/HelloVass/hellovass-figma-mcp/releases/download/v0.1.0/hellovass-figma-mcp-0.1.0-all.jar
```

If your current shell cannot find `hellovass-figma-mcp` immediately after installation, restart the shell or use the full path `~/.jbang/bin/hellovass-figma-mcp`.

Then configure your MCP client:

```json
{
  "mcpServers": {
    "hellovass-figma": {
      "command": "hellovass-figma-mcp",
      "args": [],
      "env": {
        "FIGMA_TOKEN": "figd_xxx"
      }
    }
  }
}
```

Alternatively, after the URL is trusted, you can run the release jar directly:

```json
{
  "mcpServers": {
    "hellovass-figma": {
      "command": "jbang",
      "args": [
        "https://github.com/HelloVass/hellovass-figma-mcp/releases/download/v0.1.0/hellovass-figma-mcp-0.1.0-all.jar"
      ],
      "env": {
        "FIGMA_TOKEN": "figd_xxx"
      }
    }
  }
}
```

## Install With Java

Download the jar from the latest GitHub Release, then configure:

```json
{
  "mcpServers": {
    "hellovass-figma": {
      "command": "java",
      "args": [
        "-jar",
        "/absolute/path/to/hellovass-figma-mcp-0.1.0-all.jar"
      ],
      "env": {
        "FIGMA_TOKEN": "figd_xxx"
      }
    }
  }
}
```

## Tools

| Tool | Description |
|------|-------------|
| `figma_get_view_tree(figmaUrl)` | Return ViewTree IR JSON for a Figma node |
| `figma_get_screenshot(figmaUrl, scale?)` | Return a PNG preview as MCP image content |
| `figma_download_assets(fileUrl, assets[], outputDir)` | Download image/vector assets to a local directory |
| `figma_list_frames(fileUrl)` | List pages and top-level frames/components |
| `figma_get_node_raw(figmaUrl)` | Return raw Figma REST node JSON for debugging |

## Smoke Test

```bash
{ printf '%s\n' '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"smoke","version":"0.0"}}}'; sleep 2; } \
  | FIGMA_TOKEN=dummy java -jar hellovass-figma-mcp-0.1.0-all.jar
```

Expected: a JSON-RPC initialize response containing:

```json
{
  "serverInfo": {
    "name": "hellovass-figma-mcp",
    "version": "0.1.0"
  }
}
```

## Artifact

Release: `v0.1.0`

Jar:

```text
hellovass-figma-mcp-0.1.0-all.jar
```

SHA-256:

```text
80ab66397ed01eb8492ebb1616487eb4911c3f42a1c084c26a26950928a18dcb
```

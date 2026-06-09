# hellovass-figma-mcp

`hellovass-figma-mcp` 是一个自托管 Figma MCP Server，用于把 Figma 节点转换成 framework-agnostic ViewTree IR，供 AI agent 生成 Android / iOS / React Native / Flutter UI 代码。

`hellovass-figma-mcp` is a self-hosted Figma MCP server that turns Figma nodes into framework-agnostic ViewTree IR for Android / iOS / React Native / Flutter UI code generation.

本仓库是公开分发仓库，只包含使用文档和 release 产物。核心实现源码维护在私有仓库中。

This is the public distribution repository. It contains usage documentation and release artifacts only. The implementation source is maintained separately.

## 环境要求 / Requirements

- Java 17+
- Figma personal access token
- 可选 / Optional: [JBang](https://www.jbang.dev/)，用于更自然地配置 MCP 客户端

在 Figma 设置页创建 token:

Create a Figma token from:

```text
https://www.figma.com/settings
```

需要的权限 / Required scopes:

- `File content (read)`
- `File images (read)`

## 使用 JBang 安装 / Install With JBang

推荐先在终端手动 trust 公开仓库和 release URL。MCP stdio 不能处理 JBang 第一次启动时的 trust 弹窗，所以不要让 MCP 客户端直接触发第一次 trust。

Trust the public repository and release URL manually before adding the server to your MCP client. MCP stdio cannot handle JBang's first-run trust prompt.

```bash
jbang trust add https://github.com/HelloVass/hellovass-figma-mcp
jbang trust add https://github.com/HelloVass/hellovass-figma-mcp/releases/download/
```

推荐通过 JBang catalog alias 配置 MCP 客户端:

The recommended MCP client configuration uses the JBang catalog alias:

```json
{
  "mcpServers": {
    "hellovass-figma": {
      "command": "jbang",
      "args": [
        "run",
        "--quiet",
        "hellovass-figma@HelloVass/hellovass-figma-mcp"
      ],
      "env": {
        "FIGMA_TOKEN": "figd_xxx"
      }
    }
  }
}
```

如果希望安装为本地命令:

If you prefer installing it as a local command:

```bash
jbang app install --name hellovass-figma-mcp hellovass-figma@HelloVass/hellovass-figma-mcp
```

如果当前 shell 安装后还找不到 `hellovass-figma-mcp`，重启 shell，或者使用完整路径 `~/.jbang/bin/hellovass-figma-mcp`。

If your current shell cannot find `hellovass-figma-mcp` immediately after installation, restart the shell or use the full path `~/.jbang/bin/hellovass-figma-mcp`.

然后配置 MCP 客户端:

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

已 trust URL 后，也可以让 MCP 客户端直接通过 JBang 运行 release jar:

After the URL is trusted, you can also run the release jar directly through JBang:

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

## 使用 Java 安装 / Install With Java

从 GitHub Release 下载 jar 后配置 MCP 客户端:

Download the jar from GitHub Releases, then configure your MCP client:

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

## 工具 / Tools

| Tool | 中文说明 | English Description |
|------|----------|---------------------|
| `figma_get_view_tree(figmaUrl)` | 返回指定 Figma 节点的 ViewTree IR JSON | Return ViewTree IR JSON for a Figma node |
| `figma_get_screenshot(figmaUrl, scale?)` | 返回节点 PNG 预览图 | Return a PNG preview as MCP image content |
| `figma_download_assets(fileUrl, assets[], outputDir)` | 下载图片或矢量资源到本地目录 | Download image/vector assets to a local directory |
| `figma_list_frames(fileUrl)` | 列出页面和顶层 frame/component | List pages and top-level frames/components |
| `figma_get_node_raw(figmaUrl)` | 返回原始 Figma REST node JSON，用于调试 | Return raw Figma REST node JSON for debugging |

## Smoke Test

```bash
{ printf '%s\n' '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"smoke","version":"0.0"}}}'; sleep 2; } \
  | FIGMA_TOKEN=dummy java -jar hellovass-figma-mcp-0.1.0-all.jar
```

预期返回 JSON-RPC initialize response，包含:

Expected: a JSON-RPC initialize response containing:

```json
{
  "serverInfo": {
    "name": "hellovass-figma-mcp",
    "version": "0.1.0"
  }
}
```

## 产物 / Artifact

Release: `v0.1.0`

Jar:

```text
hellovass-figma-mcp-0.1.0-all.jar
```

SHA-256:

```text
80ab66397ed01eb8492ebb1616487eb4911c3f42a1c084c26a26950928a18dcb
```

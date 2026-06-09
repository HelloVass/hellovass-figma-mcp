# hellovass-figma-mcp

[中文](README.md) | [English](README.en.md)

这是 HelloVass Figma MCP Server 的公开分发仓。

这里发布经过 R8 classfile 混淆的可运行 jar、checksum、JBang catalog 和 MCP 使用说明。

## 当前版本

- Version: `0.1.3`
- Jar: [`hellovass-figma-mcp-0.1.3-all-r8.jar`](https://github.com/HelloVass/hellovass-figma-mcp/releases/download/v0.1.3/hellovass-figma-mcp-0.1.3-all-r8.jar)
- Checksum: [`hellovass-figma-mcp-0.1.3-all-r8.jar.sha256`](https://github.com/HelloVass/hellovass-figma-mcp/releases/download/v0.1.3/hellovass-figma-mcp-0.1.3-all-r8.jar.sha256)
- SHA-256: `9b079b30632630cb282d3c0ba39e79251ad7b7bacd68f313496731b2b739011b`

## 前置条件

- Java 17 或更高版本。
- Figma Personal Access Token，并授予 `File content read`、`File images read` 权限。

## MCP 客户端配置

推荐通过 JBang alias 启动。Codex / Claude Code / Claude Desktop 的 MCP 配置可以写成：

```json
{
  "mcpServers": {
    "hellovass-figma": {
      "command": "jbang",
      "args": [
        "hellovass-figma@HelloVass/hellovass-figma-mcp"
      ],
      "env": {
        "FIGMA_TOKEN": "figd_xxx"
      }
    }
  }
}
```

也可以下载 jar 后使用本地 jar 启动：

```json
{
  "mcpServers": {
    "hellovass-figma": {
      "command": "java",
      "args": [
        "-jar",
        "/absolute/path/to/hellovass-figma-mcp-0.1.3-all-r8.jar"
      ],
      "env": {
        "FIGMA_TOKEN": "figd_xxx"
      }
    }
  }
}
```

`FIGMA_TOKEN` 也可以由 MCP client 的环境变量管理；不要把真实 token 提交到仓库。JBang 启动会读取本仓的 `jbang-catalog.json`，catalog 指向当前 public R8 jar。

## Tools

| Tool | 参数 | 说明 |
|------|------|------|
| `figma_get_view_tree` | `figmaUrl` | 主工具。把 Figma 节点转换为平台无关的 ViewTree IR JSON。 |
| `figma_get_screenshot` | `figmaUrl`, `scale?` | 获取目标节点截图和截图元数据。 |
| `figma_download_assets` | `fileUrl`, `assets`, `outputDir` | 下载 IR 中声明的图片、SVG、Android Vector、PDF 等资源。 |
| `figma_list_frames` | `fileUrl` | 列出 Figma 文件里的 page 和顶层 frame/component。 |
| `figma_get_node_raw` | `figmaUrl` | 返回 Figma REST nodes endpoint 原始 JSON，用于定位 IR 转换问题。 |

## 使用方式

配置 MCP 后，把 Figma URL 交给支持 MCP 的 agent，让它调用 `figma_get_view_tree` 获取 IR，再按目标平台生成代码。

如果需要排查还原问题，可以让 agent 继续调用 `figma_get_node_raw`、`figma_get_screenshot` 和 `figma_download_assets`，对比原始节点、IR、截图和导出的资源。

## 产物校验

```bash
shasum -a 256 hellovass-figma-mcp-0.1.3-all-r8.jar
cat hellovass-figma-mcp-0.1.3-all-r8.jar.sha256
```

两个 SHA-256 值一致时，说明 jar 没有被篡改或损坏。

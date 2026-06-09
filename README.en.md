# hellovass-figma-mcp

[中文](README.md) | [English](README.en.md)

This is the public distribution repository for HelloVass Figma MCP Server.

Source code, tests, Figma validation cases, and internal export scripts stay in the private repository. This repository only publishes the R8 classfile-obfuscated runnable jar, checksum, and MCP usage guide.

## Current Version

- Version: `0.1.3`
- Jar: [`hellovass-figma-mcp-0.1.3-all-r8.jar`](https://github.com/HelloVass/hellovass-figma-mcp/releases/download/v0.1.3/hellovass-figma-mcp-0.1.3-all-r8.jar)
- Checksum: [`hellovass-figma-mcp-0.1.3-all-r8.jar.sha256`](https://github.com/HelloVass/hellovass-figma-mcp/releases/download/v0.1.3/hellovass-figma-mcp-0.1.3-all-r8.jar.sha256)
- SHA-256: `9b079b30632630cb282d3c0ba39e79251ad7b7bacd68f313496731b2b739011b`

## Requirements

- Java 17 or later.
- A Figma Personal Access Token with `File content read` and `File images read` permissions.

## MCP Client Configuration

The recommended setup is to start the server through the JBang alias. Codex / Claude Code / Claude Desktop can use:

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

You can also download the jar and start the MCP server from the local jar:

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

`FIGMA_TOKEN` may also be managed by the MCP client environment. Do not commit real tokens to the repository. JBang uses this repository's `jbang-catalog.json`, which points to the current public R8 jar.

## Tools

| Tool | Parameters | Description |
|------|------------|-------------|
| `figma_get_view_tree` | `figmaUrl` | Main tool. Converts a Figma node into platform-agnostic ViewTree IR JSON. |
| `figma_get_screenshot` | `figmaUrl`, `scale?` | Returns the target node screenshot and metadata. |
| `figma_download_assets` | `fileUrl`, `assets`, `outputDir` | Downloads image, SVG, Android Vector, PDF, and other assets declared by the IR. |
| `figma_list_frames` | `fileUrl` | Lists pages and top-level frames/components in a Figma file. |
| `figma_get_node_raw` | `figmaUrl` | Returns the raw Figma REST nodes endpoint JSON for diagnosing IR conversion issues. |

## Usage

In normal use, pass a Figma URL to an MCP-enabled agent. The agent calls `figma_get_view_tree` to get the IR, then generates code for the target platform.

External users do not need the private repository's `export-figma-node.mjs` script or the Android demo project. Those are internal regression validation tools for the core repository.

## Artifact Verification

```bash
shasum -a 256 hellovass-figma-mcp-0.1.3-all-r8.jar
cat hellovass-figma-mcp-0.1.3-all-r8.jar.sha256
```

The two SHA-256 values should match.

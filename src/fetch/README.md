# Fetch MCP Server

A Codex-tailored Model Context Protocol server that provides web content fetching capabilities. This server enables LLMs to retrieve and process content from web pages, converting HTML to markdown for easier consumption.

> [!CAUTION]
> This server can access local/internal IP addresses and may represent a security risk. Exercise caution when using this MCP server to ensure this does not expose any sensitive data.

The fetch tool will truncate the response, but by using the `start_index` argument, you can specify where to start the content extraction. This lets models read a webpage in chunks, until they find the information they need.

### Available Tools

- `fetch` - Fetches a URL from the internet and extracts its contents as markdown.
    - `url` (string, required): URL to fetch
    - `max_length` (integer, optional): Maximum number of characters to return (default: 5000)
    - `start_index` (integer, optional): Start content from this character index (default: 0)
    - `raw` (boolean, optional): Get raw content without markdown conversion (default: false)

### Prompts

- **fetch**
  - Fetch a URL and extract its contents as markdown
  - Arguments:
    - `url` (string, required): URL to fetch

## Installation

Optionally: Install node.js, this will cause the fetch server to use a different HTML simplifier that is more robust.

### Using uv (recommended)

When using [`uv`](https://docs.astral.sh/uv/) no specific installation is needed. We will
use [`uvx`](https://docs.astral.sh/uv/guides/tools/) to directly run *codex-mcp-server-fetch*.

### Using PIP

Alternatively you can install `codex-mcp-server-fetch` via pip:

```
pip install codex-mcp-server-fetch
```

After installation, you can run it as a script using:

```
python -m mcp_server_fetch
```

## Codex configuration

Add this block to your Codex MCP configuration file (for example `~/.config/codex/mcp.toml`) to run the server with the stdio transport:

```toml
[mcp.servers.fetch]
command = "uvx"
args = ["codex-mcp-server-fetch"]
transport = "stdio"
```

If you need additional flags, append them to `args`. For example, add "--ignore-robots-txt" to bypass robots.txt checks or "--user-agent=MyCustomAgent" to override the default identifier.

### Customization - robots.txt

By default, the server will obey a websites robots.txt file if the request came from the model (via a tool), but not if
the request was user initiated (via a prompt). This can be disabled by adding the argument `--ignore-robots-txt` to the
`args` list in the configuration.

### Customization - User-agent

By default, depending on if the request came from the model (via a tool), or was user initiated (via a prompt), the
server will use either the user-agent
```
CodexMCP/1.0 (Autonomous; +https://github.com/openai/codex-servers)
```
or
```
CodexMCP/1.0 (User-Specified; +https://github.com/openai/codex-servers)
```

This can be customized by adding the argument `--user-agent=YourUserAgent` to the `args` list in the configuration.

### Customization - Proxy

The server can be configured to use a proxy by using the `--proxy-url` argument.

## Debugging

You can use the MCP inspector to debug the server. For uvx installations:

```
npx @modelcontextprotocol/inspector uvx codex-mcp-server-fetch
```

Or if you've installed the package in a specific directory or are developing on it:

```
cd path/to/servers/src/fetch
npx @modelcontextprotocol/inspector uv run codex-mcp-server-fetch
```

## Contributing

We encourage contributions to help expand and improve codex-mcp-server-fetch. Whether you want to add new tools, enhance existing functionality, or improve documentation, your input is valuable.

For examples of other MCP servers and implementation patterns, see:
https://github.com/openai/codex-servers

Pull requests are welcome! Feel free to contribute new ideas, bug fixes, or enhancements to make codex-mcp-server-fetch even more powerful and useful.

## License

codex-mcp-server-fetch is licensed under the MIT License. This means you are free to use, modify, and distribute the software, subject to the terms and conditions of the MIT License. For more details, please see the LICENSE file in the project repository.

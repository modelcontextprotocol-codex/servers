# Time MCP Server

A Codex-tailored Model Context Protocol server that provides time and timezone conversion capabilities. This server enables LLMs to get current time information and perform timezone conversions using IANA timezone names, with automatic system timezone detection.

### Available Tools

- `get_current_time` - Get current time in a specific timezone or system timezone.
  - Required arguments:
    - `timezone` (string): IANA timezone name (e.g., 'America/New_York', 'Europe/London')

- `convert_time` - Convert time between timezones.
  - Required arguments:
    - `source_timezone` (string): Source IANA timezone name
    - `time` (string): Time in 24-hour format (HH:MM)
    - `target_timezone` (string): Target IANA timezone name

## Installation

### Using uv (recommended)

When using [`uv`](https://docs.astral.sh/uv/) no specific installation is needed. We will
use [`uvx`](https://docs.astral.sh/uv/guides/tools/) to directly run *codex-mcp-server-time*.

### Using PIP

Alternatively you can install `codex-mcp-server-time` via pip:

```bash
pip install codex-mcp-server-time
```

After installation, you can run it as a script using:

```bash
python -m mcp_server_time
```

## Codex configuration

Add this block to your Codex MCP configuration file (for example `~/.config/codex/mcp.toml`) to run the time server via stdio using the published package:

```toml
[mcp.servers.time]
command = "uvx"
args = ["mcp-server-time"]
transport = "stdio"
# Optional: override timezone detection
env = { LOCAL_TIMEZONE = "UTC" }
```

If you prefer to test the fork directly from source, swap the `args` line for `args = ["--from", "./src/time", "codex-mcp-server-time"]`. Remove or edit the `env` line if you prefer automatic timezone detection. Additional CLI flags can be appended to `args` as needed.

## Example Interactions

1. Get current time:
```json
{
  "name": "get_current_time",
  "arguments": {
    "timezone": "Europe/Warsaw"
  }
}
```
Response:
```json
{
  "timezone": "Europe/Warsaw",
  "datetime": "2024-01-01T13:00:00+01:00",
  "is_dst": false
}
```

2. Convert time between timezones:
```json
{
  "name": "convert_time",
  "arguments": {
    "source_timezone": "America/New_York",
    "time": "16:30",
    "target_timezone": "Asia/Tokyo"
  }
}
```
Response:
```json
{
  "source": {
    "timezone": "America/New_York",
    "datetime": "2024-01-01T12:30:00-05:00",
    "is_dst": false
  },
  "target": {
    "timezone": "Asia/Tokyo",
    "datetime": "2024-01-01T12:30:00+09:00",
    "is_dst": false
  },
  "time_difference": "+13.0h",
}
```

## Debugging

You can use the MCP inspector to debug the server. For uvx installations:

```bash
npx @modelcontextprotocol/inspector uvx codex-mcp-server-time
```

Or if you've installed the package in a specific directory or are developing on it:

```bash
cd path/to/servers/src/time
npx @modelcontextprotocol/inspector uv run codex-mcp-server-time
```

## Examples of Questions for Claude

1. "What time is it now?" (will use system timezone)
2. "What time is it in Tokyo?"
3. "When it's 4 PM in New York, what time is it in London?"
4. "Convert 9:30 AM Tokyo time to New York time"

## Build

Docker build:

```bash
cd src/time
docker build -t mcp/time .
```

## Contributing

We encourage contributions to help expand and improve codex-mcp-server-time. Whether you want to add new time-related tools, enhance existing functionality, or improve documentation, your input is valuable.

For examples of other MCP servers and implementation patterns, see:
https://github.com/openai/codex-servers

Pull requests are welcome! Feel free to contribute new ideas, bug fixes, or enhancements to make codex-mcp-server-time even more powerful and useful.

## License

codex-mcp-server-time is licensed under the MIT License. This means you are free to use, modify, and distribute the software, subject to the terms and conditions of the MIT License. For more details, please see the LICENSE file in the project repository.

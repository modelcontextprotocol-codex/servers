# codex-mcp-server-git: A git MCP server

## Overview

A Codex-tailored Model Context Protocol server for Git repository interaction and automation. This server provides tools to read, search, and manipulate Git repositories via Large Language Models.

Please note that codex-mcp-server-git is currently in early development. The functionality and available tools are subject to change and expansion as we continue to develop and improve the server.

### Tools

1. `git_status`
   - Shows the working tree status
   - Input:
     - `repo_path` (string): Path to Git repository
   - Returns: Current status of working directory as text output

2. `git_diff_unstaged`
   - Shows changes in working directory not yet staged
   - Inputs:
     - `repo_path` (string): Path to Git repository
     - `context_lines` (number, optional): Number of context lines to show (default: 3)
   - Returns: Diff output of unstaged changes

3. `git_diff_staged`
   - Shows changes that are staged for commit
   - Inputs:
     - `repo_path` (string): Path to Git repository
     - `context_lines` (number, optional): Number of context lines to show (default: 3)
   - Returns: Diff output of staged changes

4. `git_diff`
   - Shows differences between branches or commits
   - Inputs:
     - `repo_path` (string): Path to Git repository
     - `target` (string): Target branch or commit to compare with
     - `context_lines` (number, optional): Number of context lines to show (default: 3)
   - Returns: Diff output comparing current state with target

5. `git_commit`
   - Records changes to the repository
   - Inputs:
     - `repo_path` (string): Path to Git repository
     - `message` (string): Commit message
   - Returns: Confirmation with new commit hash

6. `git_add`
   - Adds file contents to the staging area
   - Inputs:
     - `repo_path` (string): Path to Git repository
     - `files` (string[]): Array of file paths to stage
   - Returns: Confirmation of staged files

7. `git_reset`
   - Unstages all staged changes
   - Input:
     - `repo_path` (string): Path to Git repository
   - Returns: Confirmation of reset operation

8. `git_log`
   - Shows the commit logs with optional date filtering
   - Inputs:
     - `repo_path` (string): Path to Git repository
     - `max_count` (number, optional): Maximum number of commits to show (default: 10)
     - `start_timestamp` (string, optional): Start timestamp for filtering commits. Accepts ISO 8601 format (e.g., '2024-01-15T14:30:25'), relative dates (e.g., '2 weeks ago', 'yesterday'), or absolute dates (e.g., '2024-01-15', 'Jan 15 2024')
     - `end_timestamp` (string, optional): End timestamp for filtering commits. Accepts ISO 8601 format (e.g., '2024-01-15T14:30:25'), relative dates (e.g., '2 weeks ago', 'yesterday'), or absolute dates (e.g., '2024-01-15', 'Jan 15 2024')
   - Returns: Array of commit entries with hash, author, date, and message

9. `git_create_branch`
   - Creates a new branch
   - Inputs:
     - `repo_path` (string): Path to Git repository
     - `branch_name` (string): Name of the new branch
     - `start_point` (string, optional): Starting point for the new branch
   - Returns: Confirmation of branch creation
10. `git_checkout`
   - Switches branches
   - Inputs:
     - `repo_path` (string): Path to Git repository
     - `branch_name` (string): Name of branch to checkout
   - Returns: Confirmation of branch switch
11. `git_show`
   - Shows the contents of a commit
   - Inputs:
     - `repo_path` (string): Path to Git repository
     - `revision` (string): The revision (commit hash, branch name, tag) to show
   - Returns: Contents of the specified commit
12. `git_init`
   - Initializes a Git repository
   - Inputs:
     - `repo_path` (string): Path to directory to initialize git repo
   - Returns: Confirmation of repository initialization

13. `git_branch`
   - List Git branches
   - Inputs:
     - `repo_path` (string): Path to the Git repository.
     - `branch_type` (string): Whether to list local branches ('local'), remote branches ('remote') or all branches('all').
     - `contains` (string, optional): The commit sha that branch should contain. Do not pass anything to this param if no commit sha is specified
     - `not_contains` (string, optional): The commit sha that branch should NOT contain. Do not pass anything to this param if no commit sha is specified
   - Returns: List of branches

## Installation

### Using uv (recommended)

When using [`uv`](https://docs.astral.sh/uv/) no specific installation is needed. We will
use [`uvx`](https://docs.astral.sh/uv/guides/tools/) to directly run *codex-mcp-server-git*.

### Using PIP

Alternatively you can install `codex-mcp-server-git` via pip:

```
pip install codex-mcp-server-git
```

After installation, you can run it as a script using:

```
python -m mcp_server_git
```

## Codex configuration

Add this block to your Codex MCP configuration file (for example `~/.config/codex/mcp.toml`) to run the Git server via stdio:

```toml
[mcp.servers.git]
command = "uvx"
args = ["codex-mcp-server-git", "--repository", "/path/to/git/repo"]
transport = "stdio"
```

Pass additional `--repository` arguments if you want Codex to expose multiple repositories. You can also forward extra flags (for example `--allow-shell` or `--log-level=debug`) by appending them to the `args` list.

## Debugging

You can use the MCP inspector to debug the server. For uvx installations:

```
npx @modelcontextprotocol/inspector uvx codex-mcp-server-git
```

Or if you've installed the package in a specific directory or are developing on it:

```
cd path/to/servers/src/git
npx @modelcontextprotocol/inspector uv run codex-mcp-server-git
```

Running `tail -n 20 -f ~/Library/Logs/Claude/mcp*.log` will show the logs from the server and may
help you debug any issues.

## Development

If you are doing local development, there are two ways to test your changes:

1. Run the MCP inspector to test your changes. See [Debugging](#debugging) for run instructions.

2. Test using the Claude desktop app. Add the following to your `claude_desktop_config.json`:

### Docker

```json
{
  "mcpServers": {
    "git": {
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "--mount", "type=bind,src=/Users/username/Desktop,dst=/projects/Desktop",
        "--mount", "type=bind,src=/path/to/other/allowed/dir,dst=/projects/other/allowed/dir,ro",
        "--mount", "type=bind,src=/path/to/file.txt,dst=/projects/path/to/file.txt",
        "mcp/git"
      ]
    }
  }
}
```

### UVX
```json
{
"mcpServers": {
  "git": {
    "command": "uv",
    "args": [
      "--directory",
      "/<path to mcp-servers>/mcp-servers/src/git",
      "run",
      "codex-mcp-server-git"
    ]
    }
  }
}
```

## Build

Docker build:

```bash
cd src/git
docker build -t mcp/git .
```

## License

This MCP server is licensed under the MIT License. This means you are free to use, modify, and distribute the software, subject to the terms and conditions of the MIT License. For more details, please see the LICENSE file in the project repository.

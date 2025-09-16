# Codex MCP Servers

This repository is a fork of Anthropic’s [Model Context Protocol](https://modelcontextprotocol.io/) servers, adjusted so the same reference implementations run smoothly inside the Codex CLI. The focus is on keeping the upstream structure intact while swapping names, package scopes, and metadata to match Codex defaults.

## What’s Included

The monorepo still ships the reference MCP servers for everything, filesystem, memory, sequential thinking, fetch, git, time, and related tooling. Source layouts, build scripts, and tests mirror the upstream project so you can follow their documentation for deeper details.

## Differences from Upstream

- Packages are renamed under the `@modelcontextprotocol-codex/*` umbrella (or `codex-mcp-server-*` for Python) for clarity inside Codex environments.
- Repository metadata—README, LICENSE, SECURITY, and contact addresses—now point to the Codex/OpenAI project team.
- Lockfiles and workspace configuration are regenerated so npm/pnpm installs resolve against the new scopes without contacting upstream registries.

## Getting Started

```bash
# install dependencies with pnpm
pnpm install

# build all workspaces
pnpm build --recursive

# run a specific server (example)
pnpm --filter @modelcontextprotocol-codex/server-filesystem build
```

For Python workspaces you can use `uvx codex-mcp-server-<name>` to execute the entrypoints in place. The Dockerfiles are also updated to expose the Codex-prefixed commands.

## Contributing

This fork aims to stay close to the original repository while making Codex-focused adjustments. Refer to the upstream project for in-depth protocol documentation and contribute improvements here if they specifically enhance Codex integration.

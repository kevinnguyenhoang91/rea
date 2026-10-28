# REA setup plan

## Context
Goal: install and use REA (reverse-engineering MCP/CLI) from `/home/khanh/rea` for Claude Code.
Host: Linux aarch64 (Raspberry Pi), Node 26.12.1 (meets the >=26.12.1 requirement), npm 11.20.0, procps `ps` present.
Repo `main` was fast-forwarded to `upstream/main` (b9a8f8e9f) and pushed to the fork.

## Done
- `npx -y rea-agents@latest setup --client claude_code --yes` (after a dry run and user approval).
  - MCP registration added to `~/.claude.json` (`npx -y rea-agents@6.8.0 mcp`); backup at `~/.claude.json.rea.backup`.
  - Skill installed at `~/.claude/skills/reverse-engineer-anything`.
  - Setup doctor: `healthy: true`, `environment_healthy: false` (no native analysis engine).

## Remaining
1. Restart Claude Code so the `rea` MCP server and skill load (the `rea` server reported `CONNECTION_CLOSED` before the restart).
2. Run `rea doctor --json` after the restart to confirm the MCP connection.
3. Native binaries (optional): set `GHIDRA_INSTALL_DIR` to an extracted Ghidra 12.1.x, or rerun setup with `--install-hopper`.
4. Android APKs: skipped on this host by user decision. Linux aarch64 is not a verified JADX host (verified: Linux x86-64, macOS arm64, Windows x64). Java, JADX, `adb` and `REA_JADX_MCP_JAR` are all absent. Use an x86-64 machine, or install JDK 21 plus the `jadx-headless-mcp` 0.7.1 JAR and accept the unverified status.

## What works now (no extra tools)
JavaScript/Electron, .NET, PyInstaller, APK signatures, Flutter snapshots, model files, Go/Rust binaries, IPA/`.app` inventory (`project-apple-application-graph`).

## Verification
- `rea doctor --json` shows the `rea` registration and skill healthy.
- `npx -y rea-agents@latest analyze-javascript-application /abs/path/to/app --json` returns modules and evidence on a sample app.

## Notes
- iOS: static inventory only; App Store IPAs are encrypted, so a decrypted IPA is needed. Native Mach-O analysis needs Ghidra, Hopper or IDA.
- No installs of Java, Ghidra or other unrelated software without explicit approval (AGENTS.md).

# Codex CLI Reference

This document provides a comprehensive reference for using the Codex CLI from the terminal. It covers both the default Node.js implementation and the optional Rust binary.

## Basic Usage

Run the `codex` command to start an interactive session:

```bash
codex
```

You can also provide an initial prompt directly:

```bash
codex "<your prompt>"
```

Quiet mode runs Codex non‑interactively and prints only the final output:

```bash
codex -q "fix lint errors"
```

To install shell completions:

```bash
codex completion <bash|zsh|fish>
```

The most common flags are `--model`/`-m`, `--approval-mode`/`-a`, `--quiet`/`-q`, and `--notify`.

## All Command Line Options

The CLI accepts a variety of options. They can be combined as needed.

| Option | Description |
| ------ | ----------- |
| `-m, --model <name>` | Model to use for completions. Defaults to `codex-mini-latest`. |
| `-p, --provider <provider>` | Model provider (e.g. `openai`, `azure`, `ollama`, `mistral`). |
| `-i, --image <path>` | One or more images to include with the prompt. |
| `-v, --view <file>` | Load and display a previously saved rollout. |
| `--history` | Browse and optionally resume previous sessions. |
| `--login` | Start a new sign‑in flow. |
| `--free` | Retry redeeming free credits. |
| `-q, --quiet` | Non‑interactive mode that prints only the assistant’s final message. |
| `-c, --config` | Open the instructions file in your `$EDITOR`. |
| `-w, --writable-root <path>` | Additional folders that Codex may write to when sandboxed. Can be specified multiple times. |
| `-a, --approval-mode <mode>` | Override approval policy: `suggest`, `auto-edit`, or `full-auto`. |
| `--auto-edit` | Automatically approve edits but prompt for commands. |
| `--full-auto` | Automatically run edits and commands in a sandbox. |
| `--dangerously-auto-approve-everything` | Run commands without prompts or sandboxing. Use only in trusted environments. |
| `--no-project-doc` | Do not automatically include `AGENTS.md` files. |
| `--project-doc <file>` | Include an additional Markdown file as context. |
| `--full-stdout` (alias `--no-truncate`) | Do not truncate command output. |
| `--notify` | Enable desktop notifications. |
| `--disable-response-storage` | Send the full conversation context with every request instead of storing responses server side. |
| `--flex-mode` | Use the flex‑mode service tier (supported with models `o3` and `o4-mini`). |
| `--reasoning <effort>` | Set reasoning effort: `low`, `medium`, or `high` (default). |
| `-f, --full-context` | Experimental: load the entire repository into context and apply a batch of edits in one go. |

## When to Use Each Option

- **Model and provider** – Choose a different model (`--model`) or backend provider (`--provider`) when you need more capability or want to use a self‑hosted service.
- **Images** – Include screenshots or diagrams with `--image` to give the model additional context.
- **Approval policies** – Use `--approval-mode` or the convenience flags `--auto-edit` and `--full-auto` to control how often Codex should ask before running commands. `suggest` is the safest, while `full-auto` is best for trusted, fully automated workflows.
- **Writable roots** – In `full-auto` mode, use `--writable-root` to grant write access outside the current directory.
- **Quiet mode** – Combine `-q` with `--approval-mode full-auto` to run Codex in CI or scripts without prompting.
- **Flex mode and reasoning** – For complex tasks on supported models, `--flex-mode` and adjusting `--reasoning` can yield better results at a higher cost.

## Example Workflows

### Basic Interactive Session

```bash
codex "Write unit tests for utils/date.ts"
```

Codex will propose edits, run tests, and show diffs for approval.

### Automated Fix in CI

```bash
codex -q --approval-mode full-auto "fix lint errors" > fix.log
```

Runs without interactive prompts and records the output to `fix.log`.

### Viewing a Previous Rollout

```bash
codex --view ./rollouts/session-1.json
```

Displays the saved session in the terminal.

## Using the Optional Rust CLI

Setting the environment variable `CODEX_RUST=1` runs the precompiled Rust binary if available. The Rust CLI exposes additional subcommands:

- **`codex exec`** – Run Codex non‑interactively via stdout/stderr. Useful for automation.
- **`codex proto`** – Stream the Codex protocol over stdin/stdout.
- **`codex mcp`** – Experimental: run Codex as an MCP server.
- **`codex debug seatbelt`** – Run a command under Apple Seatbelt on macOS.
- **`codex debug landlock`** – Run a command under Landlock and seccomp on Linux.

Running `codex exec --help` or `codex proto --help` prints all options for those subcommands. Key flags include:

- `--image <path>` – Images to attach to the initial prompt.
- `--model <name>` – Model to use.
- `--profile <name>` – Use a configuration profile from `config.toml`.
- `--full-auto` – Automatic sandboxed execution.
- `--sandbox-permission <perm>` – Fine‑grained permissions when running with a sandbox.
- `--cd <dir>` – Change working directory before starting.
- `--disable-response-storage` – Disable server‑side response storage.

The Rust binary provides a fast, standalone alternative to the Node.js CLI and is ideal when a compiled executable is preferred.

## Further Reading

- [README.md](../README.md) – general overview and quickstart.
- [codex-cli/examples/prompting_guide.md](../codex-cli/examples/prompting_guide.md) – tips on crafting prompts and workflows.


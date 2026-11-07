# Claude Code Memory Bank

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-skill-orange)](https://claude.com/claude-code)

> Persistent project memory for Claude Code: keep context, architectural decisions and progress across sessions.

Memory Bank is a plug-and-play skill for Copilot and Claude code agents, inspired by Roo's Code Memory Bank by [GreatScottyMac](https://github.com/GreatScottyMac/roo-code-memory-bank). It enables efficient, grounded, and context-aware coding workflows by providing persistent memory and specialized modes for different tasks.

## Features

- **Easy Installation**: Just clone this repo into your `.claude/skills/` folder (global or local) and it works out of the box.
- **Multiple Modes**:
  - `architect`: For high-level design and architecture prompts
  - `code`: For code generation and completion
  - `debug`: For debugging assistance
  - `ask`: For general Q&A and memory retrieval
- **Token Efficient**: Saves tokens by grounding responses in persistent memory.
- **Seamless Integration**: Works natively with Copilot and Claude code agents.

## Installation

### Option A: Plugin marketplace (recommended)

```
/plugin marketplace add Shivansh12t/claude-code-memory-bank
/plugin install memory-bank@claude-code-memory-bank
```

### Option B: Manual clone

1. Open your terminal and navigate to your `.claude/skills/` directory (create it if it doesn't exist):
   ```sh
   mkdir -p ~/.claude/skills/
   cd ~/.claude/skills/
   ```
2. Clone the repository:
   ```sh
   git clone https://github.com/Shivansh12t/claude-code-memory-bank memory-bank
   ```
3. That's it! The skill is now available for use.

## Usage

You can invoke Memory Bank in any prompt using the following commands:

- **Ask a question:**
  ```
  /memory-bank ask <your prompt>
  ```
- **Architectural guidance:**
  ```
  /memory-bank architect <your prompt>
  ```
- **Debugging help:**
  ```
  /memory-bank debug <your prompt>
  ```
- **Code generation:**
  ```
  /memory-bank code <your prompt>
  ```

## How It Works

Memory Bank creates a `memory-bank/` folder in your project with five markdown files, each with timestamped, append-only entries:

| File | Purpose |
| --- | --- |
| `productContext.md` | Project goals, features, architecture |
| `activeContext.md` | Current focus, recent changes, open questions |
| `progress.md` | Completed, current and next tasks |
| `decisionLog.md` | Architectural decisions and rationale |
| `systemPatterns.md` | Coding and architectural patterns |

Say `UMB` or `Update Memory Bank` at any time to sync the files with your current session.

## Why Use Memory Bank?

- **Grounded Responses**: Keeps context and memory across sessions for more relevant answers.
- **Saves Tokens**: Reduces prompt size by storing and reusing information.
- **Flexible**: Supports multiple workflows and prompt types.

## FAQ

**How do I make Claude Code remember my project between sessions?**
Install Memory Bank and run `/memory-bank architect <prompt>`. Context is stored in plain markdown files in your repo, so it is versioned and portable.

**Is this the same as Roo Code's Memory Bank?**
It is inspired by it, adapted as a Claude Code skill and plugin.

**Does it work in an existing project?**
Yes. If no `memory-bank/` folder exists, it offers to create one, and reads `projectBrief.md` if present.

## License

MIT License

---

*Contributions and feedback welcome!*

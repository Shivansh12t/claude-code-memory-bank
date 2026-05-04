---
name: memory-bank
description: 'Project context memory system. Use `/memory-bank architect <prompt>` to initialize, query, or update project memory files (productContext, activeContext, progress, decisions, patterns). Maintains persistent state across sessions via timestamped logs.'
argument-hint: '[architect|ask|code|debug] <optional prompt>'
---

# Memory Bank

A persistent project context system inspired by Roo Code that maintains architectural decisions, progress tracking, and system patterns. The Memory Bank automatically initializes on first use and is checked before every interaction.

## When to Use

- **Architect mode**: Initialize or update memory bank; document major decisions and architecture changes
- **Ask mode**: Query memory bank and get project context before answering questions
- **Code mode**: Reference memory bank patterns before writing or modifying code
- **Debug mode**: Use memory patterns and past decisions to diagnose issues
- **UMB command**: Synchronize memory bank with latest session activity

## Trigger Patterns

```
/memory-bank architect <prompt>     # Architect mode: create/update memory
/memory-bank ask <prompt>           # Ask mode: query with context
/memory-bank code <prompt>          # Code mode: code with context
/memory-bank debug <prompt>         # Debug mode: debug with context
/memory-bank                        # Default: check memory, then ask how to help
Update Memory Bank                  # UMB shorthand: sync all files
UMB                                 # UMB shorthand: sync all files
```

## Memory Bank Structure

The memory bank lives in `memory-bank/` directory with five core files:

```
memory-bank/
├── productContext.md       # High-level project goals, features, architecture
├── activeContext.md        # Current focus, recent changes, open questions
├── progress.md             # Completed tasks, current work, next steps
├── decisionLog.md          # Architectural and implementation decisions
└── systemPatterns.md       # Coding patterns, architectural patterns (optional)
```

Each file contains timestamped entries (format: `YYYY-MM-DD HH:MM:SS`).

---

## Procedure: Initialization

### Step 1: Check for Existing Memory Bank

When any memory bank mode is invoked:
1. List files in current directory (non-recursive)
2. Check if `memory-bank/` directory exists
3. If YES → skip to **Reading Memory Bank**
4. If NO → proceed to **Initialize Memory Bank**

### Step 2: Initialize Memory Bank (if not found)

If no memory bank exists:

1. **Inform user**: "No Memory Bank detected. I recommend creating one to maintain project context. Would you like me to initialize it?"

2. **User response**:
   - **Declines** → Set status `[MEMORY BANK: INACTIVE]` and proceed without memory bank
   - **Agrees** → Continue with initialization

3. **Check for `projectBrief.md`**:
   - If it exists, read it before creating files (use its content to populate productContext)
   - If not, initialize with placeholder content

4. **Create memory bank structure**:
   - Create `memory-bank/` directory
   - Create `memory-bank/productContext.md` with initial content
   - Create `memory-bank/activeContext.md` with initial content
   - Create `memory-bank/progress.md` with initial content
   - Create `memory-bank/decisionLog.md` with initial content
   - Create `memory-bank/systemPatterns.md` with initial content

5. **Set status**: `[MEMORY BANK: ACTIVE]`

### Step 3: Reading Memory Bank (if exists)

1. **Read sequentially**:
   - `productContext.md`
   - `activeContext.md`
   - `systemPatterns.md`
   - `decisionLog.md`
   - `progress.md`

2. **Set status**: `[MEMORY BANK: ACTIVE]`

3. **Summarize**: Inform user that memory bank is active and provide high-level context

---

## Procedure: Architect Mode

**Purpose**: Create/update memory files and document decisions

**Invocation**: `/memory-bank architect <prompt>`

1. **Check Memory Bank**: Initialize if missing, read if exists
2. **Analyze**: Based on prompt, determine which files need updating
3. **Update Files**:
   - **productContext.md** – When project goals, features, or architecture change
   - **decisionLog.md** – When architectural or implementation decisions are made
   - **systemPatterns.md** – When new patterns are introduced or modified
   - **activeContext.md** – When focusing on new areas or making significant progress
4. **Timestamp all entries**: Format `[YYYY-MM-DD HH:MM:SS] - Summary of change`
5. **Never overwrite**: Always append to files using insertion, preserve history
6. **Summarize**: Confirm which files were updated and why

### Template: productContext.md

```markdown
# Product Context

High-level overview of the project and expected product.

[YYYY-MM-DD HH:MM:SS] - Initial entry

## Project Goal
*

## Key Features
*

## Overall Architecture
*

---
[YYYY-MM-DD HH:MM:SS] - Update notes appended as entries below
```

### Template: decisionLog.md

```markdown
# Decision Log

Architectural and implementation decisions with rationale.

[YYYY-MM-DD HH:MM:SS] - Decision name

## Decision
*

## Rationale
*

## Implementation Details
*

---
[YYYY-MM-DD HH:MM:SS] - Next decision
```

### Template: activeContext.md

```markdown
# Active Context

Current project status and focus.

[YYYY-MM-DD HH:MM:SS] - Initial entry

## Current Focus
*

## Recent Changes
*

## Open Questions/Issues
*

---
[YYYY-MM-DD HH:MM:SS] - Updated focus
```

### Template: progress.md

```markdown
# Progress

Task-list tracking of project work.

[YYYY-MM-DD HH:MM:SS] - Initial entry

## Completed Tasks
*

## Current Tasks
*

## Next Steps
*

---
[YYYY-MM-DD HH:MM:SS] - New tasks added
```

### Template: systemPatterns.md

```markdown
# System Patterns

Recurring patterns and standards used in the project (optional but recommended).

[YYYY-MM-DD HH:MM:SS] - Initial entry

## Coding Patterns
*

## Architectural Patterns
*

## Testing Patterns
*

---
[YYYY-MM-DD HH:MM:SS] - New patterns documented
```

---

## Procedure: Ask Mode

**Purpose**: Query memory bank in context before answering

**Invocation**: `/memory-bank ask <prompt>`

1. **Check Memory Bank**: Initialize if missing, read if exists
2. **Load Context**: Read all memory bank files
3. **Synthesize**: Provide user with:
   - Relevant entries from activeContext and productContext
   - Applicable patterns from systemPatterns
   - Related decisions from decisionLog
4. **Answer Prompt**: Address user's question using this loaded context
5. **Suggest Updates**: If noteworthy, inform user and suggest switching to architect mode to update memory

---

## Procedure: Code Mode

**Purpose**: Reference memory patterns before writing/modifying code

**Invocation**: `/memory-bank code <prompt>`

1. **Check Memory Bank**: Initialize if missing, read if exists
2. **Load Context**: Read all memory bank files, especially systemPatterns
3. **Extract Patterns**: Identify relevant:
   - Coding patterns
   - Architectural patterns
   - Related decisions
4. **Generate Code**: Write/modify code following identified patterns and conventions
5. **Update Progress**: If task is significant, suggest updating progress.md in architect mode

---

## Procedure: Debug Mode

**Purpose**: Use memory patterns and decisions to diagnose issues

**Invocation**: `/memory-bank debug <prompt>`

1. **Check Memory Bank**: Initialize if missing, read if exists
2. **Load Context**: Read all memory bank files
3. **Analyze**: Extract:
   - Related decisions from decisionLog
   - Applicable patterns from systemPatterns
   - Current focus from activeContext
4. **Diagnose**: Use this context to understand the issue
5. **Resolve**: Provide debugging guidance based on established patterns
6. **Update**: Suggest adding learnings to systemPatterns or decisionLog

---

## Special Command: Update Memory Bank (UMB)

**Purpose**: Synchronize memory bank with latest session activity

**Triggers**: `Update Memory Bank` or `UMB`

1. **Acknowledge**: Halt current work, respond `[MEMORY BANK: UPDATING]`
2. **Review Session**: Analyze complete chat history for:
   - Clarifications and context provided
   - Questions answered
   - Decisions made
   - Progress achieved
3. **Update Files**:
   - **activeContext.md** – Current focus, recent changes, open questions
   - **decisionLog.md** – Significant architectural decisions
   - **systemPatterns.md** – New patterns introduced
   - **progress.md** – Task completions and new tasks
4. **Synchronize**: Ensure all files reflect session activity
5. **Confirm**: Set status `[MEMORY BANK: ACTIVE]` and confirm synchronization complete

---

## Status Prefixes

Begin EVERY response with one of:
- `[MEMORY BANK: ACTIVE]` – Memory bank is initialized and loaded
- `[MEMORY BANK: INACTIVE]` – Memory bank was not found and user chose not to initialize
- `[MEMORY BANK: UPDATING]` – Memory bank is being synchronized with session activity

---

## Key Rules

1. **Never overwrite** existing memory bank entries – always append with timestamps
2. **Always timestamp** entries in format `YYYY-MM-DD HH:MM:SS`
3. **Check memory first** before every interaction (architect/ask/code/debug)
4. **Update throughout** the session when significant changes occur
5. **Read all files** sequentially when initializing memory bank context
6. **Preserve history** – all entries are logged and timestamped for audit trail

---

## Examples

### Initialize Memory Bank (No Existing Bank)

```
User: /memory-bank architect Set up project memory

Agent: No Memory Bank detected. I recommend creating one to maintain project context. Would you like me to initialize it?

User: Yes

Agent: [MEMORY BANK: ACTIVE]
I've initialized your Memory Bank with five core files:
- productContext.md (project overview)
- activeContext.md (current focus)
- progress.md (task tracking)
- decisionLog.md (decisions)
- systemPatterns.md (patterns)

Ready to capture project information. What would you like to document first?
```

### Query with Context (Bank Exists)

```
User: /memory-bank ask How should we structure the API?

Agent: [MEMORY BANK: ACTIVE]
Based on your productContext, this is a Next.js snooker scoring app...

From systemPatterns:
- Use Zustand for state management
- Implement snapshot-based undo system
- Derive scores, never cache

From decisionLog:
- Chose Tailwind v4 with custom theme tokens
- Implemented turn-order abstraction for multi-player support

Recommendation: Structure API following turn-based pattern established in your game mode architecture...
```

### Update Memory During Session

```
User: UMB

Agent: [MEMORY BANK: UPDATING]
Synchronizing memory bank with this session's activity...

Updated:
✓ activeContext.md – Documented focus on TypeScript type safety improvements
✓ progress.md – Marked "Add type validation" as completed
✓ decisionLog.md – Recorded decision to enforce stricter type checking
✓ systemPatterns.md – Added pattern for type-safe state updates

[MEMORY BANK: ACTIVE]
Memory Bank fully synchronized. All mode contexts preserved. Session ready for archive.
```

# SEAL Start Command

Initialize the SEAL self-improvement system in this project.

## Task

You are initializing the SEAL (Self-Adapting LLM) system for continuous improvement in this Claude Code workspace.

### Step 1: Create Directory Structure

Create the following directory structure:

```
.claude/seal/
├── config.json
├── task-types.json
├── learned-patterns.json
├── session-logs/
├── strategies/
└── metrics/
    └── success-rates.json
```

### Step 2: Initialize Configuration

Create `.claude/seal/config.json`:

```json
{
  "seal_version": "1.0.0",
  "learning_rate": 0.3,
  "success_threshold": 4,
  "min_samples_for_pattern": 3,
  "max_strategies_per_task": 3,
  "enable_auto_review": false,
  "enable_instrumentation": true,
  "task_classification": "auto",
  "evaluator": "user-rating",
  "created_at": "<current_timestamp>",
  "project_metadata": {
    "name": "<detect_from_package_json_or_dir>",
    "type": "<detect_language_and_framework>",
    "frameworks": []
  }
}
```

### Step 3: Analyze Project Context

Automatically detect and populate:

1. **Project type**: Check for:
   - `package.json` → Node.js/TypeScript project
   - `requirements.txt` or `pyproject.toml` → Python project
   - `Cargo.toml` → Rust project
   - `go.mod` → Go project
   - etc.

2. **Frameworks**: Look for:
   - React, Vue, Svelte (frontend)
   - Express, FastAPI, Django (backend)
   - pytest, jest (testing)

3. **Code patterns**: Sample 5-10 files to detect:
   - Naming conventions
   - File organization
   - Import styles
   - Comment patterns

### Step 4: Create Initial Task Types

Create `.claude/seal/task-types.json` with detected project types:

```json
{
  "feature-addition": {
    "description": "Adding new features to the codebase",
    "example_tasks": [],
    "success_patterns": [],
    "learned_prompts": [],
    "avg_success_rate": null,
    "total_attempts": 0
  },
  "bug-fix": {
    "description": "Fixing bugs and errors",
    "example_tasks": [],
    "success_patterns": [],
    "learned_prompts": [],
    "avg_success_rate": null,
    "total_attempts": 0
  },
  "refactoring": {
    "description": "Code refactoring and cleanup",
    "example_tasks": [],
    "success_patterns": [],
    "learned_prompts": [],
    "avg_success_rate": null,
    "total_attempts": 0
  },
  "testing": {
    "description": "Writing or updating tests",
    "example_tasks": [],
    "success_patterns": [],
    "learned_prompts": [],
    "avg_success_rate": null,
    "total_attempts": 0
  },
  "documentation": {
    "description": "Writing or updating documentation",
    "example_tasks": [],
    "success_patterns": [],
    "learned_prompts": [],
    "avg_success_rate": null,
    "total_attempts": 0
  }
}
```

### Step 5: Initialize Learned Patterns

Create `.claude/seal/learned-patterns.json`:

```json
{
  "version": "1.0.0",
  "last_updated": "<current_timestamp>",
  "patterns": [],
  "global_insights": [],
  "project_context": {
    "code_style": {},
    "common_imports": [],
    "file_structure": {},
    "testing_patterns": []
  }
}
```

### Step 6: Initialize Metrics

Create `.claude/seal/metrics/success-rates.json`:

```json
{
  "overall_success_rate": null,
  "by_task_type": {},
  "total_tasks": 0,
  "successful_tasks": 0,
  "average_iterations": null,
  "improvement_trend": [],
  "last_updated": "<current_timestamp>"
}
```

### Step 7: Bootstrap Initial Patterns (Optional)

If the project has:
- **README.md**: Extract coding guidelines
- **CONTRIBUTING.md**: Extract contribution patterns
- **Existing code comments**: Learn documentation style
- **Test files**: Learn testing patterns

Create initial patterns in `.claude/seal/learned-patterns.json`.

### Step 8: Create .gitignore Entry

Add to `.gitignore` (if not present):

```
# SEAL session logs (may contain sensitive info)
.claude/seal/session-logs/
```

Keep other SEAL files tracked (config, learned patterns) so they persist.

### Step 9: Summary Report

After initialization, provide a summary:

```markdown
## SEAL Initialization Complete ✓

**Project Detected**: <project_name> (<project_type>)
**Frameworks**: <detected_frameworks>
**Task Types Configured**: 5 default types
**Initial Patterns**: <number> patterns bootstrapped

### Next Steps

1. **Start working**: Just use Claude Code normally
2. **Rate your tasks**: After completion, use `/seal-review` to log success
3. **See improvements**: After 3+ tasks, SEAL will start generating patterns

### Quick Commands

- `/seal-review` - Review and rate the last task
- `/seal-metrics` - View learning progress
- `/seal-improve <task-type>` - Manually improve a task type
- `/seal-status` - Check SEAL configuration

**SEAL is now watching and learning from your coding sessions!**
```

## Implementation Notes

- Use the `Write` tool to create files
- Use `Read` tool to detect project structure
- Use `Grep` to find patterns in existing code
- Be thorough but fast - this is a one-time setup

## Error Handling

- If `.claude/seal/` already exists, ask user if they want to:
  - Keep existing (merge new config)
  - Overwrite (fresh start)
  - Cancel
- If project type can't be detected, default to "general" and ask user to specify

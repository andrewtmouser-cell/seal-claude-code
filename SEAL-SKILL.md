# SEAL Skill for Claude Code

This is a self-improving coding skill that implements the SEAL (Self-Adapting LLMs) technique adapted for Claude Code's API-based architecture.

## What This Skill Does

The SEAL skill enables Claude Code to:
1. **Learn from your coding sessions** - Automatically identifies successful patterns
2. **Generate self-improvements** - Creates better prompt templates over time
3. **Adapt to your projects** - Builds project-specific knowledge bases
4. **Iterate towards excellence** - Uses ReST-EM-inspired loops to refine approaches

## Installation

### Option 1: As a Claude Code Skill (Recommended)

```bash
# Create skills directory if it doesn't exist
mkdir -p .claude/skills

# Copy the skill file
cp SEAL-SKILL.md .claude/skills/seal.md
```

### Option 2: As a Slash Command

```bash
# Create commands directory
mkdir -p .claude/commands

# Copy command files
cp commands/seal-start.md .claude/commands/
cp commands/seal-review.md .claude/commands/
cp commands/seal-improve.md .claude/commands/
```

## How It Works

### Architecture

```
User Task → Strategy Generation → Execution → Evaluation → Learning
     ↑                                                          ↓
     └──────────────── Improved Prompts ←──────────────────────┘
```

### SEAL Loop Phases

#### Phase 1: Strategy Generation (Self-Edit)
When you give Claude Code a task, it first:
1. Checks `.claude/seal/learned-patterns.json` for similar tasks
2. Generates multiple solution approaches
3. Selects the most promising based on past success

#### Phase 2: Execution
Applies the selected strategy with instrumented logging:
- Records decisions made
- Tracks which context was helpful
- Notes where it struggled

#### Phase 3: Evaluation
After task completion:
- Asks you to rate success (1-5)
- Analyzes what worked vs. didn't
- Compares to historical performance

#### Phase 4: Learning (ReST-EM)
Updates knowledge base:
- Filters successful strategies (rating ≥ 4)
- Synthesizes meta-patterns from multiple successes
- Updates prompt templates
- Prunes unsuccessful approaches

## Usage

### Quick Start

```bash
# Initialize SEAL in your project
/seal-start

# Work on a task (automatically tracked)
"Add user authentication to the app"

# After task completion, trigger learning
/seal-review

# Manually improve a specific pattern
/seal-improve "bug fixing"
```

### Advanced Usage

#### Custom Task Types

Add your own task categories to `.claude/seal/task-types.json`:

```json
{
  "custom-api-integration": {
    "description": "Integrating third-party APIs",
    "example_tasks": [
      "Add Stripe payment processing",
      "Integrate SendGrid for emails"
    ],
    "success_patterns": [],
    "learned_prompts": []
  }
}
```

#### Fine-Tune Learning

Adjust SEAL parameters in `.claude/seal/config.json`:

```json
{
  "learning_rate": 0.3,
  "success_threshold": 4,
  "min_samples_for_pattern": 3,
  "max_strategies_per_task": 5,
  "enable_auto_review": true
}
```

## Project-Specific Adaptations

### Web Development Projects

SEAL learns patterns for:
- Component creation (React/Vue/Svelte)
- API endpoint design
- Database migrations
- State management

Example learned pattern:
```markdown
When creating a new React component:
1. Check existing component patterns in /src/components
2. Match styling approach (CSS modules, styled-components, etc.)
3. Follow naming convention (PascalCase, feature-based folders)
4. Include TypeScript types from existing patterns
5. Add to component index if present
```

### Data Science Projects

SEAL learns patterns for:
- Data preprocessing pipelines
- Model evaluation strategies
- Visualization styles
- Experiment tracking

### CLI Tool Development

SEAL learns patterns for:
- Argument parsing conventions
- Error handling styles
- Output formatting preferences
- Testing approaches

## Files Created by SEAL

```
.claude/seal/
├── config.json                 # SEAL configuration
├── task-types.json             # Task category definitions
├── learned-patterns.json       # Successful patterns database
├── session-logs/              # Historical session data
│   └── 2025-01-19-session-1.json
├── strategies/                # Generated strategies
│   └── component-creation-v3.md
└── metrics/                   # Performance tracking
    └── success-rates.json
```

## Integration with Existing Workflows

### With Git Hooks

SEAL can trigger learning on git commits:

```bash
# .git/hooks/post-commit
#!/bin/bash
echo "Running SEAL review..."
claude-code /seal-review --auto
```

### With CI/CD

Incorporate SEAL metrics in CI:

```yaml
# .github/workflows/seal-metrics.yml
name: SEAL Quality Check
on: [pull_request]
jobs:
  seal-check:
    runs-on: ubuntu-latest
    steps:
      - name: Check SEAL success rate
        run: |
          SUCCESS_RATE=$(cat .claude/seal/metrics/success-rates.json | jq '.overall')
          if (( $(echo "$SUCCESS_RATE < 0.7" | bc -l) )); then
            echo "Warning: SEAL success rate below 70%"
          fi
```

## Performance Metrics

SEAL tracks:
- **First-try success rate**: Tasks completed without iteration
- **Average iterations**: How many refinements needed
- **Pattern coverage**: % of tasks matching learned patterns
- **Improvement velocity**: Success rate trend over time

View metrics:
```bash
/seal-metrics
```

## Troubleshooting

### "No learned patterns found"
- You need at least 3 successful completions before SEAL generates patterns
- Try `/seal-bootstrap` to create initial patterns from documentation

### "Strategies not improving"
- Check if success_threshold is too high (lower from 4 to 3)
- Increase learning_rate in config.json
- Use `/seal-debug` to see what's being learned

### "Too many strategy options"
- SEAL generates multiple strategies by default
- Set `max_strategies_per_task: 1` for deterministic behavior
- Use `preferred_strategy` in task-types.json to favor certain approaches

## Examples

### Example 1: Learning Bug Fix Patterns

**Session 1:**
```
User: "Fix the null pointer exception in UserService"
Claude: [Applies default debugging approach]
User: *rates 5/5*
```

SEAL learns:
- Check for null guards in similar services
- Look at test files for expected behavior
- Review recent commits for context

**Session 5:**
```
User: "Fix the null pointer exception in PaymentService"
Claude: [Automatically checks test files, reviews recent commits]
User: *rates 5/5* "Perfect, you knew exactly what to look for!"
```

### Example 2: Project Onboarding

**New project:**
```bash
/seal-start --analyze-repo

# SEAL generates initial patterns:
# - Discovers Python project with pytest
# - Notes FastAPI framework usage
# - Identifies SQLAlchemy ORM patterns
# - Learns API structure conventions
```

**First task:**
```
User: "Add a new endpoint for user profile"

# SEAL uses discovered patterns:
# - Follows existing router structure
# - Matches SQLAlchemy model patterns
# - Includes pytest tests matching style
# - Uses same error handling approach
```

## Advanced: Custom Evaluators

Replace simple ratings with custom evaluation:

```python
# .claude/seal/evaluators/code-quality.py
def evaluate_task(task_result):
    """Custom evaluator using automated checks"""
    score = 0

    # Check tests pass
    if run_tests():
        score += 2

    # Check linting
    if run_linter() == 0:
        score += 1

    # Check coverage
    if get_coverage() > 0.8:
        score += 1

    # Check type safety
    if run_type_checker():
        score += 1

    return score  # 0-5
```

Enable in config:
```json
{
  "evaluator": "code-quality.py",
  "enable_auto_review": true
}
```

## Comparison to Original SEAL

| Feature | Original SEAL | SEAL for Claude Code |
|---------|--------------|---------------------|
| Learning mechanism | LoRA fine-tuning | Prompt pattern distillation |
| Speed | ~30 sec/adaptation | Instant (no training) |
| Storage | Model weights (~100MB) | JSON patterns (~1MB) |
| GPU required | Yes (A100/H100) | No |
| Persistence | Adapters on disk | Prompt library |
| Cost | GPU compute | API calls |
| Iteration speed | Moderate (training) | Fast (API) |

## Roadmap

- [ ] Multi-project learning (share patterns across repos)
- [ ] Team collaboration (shared pattern library)
- [ ] A/B testing (compare strategies before committing)
- [ ] Automatic pattern discovery (unsupervised)
- [ ] Integration with Claude.ai projects
- [ ] VSCode extension with GUI metrics

## Contributing

Improve SEAL's learning:
1. Share successful patterns in `.claude/seal/learned-patterns.json`
2. Report which task types work best
3. Suggest new evaluation metrics
4. Create domain-specific adaptations

## License

This skill is based on the MIT SEAL research project. Adapted for Claude Code by the community.

---

**Ready to start self-improving?**

```bash
/seal-start
```

Your Claude Code instance will get better with every task.

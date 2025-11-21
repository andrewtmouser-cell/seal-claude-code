# SEAL Review Command

Review and learn from the completed coding task.

## Task

You are performing a SEAL learning cycle on the most recent coding task.

### Step 1: Identify Last Task

Look at the conversation history to identify:
- What was the user's request?
- What approach did you take?
- What files were modified?
- What tools were used?
- Were there any iterations/corrections?

Extract:
```json
{
  "task_description": "<user's original request>",
  "task_type": "<classify: feature-addition|bug-fix|refactoring|testing|documentation|other>",
  "approach_taken": "<summary of your strategy>",
  "files_modified": ["<list of file paths>"],
  "tools_used": ["Read", "Write", "Grep", "Bash", ...],
  "iterations_needed": <number>,
  "challenges_encountered": ["<list of difficulties>"],
  "context_that_helped": ["<what context was useful>"]
}
```

### Step 2: Ask for User Rating

Prompt the user:

```markdown
## Task Review

I completed: **<task_description>**

Please rate this completion (1-5):
- **5**: Perfect, exactly what you wanted
- **4**: Good, minor tweaks needed
- **3**: Acceptable, but significant issues
- **2**: Poor, didn't meet requirements
- **1**: Failed, had to redo completely

**Rating**:
```

Wait for user input.

### Step 3: Detailed Evaluation

If rating ≥ 4 (successful), ask:

```markdown
### Success Analysis

What specifically worked well?
1. [ ] I understood the requirements clearly
2. [ ] I used appropriate context/files
3. [ ] The solution was correct first try
4. [ ] Code style matched your preferences
5. [ ] I anticipated edge cases

Any specific patterns I should remember?
```

If rating ≤ 3 (unsuccessful), ask:

```markdown
### Failure Analysis

What went wrong?
1. [ ] Misunderstood requirements
2. [ ] Missed important context
3. [ ] Wrong approach/architecture
4. [ ] Style/convention mismatch
5. [ ] Overlooked edge cases

What should I have done instead?
```

### Step 4: Update Session Log

Create `.claude/seal/session-logs/<timestamp>-session.json`:

```json
{
  "timestamp": "<ISO 8601 timestamp>",
  "task_id": "<generate_unique_id>",
  "task_description": "<from step 1>",
  "task_type": "<from step 1>",
  "user_rating": <rating>,
  "success": <rating >= 4>,
  "iterations": <from step 1>,
  "approach": {
    "strategy_description": "<from step 1>",
    "files_modified": [],
    "tools_used": [],
    "context_used": []
  },
  "outcome": {
    "what_worked": ["<from user feedback>"],
    "what_failed": ["<from user feedback>"],
    "user_notes": "<additional user comments>"
  },
  "metadata": {
    "conversation_id": "<if available>",
    "model_version": "claude-sonnet-4.5",
    "token_usage": "<if available>"
  }
}
```

### Step 5: Extract Patterns (if successful)

If this is a successful task (rating ≥ 4):

1. **Check pattern frequency**: Load all session logs for this task type
2. **If ≥ min_samples_for_pattern**: Look for common patterns

Analyze across successful sessions:
- Common file patterns accessed
- Common tool sequences
- Common context sources
- Common solution structures

### Step 6: Update Learned Patterns

If patterns are detected, update `.claude/seal/learned-patterns.json`:

```json
{
  "version": "1.0.0",
  "last_updated": "<current_timestamp>",
  "patterns": [
    {
      "pattern_id": "<generate_id>",
      "name": "<descriptive_name>",
      "task_type": "<task_type>",
      "confidence": <0.0-1.0>,
      "success_rate": <calculated_from_sessions>,
      "sample_size": <number_of_sessions_contributing>,
      "pattern": {
        "description": "<what this pattern does>",
        "trigger": "<when to apply this pattern>",
        "steps": [
          "<step 1>",
          "<step 2>",
          "..."
        ],
        "context_to_gather": [
          "<what files/context to read>",
          "..."
        ],
        "common_tools": ["<tools_typically_used>"],
        "pitfalls_to_avoid": ["<common_mistakes>"]
      },
      "examples": [
        {
          "task_description": "<example_task>",
          "session_id": "<session_id>",
          "rating": <rating>
        }
      ],
      "last_updated": "<timestamp>"
    }
  ],
  "global_insights": [
    "<insight 1: e.g., 'Always check test files when debugging'>",
    "<insight 2: e.g., 'User prefers functional components over class components'>"
  ]
}
```

### Step 7: Update Metrics

Update `.claude/seal/metrics/success-rates.json`:

```json
{
  "overall_success_rate": <(successful_tasks / total_tasks)>,
  "by_task_type": {
    "<task_type>": {
      "success_rate": <calculated>,
      "total": <count>,
      "average_iterations": <calculated>,
      "average_rating": <calculated>
    }
  },
  "total_tasks": <increment>,
  "successful_tasks": <increment_if_rating>=4>,
  "average_iterations": <recalculate>,
  "improvement_trend": [
    {
      "date": "<date>",
      "success_rate": <rate_for_last_10_tasks>
    }
  ],
  "last_updated": "<current_timestamp>"
}
```

### Step 8: ReST-EM Distillation (if applicable)

If there are ≥ 5 successful sessions for a task type:

1. **Load all successful strategies** for that task type
2. **Use Claude to synthesize a meta-strategy**:

Prompt (to yourself):
```markdown
I have these successful approaches for <task_type>:

<list_of_approaches>

What are the common effective patterns? Create a unified strategy that captures the best practices.

Format as:
- **Trigger**: When to use this strategy
- **Steps**: Ordered list of steps
- **Context needed**: What to read/gather first
- **Common pitfalls**: What to avoid
```

3. **Save synthesized strategy** to `.claude/seal/strategies/<task_type>-v<version>.md`

4. **Update task type** in `task-types.json` with the new learned prompt

### Step 9: Report to User

Provide summary:

```markdown
## SEAL Learning Summary

**Task**: <task_description>
**Rating**: <rating>/5
**Status**: <Success ✓ | Needs Improvement ⚠>

### Learning Applied

<if patterns updated>
- ✓ Updated pattern: "<pattern_name>"
- ✓ Confidence increased: <old> → <new>
- ✓ Added to <task_type> knowledge base

<if new pattern created>
- ⭐ New pattern discovered: "<pattern_name>"
- Based on <sample_size> successful sessions
- Will apply to future <task_type> tasks

<if no patterns yet>
- Logged session for future learning
- Need <remaining> more successful sessions to generate pattern

### Current Stats

- Overall success rate: <overall_success_rate>%
- <task_type> success rate: <task_specific_rate>%
- Total tasks completed: <total_tasks>
- Improvement trend: <trending_up|trending_down|stable>

### Next Session

<if pattern exists for this task type>
When you have a similar <task_type> task, I'll automatically apply the learned pattern: "<pattern_name>"

<else>
Continue working normally. I'm learning your preferences with each task!
```

## Implementation Notes

### Pattern Detection Algorithm

```python
def detect_pattern(sessions, task_type, min_samples=3):
    successful_sessions = [s for s in sessions
                          if s['task_type'] == task_type
                          and s['success']]

    if len(successful_sessions) < min_samples:
        return None

    # Find common elements
    common_files = find_common_files(successful_sessions)
    common_tools = find_common_tool_sequences(successful_sessions)
    common_approaches = find_common_approach_patterns(successful_sessions)

    # Calculate confidence
    confidence = len(successful_sessions) / len([s for s in sessions if s['task_type'] == task_type])

    return {
        'pattern': {
            'common_files': common_files,
            'common_tools': common_tools,
            'approach': common_approaches
        },
        'confidence': confidence,
        'sample_size': len(successful_sessions)
    }
```

### Success Rate Calculation

```python
def calculate_success_rate(sessions, window_size=10):
    recent = sessions[-window_size:]
    return sum(1 for s in recent if s['success']) / len(recent)
```

## Error Handling

- If no SEAL directory exists, prompt: "Run `/seal-start` first to initialize SEAL"
- If no recent task found in conversation, ask user to describe the task manually
- If user doesn't provide rating, use a default of 3 (neutral)

# SEAL Apply Command

Automatically apply learned patterns to a new task.

## Task

You are applying SEAL's learned patterns to solve the user's task more effectively.

This command is **automatically invoked** when SEAL detects you're starting a new task and learned patterns exist.

### Step 1: Classify the Task

Analyze the user's request and classify it:

```json
{
  "task_description": "<user's request>",
  "task_type": "<feature-addition|bug-fix|refactoring|testing|documentation|other>",
  "keywords": ["<extracted_keywords>"],
  "complexity": "<low|medium|high>",
  "similar_past_tasks": ["<session_ids_of_similar_tasks>"]
}
```

### Step 2: Load Relevant Patterns

Read `.claude/seal/learned-patterns.json` and filter for:
- Patterns matching the task_type
- Patterns with confidence ≥ 0.6
- Patterns with success_rate ≥ 0.75

Sort by: `confidence * success_rate` (descending)

### Step 3: Load Strategy (if exists)

Check for `.claude/seal/strategies/<task_type>-v*.md`

Load the latest version.

### Step 4: Generate Approach

Use the learned pattern/strategy to generate your approach:

**Template:**

```markdown
## Applying SEAL Pattern: "<pattern_name>"

Based on <sample_size> successful past sessions (success rate: <success_rate>%)

### Recommended Approach

<pattern.steps>

### Context I'll Gather

<pattern.context_to_gather>

### Common Pitfalls to Avoid

<pattern.pitfalls_to_avoid>

### Proceeding with task...
```

### Step 5: Gather Context

Based on pattern's `context_to_gather`, proactively read files:

```python
# Example pattern might specify:
context_to_gather = [
    "Read test files related to the feature",
    "Check existing similar components",
    "Review recent git commits for context"
]

# You execute:
for context_item in context_to_gather:
    execute_context_gathering(context_item)
```

**Common context gathering patterns:**

- **For bug-fix**:
  - Read the file with the bug
  - Search for related test files
  - Check git log for recent changes to that file
  - Look for similar bug fixes in history

- **For feature-addition**:
  - Find similar existing features
  - Check project structure/conventions
  - Read relevant tests
  - Review API/interface patterns

- **For refactoring**:
  - Understand current implementation fully
  - Check all usages of the code
  - Verify test coverage exists
  - Review code style guidelines

### Step 6: Execute with Instrumentation

As you work, log your decisions:

```json
{
  "pattern_applied": "<pattern_id>",
  "context_gathered": [
    {"item": "<context_item>", "helpful": true|false, "reason": "<why>"}
  ],
  "steps_followed": [
    {"step": "<step_description>", "successful": true|false, "notes": "<any_notes>"}
  ],
  "deviations_from_pattern": [
    {"what": "<what_you_did_differently>", "why": "<reason>"}
  ]
}
```

This will be used in the next SEAL review to refine the pattern.

### Step 7: Self-Edit (If Needed)

If you encounter challenges:

1. **Check if pattern has guidance** for this challenge
2. **If not**: Generate a strategy adjustment on-the-fly

Use self-edit prompt (adapted from original SEAL):

```markdown
I'm working on: <task>
Using pattern: <pattern_name>

I've encountered: <challenge>

What adjustment should I make?
Options:
1. Gather additional context (specify what)
2. Try alternative approach (specify what)
3. Seek user clarification
4. Continue with current approach

Decision: <your_decision>
Reasoning: <why>
```

Log this decision for future learning.

### Step 8: Track Progress

Update in-memory tracking (don't write to disk yet - that's for seal-review):

```json
{
  "task_id": "<generate>",
  "pattern_used": "<pattern_id>",
  "started_at": "<timestamp>",
  "context_gathered": [...],
  "steps_completed": [...],
  "current_status": "in_progress"
}
```

### Step 9: Complete Task

Execute the task following the pattern's steps.

If successful:
```markdown
## Task Complete ✓

Applied pattern: <pattern_name>
Followed steps: <steps>

<if any deviations>
**Note**: I deviated from the pattern by <deviation> because <reason>
This will be logged for future learning.

When you're ready, use `/seal-review` to rate this completion and help me improve!
```

## Special Modes

### Exploration Mode

If no pattern exists for task type (confidence < 0.5):

```markdown
## SEAL Exploration Mode

This is a <task_type> task, but I don't have a strong learned pattern yet.

I'll use a general approach and pay extra attention to:
- What context proves useful
- What steps work well
- What pitfalls to avoid

This will help me build a better pattern for future tasks!

Please use `/seal-review` after completion - your feedback is extra valuable for learning.
```

### Multi-Pattern Mode

If multiple patterns match (similar confidence):

Generate multiple strategies and use the self-edit mechanism:

```markdown
## Multiple Strategies Available

I've identified <N> possible approaches for this <task_type> task:

**Strategy 1**: <pattern_1_name>
- <brief_description>
- Success rate: <rate>%
- Best for: <scenario>

**Strategy 2**: <pattern_2_name>
- <brief_description>
- Success rate: <rate>%
- Best for: <scenario>

Based on your task specifics, I recommend: **Strategy <X>**

Reasoning: <why_this_one>

Proceed with this strategy? (or specify another)
```

### Confidence Threshold Check

Before applying a pattern, check confidence:

```python
if pattern.confidence < 0.6:
    print("⚠️ Low confidence pattern - proceeding with caution")
    print("Sample size: <N> (may need more data)")

if pattern.success_rate < 0.75:
    print("⚠️ Pattern has mixed results - will monitor closely")
```

## Integration with Conversation Flow

SEAL-apply should be **invisible when working well**:

**Good integration:**
```
User: "Add a login component to the React app"
Claude: [silently loads React component pattern]
Claude: [gathers context: checks existing components, finds auth patterns]
Claude: [creates component following learned patterns]
Claude: "I've created the login component following the project's patterns..."
```

**Only surface SEAL when:**
- User explicitly asks (`/seal-apply`)
- Pattern confidence is low (inform user)
- Multiple viable strategies exist (ask user)
- Pattern fails to apply (explain what happened)

## Auto-Apply Configuration

In `.claude/seal/config.json`:

```json
{
  "auto_apply_patterns": true,
  "min_confidence_for_auto_apply": 0.7,
  "show_pattern_info": false,
  "always_explain_strategy": false
}
```

If `auto_apply_patterns: true`:
- Automatically load and use patterns
- Don't announce it unless it's relevant

If `show_pattern_info: true`:
- Always show which pattern you're using
- Useful for debugging SEAL

If `always_explain_strategy: true`:
- Always explain your approach before executing
- Slower but more transparent

## Error Handling

**Pattern Not Found:**
- Fall back to default approach
- Log as exploration mode session
- Notify user only if asked

**Pattern Fails to Apply:**
- Log the failure with reason
- Switch to exploration mode
- Continue with task
- Mark pattern for review

**Context Gathering Fails:**
- Skip unavailable context
- Log what was missing
- Proceed with available context
- Note in session log for pattern refinement

## Implementation Notes

### Pattern Application Logic

```python
def apply_pattern(task, pattern):
    """Apply learned pattern to new task"""

    # 1. Validate pattern applicability
    if not is_applicable(task, pattern):
        return None

    # 2. Gather context
    context = {}
    for item in pattern['context_to_gather']:
        try:
            context[item] = gather_context(item, task)
        except:
            log_missing_context(item)

    # 3. Execute steps
    results = []
    for step in pattern['steps']:
        result = execute_step(step, context)
        results.append(result)

        # Check if step succeeded
        if not result.success:
            # Try pattern's recovery strategy
            if 'recovery' in pattern:
                execute_recovery(pattern['recovery'])
            else:
                # Fall back to exploration mode
                return exploration_mode(task, context)

    return results
```

### Strategy Selection

```python
def select_best_strategy(task, patterns):
    """Select best pattern for task"""

    # Score each pattern
    scores = []
    for pattern in patterns:
        score = (
            pattern['confidence'] * 0.4 +
            pattern['success_rate'] * 0.4 +
            similarity(task, pattern['examples']) * 0.2
        )
        scores.append((pattern, score))

    # Return highest scoring
    return max(scores, key=lambda x: x[1])[0]
```

### Context Similarity

```python
def similarity(task, examples):
    """Calculate task similarity to pattern examples"""

    # Simple keyword overlap for now
    task_keywords = extract_keywords(task['description'])

    example_keywords = []
    for ex in examples:
        example_keywords.extend(extract_keywords(ex['task_description']))

    overlap = len(set(task_keywords) & set(example_keywords))
    return overlap / max(len(task_keywords), 1)
```

## Next Steps After Application

After applying pattern:
1. **Monitor execution**: Track which steps work
2. **Log deviations**: Note where pattern didn't fit
3. **Wait for review**: User rates with `/seal-review`
4. **Update pattern**: If successful, increase confidence. If failed, adjust pattern.

This creates the SEAL improvement loop:
```
Apply Pattern → Execute Task → Review → Learn → Apply Better Pattern
```

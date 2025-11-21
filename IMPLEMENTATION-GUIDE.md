# SEAL for Claude Code: Complete Implementation Guide

Transform your Claude Code instance into a self-improving coding assistant using SEAL principles.

## 🎯 What You'll Build

Three tiers of SEAL integration:

1. **Tier 1: Manual SEAL** - You track patterns, Claude Code applies them (15 min setup)
2. **Tier 2: Semi-Autonomous** - Claude Code learns patterns, you approve decisions (30 min setup)
3. **Tier 3: Autonomous Agent** - Full SEAL loop with minimal oversight (2 hour setup)

## Prerequisites

- Claude Code VSCode extension installed
- Active Claude Pro/API account
- Basic familiarity with slash commands

## 📦 Installation

### Option 1: Quick Install (Recommended)

```bash
# Navigate to your project
cd your-project/

# Download and run installer
curl -O https://raw.githubusercontent.com/your-repo/seal-claude/main/install.sh
bash install.sh

# Choose your tier
? Select SEAL tier:
  ❯ Tier 1: Manual (15 min)
    Tier 2: Semi-Autonomous (30 min)
    Tier 3: Autonomous (2 hr)
```

### Option 2: Manual Installation

1. **Create SEAL directory structure**:

```bash
mkdir -p .claude/seal/{session-logs,strategies,metrics}
mkdir -p .claude/commands
```

2. **Copy command files**:

```bash
# From this repo
cp claude-code-integration/commands/*.md .claude/commands/
```

3. **Choose a template** (optional but recommended):

```bash
# For web development
cp claude-code-integration/templates/WEB-DEVELOPMENT-SEAL.md .claude/seal/

# For data science
cp claude-code-integration/templates/DATA-SCIENCE-SEAL.md .claude/seal/

# For general projects
cp claude-code-integration/templates/GENERAL-SEAL.md .claude/seal/
```

4. **Initialize SEAL**:

```bash
# In Claude Code
/seal-start
```

## 🏗️ Implementation by Tier

### Tier 1: Manual SEAL (15 minutes)

**What it does**: You maintain a pattern library, Claude Code references it.

**Setup**:

1. **Initialize**:
```bash
/seal-start --tier manual
```

2. **Create your first pattern**:

Create `.claude/seal/my-patterns.md`:

```markdown
## Pattern: Add React Component

When I ask you to create a React component:
1. Check src/components/ for similar components
2. Use functional components with TypeScript
3. Import styles from component.module.css
4. Add PropTypes or TS interface
5. Create test file if other components have tests

## Pattern: Fix Bug

When I report a bug:
1. Read the file with the error
2. Check git log for recent changes
3. Look for related test files
4. Propose a fix with explanation
5. Update tests if needed

## Pattern: API Integration

When I need to call an API:
1. Check src/api/ for existing API client
2. Match the axios/fetch pattern used
3. Add TypeScript types for request/response
4. Handle errors with toast notifications
5. Create custom hook if pattern exists
```

3. **Use patterns**:

```bash
# Tell Claude Code to use your patterns
"Create a UserProfile component. Follow the patterns in .claude/seal/my-patterns.md"
```

4. **Update patterns** as you learn what works.

**Pros**:
- Simple, no automation needed
- Full control
- Works immediately

**Cons**:
- Manual pattern maintenance
- No automatic learning
- You must remember to reference patterns

**Time to value**: Immediate

---

### Tier 2: Semi-Autonomous (30 minutes)

**What it does**: Claude Code learns patterns automatically, asks for approval before applying.

**Setup**:

1. **Initialize with semi-autonomous mode**:

```bash
/seal-start --tier semi-autonomous
```

This creates:
- `.claude/seal/config.json` (with checkpoints enabled)
- `.claude/seal/task-types.json` (task categories)
- `.claude/seal/learned-patterns.json` (empty initially)
- Commands: `/seal-review`, `/seal-apply`, `/seal-metrics`

2. **Configure oversight level**:

Edit `.claude/seal/config.json`:

```json
{
  "tier": "semi-autonomous",
  "checkpoint_frequency": "medium",
  "auto_approve_confidence_threshold": 0.85,
  "require_approval_for": [
    "package_installs",
    "schema_changes",
    "breaking_changes"
  ],
  "enable_auto_review": false
}
```

3. **Complete your first learning cycle**:

```bash
# 1. Work on a task
"Add user authentication with JWT"

# 2. Claude Code will ask for approval at checkpoints
[Claude]: "About to install jsonwebtoken package. Approve? (yes/no)"
You: "yes"

# 3. After completion, review
/seal-review

# 4. Rate the task
Rating: 5
What worked: You checked existing middleware patterns
What to remember: Always match existing error handling
```

4. **Repeat for 3-5 tasks** of each type (feature, bug-fix, etc.)

5. **See learned patterns**:

```bash
/seal-metrics --patterns
```

6. **Use patterns automatically**:

After 3+ successful tasks, Claude Code will:
- Automatically detect task type
- Apply learned pattern (with your approval)
- Learn from each iteration

**Workflow**:

```
You: "Add a login component"
↓
Claude: Detects task type = "component-creation"
        Loads pattern with confidence 0.87
        "I'll follow the React component pattern. Proceed? (yes/details)"
↓
You: "yes"
↓
Claude: Executes with checkpoints for package installs, etc.
↓
Claude: "Complete! Use /seal-review to rate"
↓
You: /seal-review → rating 5
↓
Claude: Updates pattern confidence 0.87 → 0.89
```

**Pros**:
- Automatic pattern learning
- You stay in control via checkpoints
- Improves over time

**Cons**:
- Requires 3-5 tasks to generate patterns
- Checkpoints can slow you down initially
- Needs manual reviews

**Time to value**: After 5-10 tasks (~2-3 days)

---

### Tier 3: Autonomous Agent (2 hours)

**What it does**: Full SEAL implementation - learns, applies, and improves autonomously.

**Setup**:

1. **Prerequisites**:

```bash
# Install dependencies for advanced features
npm install -g @anthropics/claude-code-agent-sdk

# Or for Python projects
pip install anthropic-agent-sdk
```

2. **Initialize autonomous tier**:

```bash
/seal-start --tier autonomous
```

3. **Set up automated evaluation**:

Create `.claude/seal/evaluators/auto-eval.py`:

```python
"""
Autonomous evaluation using project tests + metrics
"""
import subprocess
import json

def evaluate_task(task_id, files_modified):
    """Evaluate task quality automatically"""

    score = 0
    feedback = []

    # Run tests
    test_result = subprocess.run(['npm', 'test'], capture_output=True)
    if test_result.returncode == 0:
        score += 2
        feedback.append("All tests passing")
    else:
        feedback.append("Tests failing - needs fix")

    # Run linter
    lint_result = subprocess.run(['npm', 'run', 'lint'], capture_output=True)
    if lint_result.returncode == 0:
        score += 1
        feedback.append("Linting passed")

    # Check TypeScript
    if any(f.endswith('.ts') or f.endswith('.tsx') for f in files_modified):
        ts_result = subprocess.run(['npx', 'tsc', '--noEmit'], capture_output=True)
        if ts_result.returncode == 0:
            score += 1
            feedback.append("TypeScript check passed")

    # Check coverage
    coverage_result = subprocess.run(['npm', 'run', 'coverage'], capture_output=True)
    coverage = parse_coverage(coverage_result.stdout)
    if coverage > 80:
        score += 1
        feedback.append(f"Coverage: {coverage}%")

    return {
        'score': score,  # 0-5
        'feedback': feedback,
        'auto_approved': score >= 4
    }

def parse_coverage(output):
    # Parse coverage from output
    # Implementation depends on your coverage tool
    pass
```

4. **Enable auto-evaluation**:

`.claude/seal/config.json`:

```json
{
  "tier": "autonomous",
  "checkpoint_frequency": "low",
  "auto_approve_confidence_threshold": 0.85,
  "enable_auto_review": true,
  "evaluator": "evaluators/auto-eval.py",
  "auto_apply_patterns": true,
  "continuous_learning": true
}
```

5. **Set up background learning**:

Create `.claude/seal/agents/learning-agent.js`:

```javascript
/**
 * Background agent that runs ReST-EM learning
 */
const SEAL = require('./seal-core');

async function runLearningCycle() {
  // Load recent sessions
  const sessions = await SEAL.loadRecentSessions(20);

  // Filter successful
  const successful = sessions.filter(s => s.rating >= 4);

  // Group by task type
  const byType = SEAL.groupByTaskType(successful);

  // For each task type with enough samples
  for (const [taskType, tasks] of Object.entries(byType)) {
    if (tasks.length >= 3) {
      // Extract common patterns
      const patterns = await SEAL.extractPatterns(tasks);

      // Use Claude to synthesize meta-pattern
      const metaPattern = await SEAL.synthesizeMetaPattern(patterns);

      // Update learned patterns
      await SEAL.updatePattern(taskType, metaPattern);

      console.log(`Updated pattern for ${taskType}`);
    }
  }
}

// Run learning every 10 tasks
setInterval(runLearningCycle, 10 * 60 * 1000);  // Every 10 tasks (approx)
```

6. **Enable git hooks for auto-review**:

`.git/hooks/post-commit`:

```bash
#!/bin/bash

# Auto-review on commit
echo "🤖 SEAL: Auto-reviewing commit..."

# Get files changed
FILES=$(git diff-tree --no-commit-id --name-only -r HEAD)

# Call SEAL review
claude-code /seal-review --auto --files "$FILES"
```

7. **Start the autonomous loop**:

```bash
# Start SEAL agent in background
seal-agent start

# Or in Claude Code
/seal-agent start
```

**Workflow (Autonomous)**:

```
You: "Add payment processing with Stripe"
↓
Claude: [Automatically]
  - Detects task type: "api-integration"
  - Loads pattern (confidence 0.91)
  - Applies pattern autonomously
  - Runs auto-evaluation (tests, linting)
  - Scores 5/5 → auto-approves
  - Commits with descriptive message
  - Updates pattern confidence
  - No human intervention needed
↓
You: [Notification] "✓ Payment processing added. Tests passing. Commit: abc123"
```

You only intervene if:
- Auto-evaluation scores < 4
- Breaking change detected
- Security-sensitive code modified

**Pros**:
- Fully autonomous operation
- Continuous learning 24/7
- Maximum efficiency
- Scales to large projects

**Cons**:
- Complex setup
- Requires robust test suite
- Need good auto-evaluation
- Higher risk without oversight

**Time to value**: After 20-30 tasks (~1-2 weeks)

---

## 📊 Measuring Success

### Metrics to Track

**Tier 1 (Manual)**:
- Pattern library size
- Time saved vs. no patterns

**Tier 2 (Semi-Autonomous)**:
- Overall success rate (target: >80%)
- Tasks completed with patterns (target: >70%)
- Average iterations per task (target: <2)
- Pattern confidence growth

**Tier 3 (Autonomous)**:
- Auto-approved task rate (target: >85%)
- Continuous improvement velocity
- Learning cycles completed
- Error/rollback rate (target: <5%)

### View Metrics

```bash
# Basic metrics
/seal-metrics

# Detailed breakdown
/seal-metrics --detailed

# By task type
/seal-metrics --by-type

# Trends over time
/seal-metrics --trends
```

## 🔧 Customization

### Custom Task Types

Add domain-specific task types:

`.claude/seal/task-types.json`:

```json
{
  "database-migration": {
    "description": "Creating or modifying database schemas",
    "trigger_keywords": ["migration", "schema", "database", "alter table"],
    "learned_prompts": [],
    "success_patterns": []
  },
  "performance-optimization": {
    "description": "Improving code performance",
    "trigger_keywords": ["performance", "slow", "optimize", "speed up"],
    "learned_prompts": []
  }
}
```

### Custom Evaluators

Create evaluators for your domain:

`.claude/seal/evaluators/ui-eval.py`:

```python
def evaluate_ui_task(screenshots_before, screenshots_after):
    """Evaluate UI changes with visual regression"""

    # Compare screenshots
    diff_score = visual_diff(screenshots_before, screenshots_after)

    # Check accessibility
    a11y_score = run_axe_core(screenshots_after)

    # Check responsive
    responsive_score = check_breakpoints(screenshots_after)

    return {
        'score': (diff_score + a11y_score + responsive_score) / 3,
        'feedback': [...]
    }
```

### Team Collaboration

Share learned patterns across team:

```bash
# Export patterns
/seal-export > team-patterns.json

# Share via git
git add .claude/seal/learned-patterns.json
git commit -m "Update SEAL patterns"
git push

# Team members pull
git pull
/seal-import-patterns
```

## 🐛 Troubleshooting

### "SEAL isn't learning patterns"

**Check**:
1. Completed at least 3 tasks of same type?
2. Rated each task with `/seal-review`?
3. Tasks rated ≥ 4 (successful)?

**Fix**:
```bash
/seal-status
# Check min_samples_for_pattern in config
# Lower from 3 to 2 if needed
```

### "Patterns are wrong"

**Fix**:
```bash
# Edit pattern directly
code .claude/seal/learned-patterns.json

# Or reset and re-learn
/seal-reset-pattern "component-creation"
```

### "Too many checkpoints (Tier 2)"

**Fix**:
```bash
# Lower checkpoint frequency
/seal-config --checkpoints low

# Or increase auto-approve threshold
/seal-config --auto-approve 0.8
```

### "Auto-evaluation always fails (Tier 3)"

**Fix**:
```bash
# Check evaluator script
python .claude/seal/evaluators/auto-eval.py --test

# Adjust scoring thresholds
# Make evaluator less strict initially
```

## 📈 Optimization Tips

### 1. Start Conservative

- Begin with Tier 1 or 2
- Use high checkpoint frequency initially
- Build trust through verification

### 2. Focus on High-Value Patterns

Prioritize learning patterns for:
- Frequent tasks (components, API calls)
- Error-prone tasks (deployment, migrations)
- Time-consuming tasks (setup, configuration)

### 3. Iterate on Evaluation

For Tier 3:
- Start with simple evaluation (tests pass = good)
- Add metrics gradually (coverage, linting)
- Refine scoring weights based on what matters

### 4. Review Quarterly

Every 3 months:
- Review learned patterns
- Prune outdated patterns
- Share successful patterns with team
- Update task types

## 🎓 Learning Path

**Week 1**: Tier 1 (Manual)
- Create pattern library
- Reference patterns in prompts
- Iterate on what works

**Week 2-3**: Tier 2 (Semi-Autonomous)
- Initialize learning system
- Complete 15-20 rated tasks
- Watch patterns emerge

**Week 4-8**: Tier 2 refinement
- Tune checkpoint settings
- Improve pattern quality
- Reduce manual intervention

**Week 9+**: Tier 3 (Autonomous) - if desired
- Set up auto-evaluation
- Enable continuous learning
- Monitor and optimize

## 🚀 Quick Start Checklist

- [ ] Choose tier (1, 2, or 3)
- [ ] Install SEAL files
- [ ] Run `/seal-start`
- [ ] Select project template (web, data-science, etc.)
- [ ] Complete first task
- [ ] Run `/seal-review` and rate
- [ ] Repeat 5-10 times
- [ ] Check `/seal-metrics`
- [ ] Adjust configuration as needed
- [ ] Enjoy improved efficiency!

## 📚 Resources

- **Templates**: `claude-code-integration/templates/`
- **Examples**: `claude-code-integration/examples/`
- **Commands**: `.claude/commands/seal-*.md`
- **Configuration**: `.claude/seal/config.json`

## 🤝 Contributing

Improve SEAL for Claude Code:

1. Share successful patterns
2. Create domain-specific templates
3. Build custom evaluators
4. Report issues
5. Submit improvements

## 📄 License

MIT License - based on the original SEAL research from MIT CSAIL

---

**Ready to make your Claude Code instance self-improving?**

```bash
/seal-start
```

**Your coding assistant will get better with every task. 🚀**

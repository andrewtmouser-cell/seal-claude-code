# SEAL Semi-Autonomous Workflow

This workflow balances automation with human oversight - ideal when full autonomy isn't possible or desired.

## Overview

```
Human: Define Task → SEAL: Generate Strategies → Human: Approve → SEAL: Execute → Human: Review → SEAL: Learn
```

**Human checkpoints** (you're in control):
1. Task definition and approval
2. Strategy selection
3. Code review and rating
4. Pattern acceptance

**Autonomous parts** (SEAL handles):
1. Strategy generation
2. Code execution
3. Pattern detection
4. Metrics tracking

## Workflow Phases

### Phase 1: Task Definition (Human)

You provide the task as normal:
```
"Add user authentication with JWT tokens"
```

**SEAL's response** (if patterns exist):
```markdown
## Task Analysis

**Type**: feature-addition
**Complexity**: high
**Estimated iterations**: 2-3 (based on similar tasks)

I found 2 relevant learned patterns:
1. "JWT Authentication Pattern" (confidence: 0.85, 3 past successes)
2. "User Auth Pattern" (confidence: 0.72, 5 past successes)

Recommended: Pattern #1

**Checkpoint**: Should I proceed with this pattern, or would you like to see the approach first?
```

### Phase 2: Strategy Approval (Human Checkpoint)

**Option A: Auto-approve** (if you trust SEAL)
```
"Proceed"
```

**Option B: Review first**
```
"Show me the approach"
```

SEAL provides:
```markdown
## Proposed Approach (JWT Authentication Pattern)

Based on 3 successful implementations in this codebase.

**Steps**:
1. Install jsonwebtoken package
2. Create src/auth/jwt.ts with sign/verify functions
3. Add middleware to verify tokens on protected routes
4. Update User model with token generation method
5. Create login/logout endpoints
6. Add authentication to existing protected routes
7. Write tests for auth flow

**Context I'll gather**:
- Existing User model
- Current routing structure
- Existing middleware patterns
- Test file patterns

**Estimated time**: 15-20 minutes
**Confidence**: 85%

**Proceed?** (yes/no/modify)
```

You can:
- Approve: `"Yes, proceed"`
- Modify: `"Skip step 7, we'll test manually"`
- Reject: `"Use a different approach - try OAuth instead"`

### Phase 3: Execution with Check-ins (Semi-Autonomous)

SEAL executes but checks in at milestones:

```markdown
## Progress Update

✓ Step 1: Installed jsonwebtoken
✓ Step 2: Created src/auth/jwt.ts
✓ Step 3: Added middleware

🔄 Step 4: Updating User model

**Checkpoint**: I need to modify the User schema. I'll add:
- `generateAuthToken()` method
- `tokens` array field

This matches the pattern from UserProfile model. Proceed? (yes/no)
```

**Your options**:
- Quick approve: `"yes"`
- Modify: `"Only add the method, not the tokens field"`
- Take over: `"Let me do this part"`

**Configuration**:
You control checkpoint frequency in `.claude/seal/config.json`:
```json
{
  "checkpoint_frequency": "medium",  // low, medium, high
  "auto_approve_confidence_threshold": 0.9,  // auto-proceed if above this
  "require_approval_for": ["schema_changes", "package_installs", "breaking_changes"]
}
```

### Phase 4: Review & Rating (Human)

After completion, SEAL asks:

```markdown
## Task Complete ✓

**Completed**: Add user authentication with JWT tokens
**Pattern used**: JWT Authentication Pattern
**Time taken**: 18 minutes
**Files modified**: 7 files
**Tests**: All passing ✓

### What worked well
- Used existing middleware pattern successfully
- Tests matched established patterns
- Code style consistent with codebase

### Deviations from pattern
- Added refresh token support (not in original pattern)
  - Reason: Current best practice
  - Should this be added to the pattern?

**Please rate** (1-5):
```

**Your response**:
```
Rating: 5

Yes, add refresh token support to the pattern. Also, I liked how you matched the existing middleware style - make sure that's captured in the pattern.
```

### Phase 5: Learning (Autonomous)

SEAL automatically:
1. Updates pattern with refresh token step
2. Increases confidence (0.85 → 0.88)
3. Notes middleware style matching
4. Saves session log
5. Updates metrics

```markdown
## Learning Applied ✓

**Pattern updated**: JWT Authentication Pattern
- Added: Refresh token support (Step 5a)
- Noted: Match existing middleware style
- Confidence: 0.85 → 0.88
- Sample size: 3 → 4

**Next time** you add authentication, I'll:
- Include refresh tokens automatically
- Check middleware style first
- Expect ~18 minutes completion time

**Metrics**:
- Overall success rate: 87% → 88%
- Feature-addition success: 82% → 84%
- This pattern: 100% success rate (4/4)
```

## Human Checkpoint Types

### 1. Before Execution

**When**: SEAL has a plan, needs approval to start

**You decide**:
- ✓ Approve entire plan
- ✓ Modify specific steps
- ✓ Change approach entirely
- ✓ Do it yourself

**Trigger**: Always (default) or only when confidence < threshold

### 2. During Execution

**When**: SEAL encounters key decisions

**You decide**:
- ✓ Schema changes (add/modify database fields)
- ✓ Package installations (new dependencies)
- ✓ Breaking changes (modify public APIs)
- ✓ Security-sensitive changes (auth, permissions)
- ✓ Large refactors (>10 files)

**Trigger**: Configurable in `require_approval_for` list

### 3. Strategy Selection

**When**: Multiple strategies available

**You decide**:
- ✓ Which strategy to use
- ✓ Combine multiple strategies
- ✓ Try a new experimental approach

**Trigger**: When >1 strategy has similar confidence

### 4. After Execution

**When**: Task complete

**You decide**:
- ✓ Accept or reject changes
- ✓ Rate quality (1-5)
- ✓ Provide feedback for learning
- ✓ Approve pattern updates

**Trigger**: Always (this is critical for learning)

## Configuration Profiles

### Profile: Minimal Oversight

For well-established patterns you trust:

```json
{
  "checkpoint_frequency": "low",
  "auto_approve_confidence_threshold": 0.8,
  "require_approval_for": ["breaking_changes"],
  "auto_review": true,
  "auto_apply_patterns": true
}
```

**Result**: SEAL operates autonomously, only asking for:
- Breaking changes approval
- Post-task rating

### Profile: Moderate Oversight (Recommended)

Balanced control and automation:

```json
{
  "checkpoint_frequency": "medium",
  "auto_approve_confidence_threshold": 0.9,
  "require_approval_for": ["schema_changes", "package_installs", "breaking_changes"],
  "auto_review": false,
  "auto_apply_patterns": true
}
```

**Result**: SEAL auto-applies patterns but checks in for:
- Important changes (schema, packages, breaking)
- Strategy selection (if multiple options)
- Post-task manual review

### Profile: Maximum Oversight

Full control, SEAL as assistant:

```json
{
  "checkpoint_frequency": "high",
  "auto_approve_confidence_threshold": 1.0,
  "require_approval_for": ["all"],
  "auto_review": false,
  "auto_apply_patterns": false,
  "show_pattern_info": true,
  "always_explain_strategy": true
}
```

**Result**: SEAL shows you everything but executes nothing without approval

## Checkpoint Commands

### Approve Checkpoint
```
"yes" | "proceed" | "approved" | "✓"
```

### Approve with Modification
```
"yes, but skip step 3"
"proceed, but use axios instead of fetch"
"approved, add error handling"
```

### Reject Checkpoint
```
"no" | "stop" | "different approach"
"Let me handle this part"
```

### Skip Checkpoint (One Time)
```
"auto-approve for this task"
"don't ask again for this session"
```

### Change Checkpoint Settings
```
/seal-config --checkpoints low
/seal-config --auto-approve 0.85
/seal-config --require-approval "breaking_changes,security"
```

## Example: Semi-Autonomous Session

**Full walkthrough**:

```
[You]: Add pagination to the users list

[SEAL]:
## Task Analysis
Type: feature-addition
Pattern found: "List Pagination Pattern" (confidence: 0.91, 6 successes)

Approach:
1. Add pagination query params (page, limit)
2. Update API endpoint with skip/take
3. Update frontend with pagination controls
4. Match existing TablePagination component style

Proceed? (yes/no/details)

[You]: yes

[SEAL]:
🔄 Working on pagination...

✓ Updated API endpoint /api/users
✓ Added pagination params validation

🔄 Checkpoint: Installing package "react-paginate" for frontend pagination controls

This wasn't in the pattern, but matches other list components.
Approve package install? (yes/no)

[You]: no, we have a custom Pagination component already

[SEAL]:
✓ Using existing Pagination component from src/components/ui/
✓ Added pagination controls to UsersList
✓ Tested with mock data - working

## Complete ✓
- Backend: pagination implemented
- Frontend: using existing Pagination component
- Tests: added pagination tests

Pattern deviation: Used existing component instead of react-paginate
Should I update the pattern to check for existing components first? (yes/no)

[You]: yes

[SEAL]:
✓ Pattern updated: "List Pagination Pattern"
  - New step: Check for existing pagination components before installing
  - Confidence: 0.91 → 0.93

Please rate this task (1-5):

[You]: 5

[SEAL]:
## Learning Applied ✓
- Updated pattern with component reuse check
- Success rate: 100% (7/7)
- Average time: 12 minutes

Thanks! Next pagination task will be even faster.
```

## Advanced: Custom Checkpoints

Create custom checkpoint rules:

```javascript
// .claude/seal/checkpoints/custom-rules.js

module.exports = {
  // Checkpoint before modifying specific files
  protectedFiles: [
    'src/config/database.ts',
    'src/auth/permissions.ts'
  ],

  // Checkpoint for specific operations
  requireApproval: {
    deleteCode: true,  // Always ask before deleting code
    newEndpoints: true,  // Ask before adding API endpoints
    envChanges: true,  // Ask before modifying .env
  },

  // Auto-approve conditions
  autoApprove: {
    onlyComments: true,  // Auto-approve if only adding comments
    testFiles: true,  // Auto-approve test file changes
    documentation: true,  // Auto-approve doc updates
  },

  // Custom checkpoint logic
  customCheck: (operation) => {
    // Example: require approval for Friday deployments
    if (operation.type === 'deploy' && new Date().getDay() === 5) {
      return {
        require: true,
        reason: "Friday deployment - extra caution"
      };
    }
    return { require: false };
  }
};
```

Enable in config:
```json
{
  "custom_checkpoints": "checkpoints/custom-rules.js"
}
```

## Monitoring & Metrics

Track semi-autonomous performance:

```bash
/seal-metrics --semi-autonomous

## Semi-Autonomous Metrics

**Checkpoint Stats**:
- Total checkpoints triggered: 47
- Auto-approved: 23 (49%)
- Manually approved: 18 (38%)
- Rejected: 6 (13%)

**Most Common Checkpoints**:
1. Package installs (15 times)
2. Schema changes (12 times)
3. Breaking changes (8 times)

**Efficiency**:
- Average time saved vs manual: 65%
- Average checkpoints per task: 1.8
- Tasks with zero checkpoints: 34%

**Pattern Confidence Over Time**:
Week 1: 0.67 → Week 4: 0.89
(Fewer checkpoints needed as patterns improve)
```

## Best Practices

### 1. Start with High Oversight

Begin with `checkpoint_frequency: high` for first 10 tasks:
- Learn what SEAL does well
- Build trust gradually
- Refine patterns with feedback

### 2. Gradually Reduce Checkpoints

As patterns improve:
```bash
# Week 1-2: High oversight
/seal-config --checkpoints high

# Week 3-4: Medium oversight
/seal-config --checkpoints medium

# Week 5+: Low oversight for trusted patterns
/seal-config --checkpoints low
```

### 3. Keep Critical Checkpoints

Always require approval for:
- Production deployments
- Database migrations
- Security changes
- Dependency updates

### 4. Review Rejected Checkpoints

Weekly review:
```bash
/seal-metrics --rejected-checkpoints

# Learn why you rejected:
# - Was pattern wrong?
# - Should pattern be updated?
# - Is this an exception?
```

### 5. Share Team Patterns

Export learned patterns for team:
```bash
/seal-export-patterns > team-patterns.json

# Team members import:
/seal-import-patterns team-patterns.json
```

## Troubleshooting

### "Too many checkpoints, slowing me down"

```bash
# Lower frequency
/seal-config --checkpoints low

# Raise auto-approve threshold
/seal-config --auto-approve 0.8

# Remove specific checkpoint types
/seal-config --remove-checkpoint "package_installs"
```

### "SEAL proceeding without approval"

Check config:
```bash
/seal-status

# Ensure checkpoints enabled:
{
  "checkpoint_frequency": "medium",  // not "none"
  "auto_approve_confidence_threshold": 0.9  // not 0.0
}
```

### "Want to approve entire project at once"

```bash
# Batch approval mode
/seal-batch-approve

[SEAL]: Batch mode: I'll execute all steps and present final result for review.
Checkpoints will be logged but not block execution.

Continue? (yes/no)
```

## Comparison: Full Autonomy vs Semi-Autonomous

| Aspect | Full Autonomy | Semi-Autonomous | Manual |
|--------|--------------|-----------------|--------|
| Human checkpoints | 0 | 1-5 per task | Constant |
| Learning speed | Fast | Fast | Slow |
| Control | Low | High | Total |
| Efficiency | Highest | High | Lowest |
| Trust required | High | Medium | None |
| Best for | Well-known tasks | Learning phase | Critical tasks |

## Next Steps

1. **Initialize with moderate oversight**:
   ```bash
   /seal-start --profile moderate
   ```

2. **Complete 5 tasks** with manual review:
   - Rate each task
   - Approve/reject checkpoints
   - Provide feedback

3. **Review learned patterns**:
   ```bash
   /seal-metrics --patterns
   ```

4. **Adjust oversight** based on confidence:
   ```bash
   /seal-config --checkpoints low  # if patterns look good
   ```

5. **Iterate to optimal balance**:
   - Monitor rejected checkpoints
   - Adjust thresholds
   - Find your sweet spot

---

**The goal**: SEAL handles 80% autonomously, you oversee 20% critical decisions.

**Start conservative, trust through verification, automate with confidence.**

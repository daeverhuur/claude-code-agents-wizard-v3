# Checkpoint & Recovery Protocol

This document defines the checkpoint system for the SaaS Generator, enabling recovery from failures without restarting the entire workflow.

## Why Checkpoints Matter

Per Anthropic's guidance: *"Design systems to resume from checkpoints rather than restart entirely."*

Without checkpoints:
- A Step 8 failure means re-running Steps 1-7
- Hours of work can be lost
- User frustration increases
- Token costs multiply unnecessarily

With checkpoints:
- Can recover to last known-good state
- Resume from failure point
- Preserve completed work
- Reduce token costs on retry

## Checkpoint Architecture

### Git-Based Checkpoints

We use git commits as checkpoints. Each checkpoint:
- Captures complete project state
- Is recoverable via `git reset`
- Creates audit trail
- Enables diff-based debugging

### Checkpoint Points

| Step | After | Checkpoint Message | Why |
|------|-------|-------------------|-----|
| 4 | Backend Built | `CHECKPOINT: Convex backend complete` | Schema is foundation |
| 5.5 | Stripe Setup | `CHECKPOINT: Stripe configured` | Payment config is critical |
| 6 | Landing Pages | `CHECKPOINT: Landing pages generated` | 70+ pages, expensive to regenerate |
| 7 | Frontend Built | `CHECKPOINT: Frontend built` | Major milestone |
| 8.5 | Refiner Done | `CHECKPOINT: Refinement complete` | Ready for final testing |

### Checkpoint Commands

```bash
# Create checkpoint
git add -A && git commit -m "CHECKPOINT: [Description]"

# View checkpoints
git log --oneline | grep CHECKPOINT

# Recover to last checkpoint
git reset --hard HEAD~1

# Recover to specific checkpoint
git log --oneline | grep CHECKPOINT
git reset --hard <commit-hash>
```

## Recovery Scenarios

### Scenario 1: Frontend Build Fails Badly

**Problem:** nextjs-builder creates broken code that can't be fixed.

**Recovery:**
```bash
# Reset to before frontend
git reset --hard HEAD~1  # Goes back to "CHECKPOINT: Landing pages generated"

# Verify state
git log -1 --oneline  # Should show landing pages checkpoint

# Re-run Step 7 with different approach
```

### Scenario 2: Landing Pages Have Major Issues

**Problem:** Landing pages have fundamental content problems.

**Recovery:**
```bash
# Reset to before landing pages
git log --oneline | grep CHECKPOINT
git reset --hard <stripe-checkpoint-hash>

# Verify state
ls /landing-pages/  # Should be empty

# Re-run Step 6 with corrected inputs
```

### Scenario 3: Stripe Config Wrong

**Problem:** Wrong products/prices created.

**Recovery:**
```bash
# Reset to before Stripe
git log --oneline | grep CHECKPOINT
git reset --hard <backend-checkpoint-hash>

# Additionally, clean up Stripe (manual or via CLI)
stripe products list
stripe products delete prod_xxx

# Re-run Step 5.5
```

### Scenario 4: Need to Start Fresh

**Problem:** Fundamental architecture issues discovered.

**Recovery:**
```bash
# Nuclear option - reset to initial state
git log --oneline
git reset --hard <first-commit-hash>

# Or if never committed
rm -rf .git
git init
```

## Checkpoint Implementation

### Orchestrator Responsibility

The orchestrator creates checkpoints at defined points:

```markdown
### After Step 6: Landing Page Generation

1. Verify all landing page agents completed
2. Count JSON files: `ls /landing-pages/**/*.json | wc -l`
3. Create checkpoint:
   ```bash
   git add -A && git commit -m "CHECKPOINT: Landing pages generated - [count] pages"
   ```
4. Log to observer: "Checkpoint created: Step 6"
```

### Checkpoint Verification

Before creating checkpoint, verify:
- [ ] Previous step completed successfully
- [ ] No pending errors or issues
- [ ] Build still passes (for code checkpoints)
- [ ] Required files exist

### Progress File Updates

Each checkpoint updates `/progress/status.md`:

```markdown
## Checkpoint Log

| Checkpoint | Commit | Timestamp | Status |
|------------|--------|-----------|--------|
| Backend Complete | abc1234 | 10:15:00 | ✅ |
| Stripe Configured | def5678 | 10:25:00 | ✅ |
| Landing Pages Generated | ghi9012 | 10:45:00 | ✅ |
| Frontend Built | jkl3456 | 11:00:00 | ✅ |
| Refinement Complete | mno7890 | 11:15:00 | ✅ |
```

## Session Recovery

### Recovering After Session Break

If a session is interrupted (context limit, user break, etc.):

1. **Read progress files first:**
   ```
   Read /progress/status.md
   Read /progress/stripe-config.md
   Read /progress/landing-pages.md
   ```

2. **Check git status:**
   ```bash
   git log --oneline | head -10
   git status
   ```

3. **Identify last checkpoint:**
   ```bash
   git log --oneline | grep CHECKPOINT | head -1
   ```

4. **Resume from checkpoint:**
   - If work exists after checkpoint: continue
   - If work is broken: reset to checkpoint
   - If checkpoint missing: identify last stable state

### Example: Session Recovery

```markdown
## Session Recovery

Last checkpoint: "CHECKPOINT: Frontend built" (commit jkl3456)

Progress files show:
- Steps 1-7: Complete
- Step 8: Partially complete (tester found issues)
- Step 8.5: Not started

Decision: Continue from Step 8 with tester results
```

## Error Recovery Patterns

### Pattern 1: Retry with Same Approach

```
Error occurs → stuck agent → human says "retry"
→ Reset to checkpoint
→ Re-invoke same agent with same inputs
```

### Pattern 2: Retry with Different Approach

```
Error occurs → stuck agent → human chooses alternative
→ Reset to checkpoint
→ Re-invoke agent with modified inputs
```

### Pattern 3: Skip and Continue

```
Error occurs → stuck agent → human says "skip for now"
→ Log issue to /progress/errors.md
→ Continue to next step
→ Address skipped issue in refinement
```

### Pattern 4: Escalate and Pause

```
Error occurs → stuck agent → human needs time
→ Create checkpoint
→ Log full state to progress files
→ End session gracefully
→ Resume later from checkpoint
```

## Checkpoint Best Practices

### DO:
- Create checkpoints after every major milestone
- Verify checkpoint before continuing
- Update progress files with checkpoint info
- Test that reset actually works (occasionally)
- Include meaningful descriptions in commit messages

### DON'T:
- Create checkpoints mid-step (unstable state)
- Skip checkpoints to "save time"
- Rely solely on checkpoints (also use progress files)
- Create too many checkpoints (noise)
- Forget to push checkpoints for persistence

## Emergency Procedures

### Complete System Failure

If everything breaks:

1. **Preserve evidence:**
   ```bash
   git stash  # Save current state
   git log > /tmp/git-log.txt
   cp -r /progress /tmp/progress-backup
   ```

2. **Reset to last known-good:**
   ```bash
   git log --oneline | grep CHECKPOINT
   git reset --hard <earliest-good-checkpoint>
   ```

3. **Verify recovery:**
   ```bash
   npm run build  # Should pass
   npm run dev    # Should start
   ```

4. **Resume with caution:**
   - Re-read all progress files
   - Understand what went wrong
   - Proceed step-by-step with extra verification

### Corrupted Git State

If git itself is corrupted:

1. **Backup current state:**
   ```bash
   cp -r . ../project-backup
   ```

2. **Reinitialize git:**
   ```bash
   rm -rf .git
   git init
   git add -A
   git commit -m "Recovery: Reinitializing from corrupted state"
   ```

3. **Note in progress files:**
   - Git history was lost
   - Checkpoints no longer available
   - Proceeding without recovery option

## Integration with Observer

The observer agent logs all checkpoints:

```markdown
### Checkpoint Created
- **Time**: 10:45:00
- **Step**: 6 (Landing Pages)
- **Commit**: ghi9012
- **Files**: 70 landing page JSONs
- **Build Status**: N/A (no code changes)
- **Notes**: All 6 parallel agents completed successfully
```

This creates an audit trail for debugging and optimization.

## Summary

Checkpoints are insurance against failure. The cost of creating them (a few git commits) is negligible compared to the cost of re-running hours of work.

**Remember:**
- Checkpoint after major milestones
- Verify before checkpointing
- Document in progress files
- Know how to recover
- Don't skip checkpoints

---

*Last Updated: December 2025*
*For: Claude Code SaaS Generator v3*

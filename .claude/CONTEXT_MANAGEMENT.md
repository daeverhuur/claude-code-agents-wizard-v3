# Context Management Guide for Claude Code SaaS Generator

## Overview

This guide documents context management strategies for the SaaS Generator system based on latest research (November 2025). Effective context management is critical when orchestrating multiple agents to build complete SaaS applications.

**Key Insight**: Context QUALITY matters more than quantity. Bad context destroys good context.

---

## 1. The 80/20 Context Quality Rule

### The Problem
- A 3,000-token feature specification gets destroyed when dumped alongside 50,000 tokens of irrelevant logs
- Bad context (logs, error traces, old decisions) is cheap but toxic
- Signal drowns in noise, leading to worse outputs

### The Solution
**Prioritize high-quality, relevant context over comprehensive but noisy context.**

**Good Context:**
- Feature specifications
- Schema decisions
- API documentation (from research-agent)
- Design system guidelines
- Current implementation status
- User requirements

**Bad Context (Filter or Isolate):**
- Full npm install logs
- Verbose error stack traces (summarize instead)
- Old implementation attempts
- Debug output
- Repetitive test results

### For SaaS Generator Orchestrator
```markdown
GOOD: "User wants thumbnail generator with Gemini, needs 70 landing pages"
BAD: Dumping 50k tokens of previous conversation about unrelated app
```

---

## 2. Subagent Context Isolation Pattern

### Why This Matters
Subagents burn tokens in **ISOLATED context windows**. Their exploration doesn't pollute the main orchestrator's context.

### The Pattern

**Main Thread (Orchestrator):**
- Stays clean with high-quality context
- Receives ONLY distilled summaries from subagents
- Maintains 76% signal ratio

**Subagent Threads:**
- Can use 180k tokens exploring, researching, debugging
- Returns 500-token summary to main thread
- Burns context in isolation

### Example: research-agent
```
SUBAGENT USES: 45,000 tokens
- Scrapes AI SDK docs (15k tokens)
- Scrapes Google Gemini docs (20k tokens)
- Analyzes model capabilities (10k tokens)

SUBAGENT RETURNS: 800 tokens
- Exact model name: "gemini-2.0-flash-exp"
- Import path: @google/generative-ai
- Key capabilities: multimodal, streaming
- Code example: 200 tokens
```

### For All Agent Implementations

**Agents MUST return concise summaries:**
```markdown
TASK COMPLETE: [Feature name]

RESULT:
- [1-2 sentence summary of what was built]

FILES CHANGED:
- convex/schema.ts (added subscriptions table)
- convex/stripe.ts (created webhook handlers)
- lib/stripe/config.ts (created)

KEY DECISIONS:
- Using Stripe Checkout instead of Elements (simpler for MVP)
- Webhook validates signatures before processing

NEXT STEPS:
- Test webhook with Stripe CLI
- Configure production webhook URL

[NO raw logs, NO full file contents, NO verbose explanations]
```

**DO NOT return:**
- Full file contents (use file paths instead)
- Raw npm install logs
- Verbose stack traces
- Step-by-step narratives

---

## 3. Manual vs Auto-Compact Strategy

### The Problem with Auto-Compact
- Wastes 45k tokens as buffer
- Triggers at unpredictable times
- Doesn't preserve important context strategically

### The Solution: Manual Compaction

**DISABLE auto-compact** and use `/compact` MANUALLY at natural breakpoints:

**When to Compact:**
1. ✅ **After completing a feature**
   - Research phase complete → `/compact preserve research findings and model names`
   - Convex backend built → `/compact preserve schema design and function signatures`
   - Landing pages generated → `/compact preserve landing page count and categories`

2. ✅ **Before switching tasks**
   - Switching from backend to frontend → `/compact preserve API endpoints and data structures`
   - Switching from implementation to testing → `/compact preserve feature list and expected behaviors`

3. ✅ **After commits**
   - Git commit created → `/compact preserve commit message and deployment status`

4. ✅ **When switching domains**
   - Frontend → Backend → `/compact preserve UI requirements and user flows`
   - Stripe setup → Landing pages → `/compact preserve pricing tiers and product IDs`

### Preservation Instructions

**Bad compaction:**
```
/compact
```
(Loses important context!)

**Good compaction:**
```
/compact preserve the following:
- Stripe product IDs: prod_xxx (Pro), prod_yyy (Enterprise)
- Pricing tiers: Free (10/mo), Pro ($29/mo, 500/mo), Enterprise ($99/mo, unlimited)
- AI model: gemini-2.0-flash-exp
- Project directory: C:\Users\...\my-app
- Landing pages: 70 total across 6 categories
```

### For SaaS Generator Workflow

**Strategic compaction points:**

```
STEP 1: Design Generation ✅
→ /compact preserve design system colors, component patterns, pricing page layout

STEP 3: Research Complete ✅
→ /compact preserve exact model name, import paths, API patterns

STEP 4: Convex Backend Built ✅
→ /compact preserve schema tables, function names, webhook endpoints

STEP 5.5: Stripe Setup Complete ✅
→ /compact preserve product IDs, price IDs, webhook secret status

STEP 6: Landing Pages Generated ✅
→ /compact preserve landing page count, categories, JSON file locations

STEP 8: Testing Complete ✅
→ /compact preserve test results, deployment readiness status
```

---

## 4. MCP Server Optimization

### What Are MCP Servers?
Model Context Protocol servers provide additional capabilities (database access, APIs, tools) but consume context tokens even when unused.

### Optimization Strategy

**Check enabled servers:**
```
/mcp
```

**Disable unused servers:**
- Only enable servers needed for current task
- Each enabled server consumes baseline context tokens
- Re-enable when needed

### For SaaS Generator

**During research phase:**
- Enable: HTTP/fetch server (for Jina API)
- Disable: Database servers, deployment tools

**During backend building:**
- Enable: Filesystem, git
- Disable: HTTP servers

**During testing:**
- Enable: Filesystem, HTTP (for API testing)
- Disable: Unused integrations

---

## 5. Scratchpad Pattern

### The Problem
Conversation history grows unbounded. Tracking progress in conversation wastes context.

### The Solution
Create markdown files to track progress instead of relying on conversation memory.

### Implementation

**Create tracking files:**

**`/progress/orchestration-status.md`:**
```markdown
# SaaS Generator Progress

## User Inputs Collected
- [x] App description: Thumbnail generator
- [x] AI provider: Google Gemini
- [x] AI model: gemini-2.0-flash-exp
- [x] Jina API key: ✓
- [x] Clerk credentials: ✓
- [x] Stripe API key: ✓
- [x] Project directory: C:\Users\...\thumbnail-gen

## Steps Completed
- [x] Step 1: Design generation
- [x] Step 2: Environment setup
- [x] Step 3: Research
- [x] Step 4: Convex backend
- [x] Step 5: AI implementation
- [x] Step 5.5: Stripe setup
- [ ] Step 6: Landing pages (4/6 agents complete)
- [ ] Step 7: Next.js frontend
- [ ] Step 8: Testing
- [ ] Step 9: GitHub deployment

## Current Status
Working on: Landing page generation
Next: Wait for all 6 agents to complete
```

**`/progress/stripe-config.md`:**
```markdown
# Stripe Configuration

## Products Created
- Pro: prod_Qxxxxxxxxxxxx
- Enterprise: prod_Qyyyyyyyyyy

## Prices Created
- Pro Monthly: price_1Qxxxxxxxxxxxx ($29/mo)
- Pro Yearly: price_1Qyyyyyyyyyy ($290/yr)
- Enterprise Monthly: price_1Qzzzzzzzzzz ($99/mo)
- Enterprise Yearly: price_1Qaaaaaaaaa ($990/yr)

## Webhook Status
- Endpoint created: ✓
- Secret set in Convex: ✓
- Local testing: Pending
```

**`/progress/landing-pages-status.md`:**
```markdown
# Landing Page Generation Progress

## Agents Spawned: 6

### Agent 1: Feature Pages (12 pages)
Status: ✅ Complete
Files: /landing-pages/features/*.json

### Agent 2: Use Case Pages (12 pages)
Status: ✅ Complete
Files: /landing-pages/use-cases/*.json

### Agent 3: Industry Pages (12 pages)
Status: ⏳ In Progress
Files: /landing-pages/industries/*.json

### Agent 4: Comparison Pages (12 pages)
Status: ⏳ In Progress
Files: /landing-pages/vs/*.json

### Agent 5: Problem/Solution Pages (12 pages)
Status: ⏳ Pending
Files: /landing-pages/solutions/*.json

### Agent 6: Pricing Pages (10 pages)
Status: ⏳ Pending
Files: /landing-pages/pricing/*.json

## Total: 24/70 pages complete
```

### Benefits
- Claude references and updates files, not memory
- Files persist across sessions
- Context-efficient (read once, update incrementally)
- Clear audit trail

### Usage in Orchestrator
```
1. Read /progress/orchestration-status.md
2. Update status after each step
3. Reference in /compact preservation
4. Share with user at end
```

---

## 6. Read vs Write Isolation

### The Pattern

**READ operations:** Use parallel subagents (no conflicts possible)
```
Spawn 6 landing-page-generator agents simultaneously
Each reads shared research docs
Each writes to isolated JSON files
Main thread waits for all completions
```

**WRITE operations:** Single-threaded in main context (avoid conflicts)
```
Don't spawn 3 agents to edit the same convex/schema.ts file
One agent owns one file (or clearly separated sections)
```

### For SaaS Generator

**Safe to parallelize:**
- Landing page generation (each writes separate JSON files)
- Research scraping (read-only operations)
- Testing (read-only validation)

**Must serialize:**
- Environment setup (one .env.local file)
- Convex schema changes (one schema.ts file)
- Next.js app building (potential file conflicts)

---

## 7. Subagent Return Format Standard

### Mandatory Format for ALL Agents

```markdown
TASK COMPLETE: [Agent name - Feature implemented]

RESULT:
- [1 sentence: What was built]
- [1 sentence: Key outcome or metric]

FILES CHANGED:
- /absolute/path/to/file.ts (added)
- /absolute/path/to/file2.tsx (modified)
- /absolute/path/to/file3.css (deleted)

KEY DECISIONS:
- [Important choice made and why]
- [Important choice made and why]

BLOCKERS/ISSUES:
- [Any problems encountered]
- [Workarounds applied]

NEXT STEPS:
- [What needs to happen next, if anything]

---
[Optional: 1-2 line code snippet if critical]
[NO raw logs, NO full file dumps, NO verbose explanations]
```

### Examples

**Good Return:**
```markdown
TASK COMPLETE: stripe-builder - Automatic Stripe setup

RESULT:
- Created 2 Stripe products (Pro, Enterprise) with 4 prices via CLI
- Configured webhook endpoint pointing to Convex HTTP action

FILES CHANGED:
- /lib/stripe/config.ts (created - product/price IDs)
- /lib/stripe/plans.ts (created - feature limits)
- /convex/stripe/webhook.ts (created - event handlers)
- /convex/http.ts (modified - added webhook route)

KEY DECISIONS:
- Used Stripe CLI instead of API for product creation (simpler, repeatable)
- Set webhook to point to Convex HTTP action (serverless, auto-scaling)

BLOCKERS/ISSUES:
- None

NEXT STEPS:
- Test webhook locally with Stripe CLI listen command
- Configure production webhook URL after deployment
```

**Bad Return:**
```markdown
I've completed the Stripe setup! Here's what I did:

First, I installed the Stripe CLI and authenticated...
[500 lines of narrative]

Then I ran this command:
stripe products create --name "Pro Plan" --description "Professional features"
[output log 200 lines]

And here's the full file I created:
[paste entire 300-line file]

Let me know if you need anything else!
```

### Enforcement

Orchestrator should:
1. Expect concise returns
2. Warn if agent returns >2000 tokens
3. Ask agent to summarize if needed

---

## 8. When to /clear vs /compact

### /compact: Same Project Continuation

**Use when:**
- Natural milestone reached
- Switching between related tasks
- Need to preserve schema, decisions, config
- Same project, same day

**Example:**
```
Completed: Convex backend
Next: AI implementation
Action: /compact preserve schema tables, function names, API endpoints
```

### /clear: Hard Reset

**Use when:**
- Switching to completely unrelated project
- New day, new app generation
- After multiple compactions (context drift)
- Starting fresh troubleshooting session

**Example:**
```
Completed: Thumbnail generator app
Next: Build chat bot app (different user, different requirements)
Action: /clear
```

### For SaaS Generator

**Typical session (one app):**
```
Start → Design → (compact) → Research → (compact) → Backend → (compact)
→ AI → (compact) → Stripe → (compact) → Landing pages → (compact)
→ Frontend → (compact) → Testing → Deploy → END
```

**Multiple apps in one day:**
```
App 1 complete → /clear → App 2 start
```

**Debugging after deployment:**
```
Production issue → /clear → Fresh debugging session
```

---

## 9. Context Meter Monitoring

### Check Context Usage
```
/context
```

**Output example:**
```
Context: 87,453 / 200,000 tokens (43%)
```

### Thresholds

**0-50% (Green Zone):**
- Optimal performance
- Continue normally

**50-70% (Yellow Zone):**
- Performance still good
- Plan next compaction point
- Avoid dumping large logs

**70-85% (Orange Zone):**
- **Compact immediately**
- Preserve critical context with instructions
- Performance degradation begins

**85-100% (Red Zone):**
- **Compact NOW or /clear**
- Severe performance degradation
- Risk of context overflow
- Reserve final 20% for complex operations

### For SaaS Generator

**Monitor at checkpoints:**
```
After Step 3 (Research): Should be ~30-40%
After Step 4 (Convex): Should be ~50-60%
After Step 6 (Landing pages): Should be ~70-80% → COMPACT
After Step 7 (Frontend): After compaction ~40-50%
After Step 8 (Testing): Should be ~60-70%
```

**If approaching 80% mid-step:**
1. Pause current operation
2. `/compact preserve [current step critical info]`
3. Resume operation

---

## 10. Token-Efficient Prompting

### The Problem
Verbose prompts waste tokens and reduce response quality.

### Natural Language > Verbose Instructions

**Bad (verbose, wastes tokens):**
```
Please read the following files:
- C:\Users\...\convex\schema.ts
- C:\Users\...\convex\uploads.ts
- C:\Users\...\convex\projects.ts
- C:\Users\...\convex\images.ts
- C:\Users\...\convex\thumbnails.ts

And then fix any TypeScript errors you find in each file by:
1. Reading the error
2. Understanding the type mismatch
3. Correcting the type annotation
4. Saving the file
```

**Good (concise, natural):**
```
Fix TypeScript errors in convex/*.ts files
```

### Reference Files, Don't Paste

**Bad:**
```
Here's the schema file:
[paste 300 lines]

Here's the function file:
[paste 200 lines]

Now add a new field...
```

**Good:**
```
Add `subscriptionTier` field to users table in convex/schema.ts
Reference existing pattern from projects table
```

### Use @file Syntax

**Instead of:**
```
Read the file at C:\Users\...\convex\schema.ts and look at the users table
```

**Use:**
```
@convex/schema.ts - add subscriptionTier field to users table
```

### For SaaS Generator Orchestrator

**When invoking agents:**

**Bad:**
```
I need you to build a Convex backend. The user wants these features:
1. User authentication
2. File uploads
3. Image generation
4. Thumbnail storage
5. Real-time sync
...
[500 words describing each feature in detail]
```

**Good:**
```
Build Convex backend for thumbnail generator app.

Features:
- Upload images
- Generate thumbnails with AI
- Store results
- Real-time sync

Research docs: /research/*.md
Design reference: /design/dashboard.html
```

---

## 11. SaaS Generator-Specific Best Practices

### Context Budget Allocation

**Total context: 200k tokens**

**Budget allocation:**
- Orchestrator conversation: 40k tokens (20%)
- User inputs & requirements: 10k tokens (5%)
- Research documentation: 20k tokens (10%)
- Design files: 10k tokens (5%)
- Progress tracking: 5k tokens (2.5%)
- Agent returns (cumulative): 15k tokens (7.5%)
- **Reserve for responses: 100k tokens (50%)**

### Critical Compaction Points

**Step 3 Complete (Research):**
```
/compact preserve:
- Exact AI model name: gemini-2.0-flash-exp
- Import path: @google/generative-ai
- Key API patterns from research
- Convex patterns from docs
```

**Step 5.5 Complete (Stripe):**
```
/compact preserve:
- Product IDs: prod_xxx (Pro), prod_yyy (Enterprise)
- Price IDs: price_1xxx (Pro monthly $29), etc.
- Webhook secret set: ✓
- Feature limits: Free (10/mo), Pro (500/mo), Enterprise (unlimited)
```

**Step 6 Complete (Landing Pages):**
```
/compact preserve:
- Total landing pages: 70
- Categories: features (12), use-cases (12), industries (12), comparisons (12), solutions (12), pricing (10)
- All JSON files created in /landing-pages/
- Ready for nextjs-builder
```

### Parallel Agent Pattern

**When spawning 6 landing-page agents:**

**DON'T do this (serial, wastes time and context):**
```
Invoke agent 1 → wait → read return → invoke agent 2 → wait → read return...
```

**DO this (parallel):**
```
Invoke all 6 agents simultaneously
Wait for all completions (check file system or agent returns)
Read summary of each agent's output
Proceed to next step
```

### Scratchpad Files for Multi-Agent Coordination

**Create before Step 6:**
```
/progress/landing-pages-assignments.md
```

**Content:**
```markdown
# Landing Page Agent Assignments

Total pages: 70

## Agent 1: Feature Pages
- Category: features
- Count: 12
- Topics: AI generation, batch processing, templates, etc.
- Status: ⏳ Assigned

## Agent 2: Use Case Pages
- Category: use-cases
- Count: 12
- Topics: Marketing teams, content creators, etc.
- Status: ⏳ Assigned

[etc for all 6 agents]
```

**Update as agents complete:**
```markdown
## Agent 1: Feature Pages
- Category: features
- Count: 12
- Topics: AI generation, batch processing, templates, etc.
- Status: ✅ Complete (12 JSON files created)
```

**Reference in orchestrator:**
```
Read /progress/landing-pages-assignments.md
→ See that 4/6 agents complete
→ Wait for remaining 2
→ Once all ✅, proceed to Step 7
```

---

## 12. Anti-Patterns to Avoid

### ❌ Dumping Full Logs
```
Here's the npm install output:
[3000 lines]
```
**Instead:** "npm install completed successfully" or "npm install failed with EACCES error on package X"

### ❌ Pasting Full Files Unnecessarily
```
Here's the updated schema.ts:
[paste entire 400-line file]
```
**Instead:** "Added subscriptions table to convex/schema.ts with fields: userId, stripeCustomerId, status, currentPeriodEnd"

### ❌ Repeating Context Every Message
```
So to recap, the user wants a thumbnail generator using Gemini with 70 landing pages and Stripe payments and Clerk auth and...
[Repeat everything every response]
```
**Instead:** Reference progress files or assume context continuity

### ❌ Over-Explaining Agent Tasks
```
I'm now going to invoke the research-agent. This agent will scrape documentation from Jina AI. First it will get the AI SDK docs, then it will...
[500 words explaining what the agent does]
```
**Instead:** "Invoking research-agent to scrape AI SDK and Gemini docs"

### ❌ Not Using Subagent Isolation
```
[Orchestrator directly scrapes 50k tokens of documentation]
[Main context polluted]
```
**Instead:** Invoke research-agent subagent, receive 800-token summary

### ❌ Sequential When Parallel Is Possible
```
Invoke landing-page-agent-1 → wait for completion
Invoke landing-page-agent-2 → wait for completion
[6 sequential invocations = 6x slower]
```
**Instead:** Invoke all 6 agents in parallel, wait for all completions

---

## 13. Emergency Context Recovery

### If Context Becomes Degraded

**Symptoms:**
- Responses ignore previous decisions
- Hallucinating features not requested
- Forgetting critical config (API keys, project directory)
- Degraded output quality

**Recovery Steps:**

1. **Check context usage:**
   ```
   /context
   ```

2. **If >80%, compact immediately:**
   ```
   /compact preserve critical context from /progress/*.md files
   ```

3. **If compaction doesn't help, /clear and restore:**
   ```
   /clear
   ```
   Then restore from scratchpad files:
   ```
   Read /progress/orchestration-status.md
   Read /progress/stripe-config.md
   Read /progress/landing-pages-status.md
   ```

4. **Resume from last checkpoint:**
   ```
   Based on progress files:
   - Steps 1-5.5 complete
   - Currently on Step 6 (4/6 agents done)
   - Resume: Wait for agents 5-6, then proceed to Step 7
   ```

---

## 14. Metrics for Success

### Context Efficiency Metrics

**Signal Ratio:**
- Target: >70% high-quality context
- Calculate: (Feature specs + decisions + research) / (Total context)
- If <50%, too much noise (logs, repetition)

**Agent Return Efficiency:**
- Target: <1500 tokens per agent return
- Measure: Average tokens in subagent summaries
- If >3000, agents are too verbose

**Compaction Frequency:**
- Target: Every 40-60k tokens OR major milestone
- Too frequent: <20k intervals (wasting time)
- Too rare: >80k intervals (degraded performance)

### For SaaS Generator

**Per-app token budget:**
- Target: <120k tokens per complete app generation
- Including: All agent invocations, returns, orchestration
- Excluding: Subagent internal context (isolated)

**Breakdown:**
- Orchestration overhead: ~20k
- Design generation: ~8k
- Research: ~15k
- Convex backend: ~12k
- AI implementation: ~10k
- Stripe setup: ~8k
- Landing pages (6 agents): ~30k (5k each)
- Frontend building: ~15k
- Testing: ~8k
- Deployment: ~4k
- **Total: ~130k tokens** (65% of budget)

**Remaining 70k tokens** reserved for:
- User clarifications
- Error recovery
- Testing iterations
- Complex responses

---

## 15. Quick Reference Checklist

### Before Starting SaaS Generation

- [ ] /context check (should be <20%)
- [ ] Create /progress/ tracking files
- [ ] Disable unused MCP servers (/mcp)
- [ ] Confirm subagents will return concise summaries

### During Generation (Per Step)

- [ ] After agent completes: Check return is <2000 tokens
- [ ] Update /progress/ files
- [ ] Check /context - if >70%, compact

### After Major Milestones

- [ ] Compact with preservation instructions
- [ ] Update all /progress/ files
- [ ] Verify critical info preserved

### Before Deployment

- [ ] Review /progress/ for completeness
- [ ] Final /context check
- [ ] Prepare deployment summary from progress files

---

## Conclusion

Effective context management is about **quality over quantity**, **isolation over pollution**, and **strategic preservation over auto-compaction**.

**For the SaaS Generator:**
- Use subagents to isolate expensive operations
- Maintain scratchpad files for state tracking
- Compact strategically at milestones
- Require concise returns from all agents
- Monitor context meter proactively
- Parallelize when safe (reads), serialize when necessary (writes)

**Result:** Clean orchestration context that can guide 6+ parallel agents through 10 steps to produce a complete SaaS application with 70+ pages while staying under 150k tokens.

---

**Last Updated:** November 2025
**For:** Claude Code SaaS Generator v3
**Context Window:** 200k tokens

---
name: workflows:compound
description: Document recently solved problems (technical + product learnings)
argument-hint: "[optional: brief context about the fix]"
---

# /compound

Document a recently solved problem, capturing both technical solutions AND product learnings.

## Purpose

Captures problem solutions while context is fresh, creating:
1. **Technical documentation** in `docs/solutions/` (for engineers)
2. **Product learnings** in `PRODUCT.md` (for product-owner agent)

**Why "compound"?** Each documented solution compounds your team's knowledge. The first time you solve a problem takes research. Document it, and the next occurrence takes minutes. Knowledge compounds.

## Usage

```bash
/workflows:compound                    # Document recent fix + product learnings
/workflows:compound [brief context]    # Provide additional context hint
```

## What It Captures

### Technical Solutions (docs/solutions/)
- Problem symptom and error messages
- Investigation steps tried
- Root cause analysis
- Working solution with code
- Prevention strategies

### Product Learnings (PRODUCT.md)
- New product principles discovered
- Priority shifts based on evidence
- Domain knowledge gained
- Updated trade-off decisions

## Execution Flow

### Phase 1: Technical Documentation (Parallel)

Launches multiple subagents in parallel:

1. **Context Analyzer** - Extracts problem type, symptoms
2. **Solution Extractor** - Identifies root cause and fix
3. **Related Docs Finder** - Searches for related documentation
4. **Prevention Strategist** - Develops prevention strategies
5. **Category Classifier** - Determines correct docs/solutions/ category
6. **Documentation Writer** - Creates solution markdown file

### Phase 2: Product Learning Detection (Sequential)

After technical doc is created, analyze for product insights:

**Product Learning Extractor** asks:

1. **Did we discover a new product principle?**
   - When is completeness required vs YAGNI acceptable?
   - What makes features valuable or worthless?
   - When do users trust or abandon the product?

2. **Did priorities shift?**
   - Did user feedback change importance?
   - Did technical work reveal non-negotiable requirements?
   - Should something move between P0/P1/P2/P3?

3. **Did we gain domain knowledge?**
   - New understanding of how the domain works?
   - Non-obvious behavior that affects decisions?
   - Terminology or concepts that inform requirements?

4. **Did trade-offs get validated/invalidated?**
   - Did we learn when to prioritize X over Y?
   - Did assumptions about user needs change?

### Phase 3: PRODUCT.md Update (With Approval)

If product learnings found:

```
✓ Product learnings detected:

  1. NEW PRINCIPLE: "100% State Coverage Non-Negotiable"
     Section: ## Product Principles
     Rationale: Compliance domain requires completeness

  2. PRIORITY SHIFT: State coverage P1 → P0
     Section: ## Strategic Priorities  
     Rationale: User research showed 44% = abandonment

Update PRODUCT.md? (y/n)
```

If approved, updates PRODUCT.md with:
- New principles added to ## Product Principles
- Priorities updated in ## Strategic Priorities
- Domain knowledge added to ## Domain Knowledge
- Trade-off guidance added

Each update includes:
- What was learned
- Evidence/rationale
- Link to solution doc

## Example Output

```
✓ Technical documentation created:
  - docs/solutions/integration-issues/kaplan-state-coverage.md

✓ Product learnings detected:

  NEW PRINCIPLE: "Data Completeness > Engineering Simplicity"
  ├─ Evidence: 44% state coverage = $0 product value
  ├─ Rationale: Compliance domain requires 100% or users abandon
  └─ Added to: PRODUCT.md ## Product Principles

  PRIORITY SHIFT: Complete State Coverage (P1 → P0)
  ├─ Evidence: User interviews showed partial = deal-breaker
  ├─ Rationale: Non-negotiable for market entry
  └─ Updated: PRODUCT.md ## Strategic Priorities

  DOMAIN KNOWLEDGE: Multi-State Hour Inflation
  ├─ Evidence: Library courses show 8,767h (51 states × 24h)
  ├─ Rationale: Expected behavior, not data error
  └─ Added to: PRODUCT.md ## Domain Knowledge

✓ PRODUCT.md updated with 3 learnings

Next time product-owner reviews plans, it will have this context!
```

## Success Metrics

**Technical compound:**
- Solution documented in correct category
- Code examples included
- Prevention strategies captured
- Searchable via YAML frontmatter

**Product compound:**
- Product-owner agent has updated context
- New principles guide future decisions
- Priorities reflect learned importance
- Domain knowledge prevents confusion

## The Compounding Philosophy

**Technical Knowledge:**
1. First time: Research solution (30 min)
2. Document: Create docs/solutions/ entry (5 min)
3. Next time: Quick lookup (2 min)

**Product Knowledge:**
1. First time: Discover principle through work
2. Document: Update PRODUCT.md (5 min)
3. Next time: Product-owner references it in reviews

Both compound. Each unit of work makes subsequent work easier.

## Implementation Details

### Product Learning Detection

When analyzing completed work, look for:

**Signals of new product principles:**
- User feedback that changed direction
- Discovering completeness requirements
- Learning when partial = worthless
- Understanding trust/abandonment triggers

**Signals of priority shifts:**
- "This is more critical than we thought"
- "Users won't use the product without X"
- "This blocks everything else"

**Signals of domain knowledge:**
- "That's actually how the domain works"
- "This behavior is expected, not a bug"
- "Industry terminology means X not Y"

**Signals of trade-off updates:**
- "We thought users wanted X, they need Y"
- "Speed matters more/less than we thought"
- "Simplicity breaks value in this case"

### PRODUCT.md Update Format

**New Principle:**
```markdown
### [Principle Number]. [Principle Name]

[Clear statement of what you prioritize and why]

**When to push back on YAGNI:**
- [Scenario where this principle applies]

**Example:**
- [Concrete example from the work you just completed]
- [Evidence: link to solution doc]

**Learned:** [Date] during [work completed]
```

**Priority Update:**
```markdown
### P[0-3]: [Feature/Capability]
**Why:** [Updated rationale with evidence]
**Done when:** [Completion criteria]
**Updated:** [Date] - [What changed and why]
**Evidence:** docs/solutions/[category]/[filename].md
```

**Domain Knowledge:**
```markdown
### [Concept Name]

[Explanation of domain concept]

**Why this matters:** [How it affects product decisions]

**Example:** [From the work you completed]

**Learned:** [Date] - [Evidence/link]
```

## Preconditions

<preconditions enforcement="advisory">
  <check condition="problem_solved">
    Problem has been solved (not in-progress)
  </check>
  <check condition="solution_verified">
    Solution has been verified working
  </check>
  <check condition="non_trivial">
    Non-trivial problem (not simple typo)
  </check>
</preconditions>

## Auto-Invoke

<auto_invoke>
  <trigger_phrases>
    - "that worked"
    - "it's fixed"  
    - "working now"
    - "problem solved"
  </trigger_phrases>
  
  <manual_override>
    Use /workflows:compound [context] to document immediately
  </manual_override>
</auto_invoke>

## Routes To

`compound-docs` skill (extended with product learning extraction)

## Related Commands

- `/workflows:plan` - References PRODUCT.md (now kept current)
- `/plan_review` - Product-owner uses updated PRODUCT.md
- `/workflows:review` - Reviews against current PRODUCT.md

## After Technical Documentation

Once `docs/solutions/` entry is created, analyze for product learnings:

### Product Learning Questions

Ask yourself after documenting the fix:

**1. New Product Principle?**
```
Did this work reveal when completeness matters vs when YAGNI applies?
Did you discover what makes features valuable vs worthless?
Did you learn what builds or breaks user trust?

If yes → Add to PRODUCT.md ## Product Principles
```

**2. Priority Shift?**
```
Did user feedback change what's important?
Did technical work reveal non-negotiable requirements?
Should something move between P0/P1/P2/P3?

If yes → Update PRODUCT.md ## Strategic Priorities
```

**3. Domain Knowledge?**
```
Did you learn how the domain actually works?
Did you discover non-obvious behavior?
Did you learn terminology that affects decisions?

If yes → Add to PRODUCT.md ## Domain Knowledge  
```

**4. Trade-off Update?**
```
Did you validate/invalidate when to prioritize X over Y?
Did assumptions about user needs change?

If yes → Update PRODUCT.md ## Common Trade-offs
```

### How to Update PRODUCT.md

If any learnings emerged:

```bash
# 1. Open PRODUCT.md
# 2. Add learning to appropriate section
# 3. Include evidence and date

# Example:
## Product Principles

### 5. Data Completeness > Engineering Simplicity

Compliance domains require 100% completeness. Partial data = $0 product value.

**When to push back on YAGNI:**
- State coverage: 100% required (not "ship 37/83 states")
- Provider data: All courses must be imported accurately

**Example:**
- State coverage plan: 44% vs 100% decision
- Evidence: docs/solutions/integration-issues/kaplan-state-coverage.md
- Learned: 2026-02-06 during insurance CE state coverage work

# 4. Commit with context
git add PRODUCT.md docs/solutions/
git commit -m "docs: Compound learnings (technical + product)

Technical: docs/solutions/[category]/[file].md
Product: Updated PRODUCT.md with [principle/priority/knowledge]

Evidence: [what we learned and why it matters]"
```

### Why This Matters

**Product-owner agent uses PRODUCT.md** in every review.

Updating it means:
- ✅ Future plans get validated against current principles
- ✅ Product-owner references real evidence (not outdated assumptions)
- ✅ Product knowledge compounds like technical knowledge

**Example:**

```
# Next time someone proposes partial coverage:

Product Owner: BLOCK - violates PRODUCT.md principle
"Data Completeness > Engineering Simplicity"

Evidence: docs/solutions/integration-issues/kaplan-state-coverage.md
shows 44% coverage = user abandonment

Learned: 2026-02-06 during state coverage work
```

Product knowledge compounds. Document it.

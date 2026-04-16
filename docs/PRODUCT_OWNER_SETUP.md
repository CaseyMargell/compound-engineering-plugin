# Product Owner Agent Setup Guide

The `product-owner` agent validates plans and PRs against product requirements defined in PRODUCT.md.

## Quick Setup

### 1. Create PRODUCT.md

```bash
# Copy template to your project
cp templates/PRODUCT.md /path/to/project/PRODUCT.md

# Fill in:
# - Target users
# - Strategic priorities (P0-P4)
# - Product principles
# - Domain knowledge
```

### 2. Use in Reviews

```bash
# Automatically included in /plan_review
/plan_review docs/plans/my-plan.md

# Runs 4 agents:
# - DHH Rails Reviewer
# - Kieran Rails Reviewer  
# - Code Simplicity Reviewer
# - Product Owner ← validates against PRODUCT.md
```

## What It Does

**Reads PRODUCT.md** and validates:
- Strategic alignment (P0-P4 priorities)
- Completeness requirements (when partial = worthless)
- User value delivery
- Trade-off acceptability

**Example:**

Plan proposes: "Ship with 40% coverage"

PRODUCT.md says: "P0: Complete coverage required"

Product Owner: **BLOCK** - Partial coverage = $0 value

## PRODUCT.md Structure

### Required

```markdown
## Market Reality
- Target User
- Core Job to Be Done
- Critical Success Factor

## Strategic Priorities
- P0: Non-negotiable
- P1-P3: Ranked priorities

## Product Principles
- What you prioritize
- When to push back on YAGNI
```

### Recommended

```markdown
## Domain Knowledge
- Key concepts
- Why they matter

## Common Trade-offs
- When to prioritize X over Y
```

## Reusable Across Projects

```
project-a/
├── PRODUCT.md    ← E-commerce context
└── .claude/

project-b/
├── PRODUCT.md    ← Compliance context
└── .claude/
```

Same agent, different context per project.

## Troubleshooting

**No PRODUCT.md found?**
```bash
cp templates/PRODUCT.md ./PRODUCT.md
```

**PRODUCT.md incomplete?**
- Fill in at least: Target users, P0 priority, 1 principle
- Evolve over time as you learn

## Best Practices

1. **Update as you learn** - Add principles from completed work
2. **Reference in proposals** - Show alignment with PRODUCT.md
3. **Use early** - Get product validation during planning, not just PR review
4. **Keep current** - Review priorities quarterly

## See Also

- `templates/PRODUCT.md` - Template to copy
- `agents/review/product-owner.md` - Agent implementation

---
name: product-acceptance
description: 'Product Owner review of implementation for product requirements compliance. Use when reviewing PRs, branches, or current changes against PRODUCT.md requirements before merging.'
argument-hint: "[PR number, branch name, or empty for current changes]"
---

# Product Acceptance Review

Invoke the product-owner agent to review an implementation (PR, branch, or current changes) against product requirements defined in PRODUCT.md.

## Prerequisites

PRODUCT.md must exist in the project root. If it does not exist, inform the user and skip product validation.

## Usage

```
/product-acceptance              # Review current uncommitted changes
/product-acceptance 42           # Review PR #42
/product-acceptance feat/auth    # Review branch feat/auth
```

## Execution Flow

### Step 1: Determine Review Target

Parse the argument:
- Numeric → PR review
- Non-empty string → branch review
- Empty → current uncommitted/staged changes

### Step 2: Gather Changed Files

**For PR:** Use `gh pr view <number> --json files` to list changed files.
**For branch:** Use `git diff --name-only main...<branch>` to list changed files.
**For current:** Use `git diff --name-only` + `git diff --cached --name-only`.

### Step 3: Invoke Product-Owner

Task compound-engineering:review:product-owner("Product acceptance review:

**Review Target:** <PR #/branch/current>
**Changed Files:** <list>

Evaluate against PRODUCT.md requirements:

**Must Have (BLOCK if missing):**
- Accurate data, functional core, no regressions, meets requirements

**Should Have (backlog if missing):**
- Edge cases, clear errors, acceptable performance

**Questions:**
1. Strategic alignment (P0-P4)?
2. Completeness requirements met?
3. User value delivered?
4. Data quality acceptable?
5. Product principles followed?

**Provide:** APPROVE / APPROVE WITH CONDITIONS / BLOCK with rationale")

## Post-Review Actions

- **APPROVE**: Ready to merge
- **APPROVE WITH CONDITIONS**: Address items or create backlog for post-merge
- **BLOCK**: Resolve issues before proceeding

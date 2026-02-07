---
name: product-acceptance
description: Product Owner review of implementation for product requirements compliance
argument-hint: "[PR number, branch name, or empty for current changes]"
---

# Product Acceptance Review

Invoke the product-owner agent to review an implementation (PR, branch, or current changes) against product requirements defined in PRODUCT.md.

## Usage

```bash
/product-acceptance              # Review current uncommitted changes
/product-acceptance 42           # Review PR #42
/product-acceptance feat/auth    # Review branch feat/auth
```

## Prerequisites

Check if PRODUCT.md exists:

```bash
test -f PRODUCT.md || echo "⚠️  No PRODUCT.md found. Product acceptance requires PRODUCT.md with product requirements."
```

If PRODUCT.md doesn't exist, skip product validation.

## Execution Flow

### Step 1: Determine Review Target

```bash
ARGUMENT="#$ARGUMENTS"

if [[ "$ARGUMENT" =~ ^[0-9]+$ ]]; then
  REVIEW_TYPE="pr"
elif [[ -n "$ARGUMENT" ]]; then
  REVIEW_TYPE="branch"
else
  REVIEW_TYPE="current"
fi
```

### Step 2: Gather Changed Files

**For PR:** `gh pr view $PR_NUMBER --json files -q '.files[].path'`  
**For branch:** `git diff --name-only main...$BRANCH`  
**For current:** `git diff --name-only` + `git diff --cached --name-only`

### Step 3: Invoke Product-Owner

Task compound-engineering:review:product-owner("Product acceptance review:

**Review Target:** <PR #/branch/current>
**Changed Files:** <list>

Evaluate against PRODUCT.md requirements:

**Must Have (BLOCK if missing):**
- Accurate data, functional core, no regressions, meets requirements

**Should Have (backlog):**
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
- **CONDITIONS**: Address items (create backlog for post-merge)
- **BLOCK**: Resolve issues before proceeding


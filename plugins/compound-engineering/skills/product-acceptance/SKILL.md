---
name: product-acceptance
description: 'Validate implementations against project-specific product requirements defined in PRODUCT.md. Use when reviewing PRs, branches, or current changes for product alignment before merging, or when checking whether completed work delivers the intended user value.'
argument-hint: "[PR number, branch name, or blank for current changes]"
disable-model-invocation: true
---

# Product Acceptance Review

Validate an implementation (PR, branch, or current changes) against product requirements defined in PRODUCT.md.

## The Problem This Solves

Engineering agents build the right thing *technically* — clean code, passing tests, good architecture. But nothing in the workflow validates whether the work aligns with product strategy, delivers user value, or meets completeness requirements that the product demands. This skill fills that gap by invoking the product-owner agent against the actual implementation.

## Prerequisites

PRODUCT.md must exist in the project root. If it does not exist, inform the user:

```
No PRODUCT.md found. Product acceptance requires a PRODUCT.md with product
requirements, strategic priorities, and product principles.

See docs/PRODUCT_OWNER_SETUP.md for setup instructions and templates/PRODUCT.md
for a starter template.
```

Do not proceed without PRODUCT.md.

## Input

<review_target> #$ARGUMENTS </review_target>

## Workflow

### Phase 1: Determine Review Target

Parse the argument to determine the review scope:

| Input | Review Type | How to gather changes |
|-------|-------------|----------------------|
| Numeric (e.g., `42`) | PR review | `gh pr view <number> --json files,title,body` |
| Branch name (e.g., `feat/auth`) | Branch review | `git diff --name-only main...<branch>` |
| Blank | Current changes | `git diff --name-only` + `git diff --cached --name-only` |

If the argument is a GitHub PR URL, extract the PR number and treat as PR review.

### Phase 2: Read PRODUCT.md and Gather Context

1. Read PRODUCT.md from the project root
2. Gather the list of changed files from Phase 1
3. Read the changed files to understand the implementation
4. If reviewing a PR, read the PR title and body for stated intent

### Phase 3: Invoke Product-Owner Review

Task compound-engineering:review:product-owner("Product acceptance review:

**Review Target:** <type and identifier from Phase 1>
**Stated Intent:** <PR title/body or branch name>
**Changed Files:** <list from Phase 1>

Read PRODUCT.md and evaluate this implementation against product requirements.

**Must Have (BLOCK if missing):**
- Accurate data — core data is correct and complete
- Functional core — primary use case works end-to-end
- No regressions — existing functionality preserved
- Meets stated requirements — delivers what was described

**Should Have (flag for backlog if missing):**
- Edge cases handled or explicitly documented as known gaps
- Clear error messages for user-facing failures
- Acceptable performance for the use case

**Evaluate against PRODUCT.md:**
1. Strategic alignment — which P0-P4 priority does this serve?
2. Completeness requirements — does the product demand full coverage here, or is partial acceptable?
3. User value delivery — does this solve the core job-to-be-done?
4. Product principles — does the implementation follow stated product principles?
5. Data quality — is data imported, processed, or presented accurately?

**Provide verdict:** APPROVE / APPROVE WITH CONDITIONS / BLOCK

For each verdict, include:
- Which PRODUCT.md priorities and principles informed the decision
- Specific evidence from the implementation
- For CONDITIONS: concrete items to address, classified as pre-merge or post-merge backlog
- For BLOCK: what must change before this can ship")

### Phase 4: Present Results

Present the product-owner's findings to the user with clear next actions:

- **APPROVE** — Ready to merge. Note which product priorities this serves.
- **APPROVE WITH CONDITIONS** — List items to address. Classify each as:
  - *Pre-merge*: must fix before merging
  - *Post-merge backlog*: create a tracked issue for follow-up
- **BLOCK** — List what must change. Explain why from a product perspective, referencing specific PRODUCT.md requirements.

## Relationship to Other Review Tools

- **ce:review** validates *how* the code is built (quality, patterns, security). Product-acceptance validates *what* was built (product alignment, user value).
- **product-lens-reviewer** (in document-review) challenges premises in planning documents from first principles. Product-acceptance validates implementations against *stated* product requirements.
- Use both for complete coverage — they produce different findings for the same PR.

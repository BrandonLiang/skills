---
name: code-review
description: Review a proposed code change or diff for concrete correctness, security, and requirement gaps. Use when asked to review a branch, pull request, commit, or uncommitted changes.
---

# Code Review

Review the change and report actionable defects. Do not edit files or apply fixes unless the user separately asks you to.

## 1. Establish the review scope

1. Identify the working tree state and the exact commits or files being reviewed. Include staged, unstaged, and untracked changes when the user asks for a working-tree review.
2. Use the comparison point the user names. If none is named, inspect the repository's default branch and recent history to identify a reasonable merge base. If the target is ambiguous, ask which base to use instead of silently reviewing the wrong range.
3. Confirm the selected reference resolves and that the resulting diff is non-empty. State any exclusions, such as generated files or unrelated local changes.
4. Read the relevant surrounding implementation, tests, and repository guidance. Look for an issue, acceptance criteria, or spec supplied with the request or discoverable in the repository. Do not invent requirements when no spec exists.

## 2. Review the change

Assess the diff from two complementary angles:

- **Behavior and requirements:** Does the change satisfy the stated request? Look for missing cases, incorrect behavior, unintended behavior, and scope that does not match the request.
- **Implementation risks:** Look for concrete bugs, security or privacy problems, data loss, unsafe input handling, race conditions, compatibility breaks, and failures in error paths. Consider maintainability only when the change creates a specific and material problem.

Follow repository documentation and established patterns where they apply. Treat a documented rule as a standard; distinguish a likely defect from a stylistic preference. Avoid duplicating findings that describe the same root cause. Do not flag issues that existing automated checks already enforce unless the change exposes a separate user-impacting failure.

For each candidate finding:

1. Trace the affected path through the actual code and verify the failure condition.
2. Check whether the issue is introduced by this diff and whether it can be tied to a changed line.
3. Describe a concrete triggering scenario and its impact. Include a practical direction for fixing it when clear.
4. Omit speculative, cosmetic, or low-impact concerns. If evidence is incomplete, say so and do not present a guess as a defect.

Do not modify code, create commits, or broaden the review into unrelated repository cleanup. Do not invoke this skill recursively or spawn additional reviewers.

## 3. Report findings

Put findings first, ordered by severity. Use this format:

```text
### Findings
- [P1] Short description — `path/to/file.ext:line`
  Explain the triggering scenario, user or system impact, and a concrete fix direction.
```

Use these severity levels consistently:

- **P0:** Critical, broadly exploitable or catastrophic; blocks release immediately.
- **P1:** High impact; likely to cause data loss, a security breach, or a major feature failure.
- **P2:** Moderate impact; a real defect in a narrower or less common scenario.
- **P3:** Low impact; a minor but concrete defect worth fixing.

Only include findings with a specific changed-line location. Use the narrowest useful line or range, and verify the location against the current diff. If there are no substantiated findings, say `No findings.` Do not invent a severity to make a report look complete.

After findings, briefly state:

- **Scope:** the base and head or working-tree state reviewed.
- **Requirements:** whether a spec was available and any unmet requirement, or that no spec was provided.
- **Validation:** checks already run or inspected and their result. Do not claim a check passed unless you observed it. Run tests only when requested or when the user's review request explicitly includes verification.

The review is advisory. Do not claim that CI, deployment, or production behavior has been verified unless evidence was provided.

## Design references

- [AI Hero's code-review skill](https://www.aihero.dev/skills-code-review) informed the separation between requirement fit and implementation quality.
- [A community comparison of code-review skills](https://www.reddit.com/r/AI_Agents/comments/1wj95gf/i_tested_five_code_review_skills_on_real_tasks_to/) reinforces treating review quality as a balance of useful coverage and false positives; its small sample is directional, not proof of a universal ranking.
- [Gitar](https://try.gitar.ai/) emphasizes actionable findings and verified fixes. This skill adopts the actionable-finding goal while leaving code changes and CI verification to the user or their normal workflow.

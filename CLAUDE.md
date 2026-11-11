# Global Rules

## PR / MR Workflow (mandatory, no exceptions)

Every code change must follow these steps in order:

1. **Feature branch** — never commit directly to main.
2. **Commit & push** to the feature branch.
3. **Open a PR/MR** (`glab mr create` or `gh pr create`).
4. **Review loop — up to 20 cycles** (a cap, not a target):
   - Run the code-reviewer agent on the diff, and tell it the cycle number ("cycle N of 20").
   - Post its findings as a comment on the MR.
   - Fix every blocking and high-priority finding, push the fixes, and re-run the code-reviewer agent.
   - Stop when the verdict is APPROVED WITH SUGGESTIONS with no blocking or high-priority findings left.
   - Medium and low suggestions are optional: fix the cheap ones in one last commit without another review cycle, and record the rest as declined, with a reason, in the MR comment.
   - Docs-only changes (Markdown, LICENSE, comments; no code, config or test changes) get one review cycle. Re-review only if it found a blocking or high-priority issue.
5. **Wait for a green pipeline** — poll CI until status is `success`.
   - If the pipeline fails, fix the cause, push, and go back to step 4.
6. **Merge** — only after BOTH review approval AND green pipeline.

**Never merge while a blocking or high-priority finding is open or the pipeline is not green.** Optional suggestions count as resolved once they are fixed or declined in the MR comment.

## General Rules

- Always use feature branches — never push directly to main.
- No PR without test coverage for new functionality.
- `glab ci *`, `glab mr *`, `glab api *` commands are pre-approved and do not need user confirmation.

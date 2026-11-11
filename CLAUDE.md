# Global Rules

## PR / MR Workflow (mandatory, no exceptions)

Every code change must follow these steps in order:

1. **Feature branch** — never commit directly to main.
2. **Commit & push** to the feature branch.
3. **Open a PR/MR** (`glab mr create` or `gh pr create`).
4. **Review loop — up to 20 cycles** (a cap, not a target; at cycle 20 the reviewer pauses and asks the user, see `agents/code-reviewer.md`):
   - Run the code-reviewer agent on the diff, and tell it the cycle number ("cycle N of 20").
   - Post its report as a comment on the MR.
   - Fix every Blocker and High Priority Issue, push the fixes, and re-run the code-reviewer agent.
   - Stop when the report shows 0 Blockers and 0 High Priority Issues (its verdict is then APPROVED WITH SUGGESTIONS). If the verdict and the counts disagree, the counts decide.
   - Medium Priority Suggestions are optional. In one last commit, without another review cycle, fix the cheap ones (no behavior change, no test change, about 10 lines or fewer), and decline the rest with a reason in the MR comment.
   - Docs-only changes get one review cycle. If it finds a Blocker or High Priority Issue, fix it and re-review; from then on the stop rule above applies. Docs-only means `README.md`, `docs/`, `LICENSE` or code comments only; changes to `CLAUDE.md`, `agents/`, `skills/`, `commands/`, `templates/`, `hooks/` or settings are not docs-only.
5. **Wait for a green pipeline** — poll CI until status is `success`.
   - If the pipeline fails, fix the cause and push. Go back to step 4, unless the fix changes no reviewed code (CI config or formatting only): then wait for the pipeline again.
6. **Merge** — only after BOTH a review report with 0 Blockers and 0 High Priority Issues AND a green pipeline.

**Never merge while a Blocker or High Priority Issue is open or the pipeline is not green.** Medium Priority Suggestions count as resolved once they are fixed or declined in the MR comment.

## General Rules

- Always use feature branches — never push directly to main.
- No PR without test coverage for new functionality.
- `glab ci *`, `glab mr *`, `glab api *` commands are pre-approved and do not need user confirmation.

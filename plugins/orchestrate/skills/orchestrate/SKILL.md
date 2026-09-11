---
name: orchestrate
description: Run one Linear ticket, or the next ready ticket under a parent, through a fresh agent; report back with per-repo commit lines. Usage — /orchestrate ENG-38 or /orchestrate ENG-37
disable-model-invocation: true
---

# orchestrate

Run tickets on the current feature branch, one at a time, one fresh agent per ticket. The user commits; nobody else does.

Needs the Linear MCP server and a workspace `CLAUDE.md` that maps ticket labels (or names in the ticket) to repo folders and gives the test commands.

## Steps

1. **Pick the ticket.** `get_issue` on `$ARGUMENTS` with `includeRelations`.
   - No sub-issues: this is the ticket.
   - Has sub-issues: it is a parent. List them (`list_issues` with `parentId`). Drop Done and Canceled. Drop any whose blockers are not Done. Drop any the parent marks blocked on an open decision or external approval. Of what is left, take the first in the parent's own order. Say which one and why before continuing. Then move the parent to In Progress in Linear if it is not already: `save_issue` on the parent with `state: "In Progress"`. Leave the parent open when the sub-issue finishes; the user closes it.
2. **Confirm the branch.** Map the ticket's repo labels to folders using the workspace `CLAUDE.md`. In each touched repo run `git rev-parse --abbrev-ref HEAD`; all must match. If any touched repo has working-tree changes other than `CLAUDE.md`, stop: the previous ticket is uncommitted.
3. **Dispatch one fresh `general-purpose` agent.** The prompt is only:
   - the ticket identifier (e.g. `ENG-45`)
   - the ticket body, verbatim
   - the paths of the `CLAUDE.md` files for the touched repos, plus the workspace root one
   - the branch name
   - the rules below, verbatim
4. **Relay the report** in the agent's format, then stop. The next ticket starts only when the user runs this skill again.

## Rules given to the agent

```
Before any other work, move your ticket to In Progress in Linear: `save_issue` on <ticket> with `state: "In Progress"`. If the Linear MCP server is unavailable, say so in Open questions and carry on. Leave the status at In Progress when you finish; the user closes tickets.
Work on branch <branch> in the repos named. Read each CLAUDE.md before touching code; it gives the test commands.
Leave all changes in the working tree. Git is read-only for you: no commit, no branch, no checkout, no push, no stash.
Test as the ticket's "Done when" describes.
Report back with exactly these sections:
- Files changed, grouped by repo
- How it was tested, with the commands run and their result
- Open questions
- Commit message per repo touched: one line, Conventional Commits, no trailers
```

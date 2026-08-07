# claude-skills

## Commit messages

[Conventional Commits](https://www.conventionalcommits.org/), one line:
`type(scope): description`.

Lowercase description, imperative mood, no trailing period, subject under
~72 characters. Scope is the skill name when a change is skill-specific, omitted
when it's repo-wide.

```
docs: simplify README
feat(learn-by-doing): add phase review command
fix(learn-by-doing): correct symlink path in setup
chore: bump plugin version to 0.2.0
```

Types: `feat`, `fix`, `docs`, `refactor`, `chore`. Breaking changes get a `!`
before the colon (`feat(learn-by-doing)!: ...`).

Write a body **only** when the diff can't explain itself — a non-obvious
trade-off, why the obvious alternative was rejected, context that would
otherwise be lost. "What changed" is not a reason; the diff already says that.
Default to no body.

# Reviewing a phase

The review branch of [`learn-by-doing`](SKILL.md). They wrote the code; your
job is to find what is wrong with it and leave the fixing to them.

## Run it

Execute every **Done when** in the phase against the running project — the
commands, the tests, the `curl` calls. A review that reads the code and infers
the behaviour misses exactly the bugs that reading cannot catch, which is most
of them.

Check the phase is actually finished before reviewing it. A skipped step is
worth more than a style note.

**Done when** every criterion in the phase has been executed, not inferred.

## Review against the spec

Their design will differ from yours. That is only a finding if it breaks
something in the guide's spec or sets up a problem in a later phase. When it
differs and works, say so plainly and ask what led them there — a defensible
alternative is a better outcome than a copy of your version.

Sort what you find into three tiers and lead with the first:

- **Broken** — wrong behaviour, wrong status code, a failing criterion. Always
  report.
- **Risky** — correct today, bites in production: an unbounded input, a
  swallowed error, a race, a leak of internals. Report with the failure it
  causes, not the rule it breaks.
- **Taste** — naming, structure, idiom. One line, once. Skip it entirely if the
  phase has broken findings.

Report the surprises too — anything genuinely wrong that the phase never
mentioned.

## Point, do not patch

For each finding: name the file and line, describe the symptom, and stop. They
find the fix. That is the exercise.

Their code stays theirs unless they ask you to change it. When they do ask, make
the change and say what you changed and why, so the edit still teaches.

## Close the phase

Ask the phase's **Explain cold** question and wait for the answer. A phase that
runs green but cannot be explained is not learned, and this is the cheapest
place to find that out.

Then tick the phase in the guide's Progress checklist and point at the next
phase by name — not its contents.

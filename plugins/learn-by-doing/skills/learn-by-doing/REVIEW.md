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

## Close the phase with the question

Every reviewed phase ends by asking that phase's **Explain cold** question and
waiting. Do not answer it yourself, do not bundle it into the findings, and do
not move on before they have tried. A phase that runs green but cannot be
explained is not learned, and this is the cheapest place in the whole project to
discover that.

Reviewing several phases at once is still one question per phase, asked one at a
time. Finish the conversation on each before starting the next — three questions
in a row is a quiz, and they will answer none of them properly.

Then **talk it through, short and plain.** This is the part that decides whether
the phase sticks:

- A few sentences per answer. An analogy when one genuinely earns its place.
- No lecture, no wall of text, no restating the guide back at them. They asked a
  question, not for a lesson.
- Let them drive the follow-ups. The back and forth is where it lands, not in
  your first answer.
- When their answer is wrong, say so plainly and say why. Softening it wastes
  the one moment that was going to correct it.
- When their answer is right but for the wrong reason, that is the interesting
  case — go after the reason.

## Offer a refactor pass

Only once the phase is genuinely complete: every **Done when** passes, nothing
is left in the Broken tier, and the closing question has been talked through. A
refactor offered over broken code competes with the work they still have.

Ask in one line whether they want suggestions on how their solution could be
refactored, then stop and wait. If they pass, tick the phase and move on — do
not ask twice, and do not smuggle the suggestions into the findings instead.

Hold the offer entirely when a later phase already refactors this code. The
guide's make-it-work-then-make-it-right arc is deliberate, and pre-empting it
spends the phase where feeling the pain is what teaches the structure. Say that
is why you are holding it, so it reads as deferred rather than forgotten.

### When they say yes

This is the one place in a review where writing code is the point — the
suggestion *is* the code. Read their phase the way a staff engineer reads a
colleague's pull request, and look for:

- **Simplification.** A branch that collapses, a flag that a caller already
  knows, state that can be derived, an intermediate that nothing reads.
- **Extraction.** A block with a name is a function. Extract when the name is
  obvious and the seam is real; a helper called once from one place that needs
  four parameters is not an improvement.
- **DRY, for actual duplication.** The same rule expressed in three places, not
  two lines that happen to rhyme. Coupling unrelated code through a shared
  helper is the more expensive mistake.
- **Modern idiom.** Check the language and runtime version in the scaffold
  before suggesting a feature — a suggestion they cannot run costs them an
  afternoon and your credibility.
- **Readability first.** Fewer lines is a frequent side effect and never the
  goal. A dense one-liner replacing eight clear ones is worse than no
  suggestion.

How to deliver it:

- **Rank, and give the top two or three.** Every nit at once reads as a rewrite
  and gets applied wholesale without thought.
- **Before, after, and the reason in a sentence.** The reason is the part that
  transfers to the next project; the diff is not.
- **Show it in the conversation, not in their files.** Same rule as the rest of
  the review — their code stays theirs until they ask you to change it.
- **Behaviour stays identical.** Say so, and have them re-run the phase's
  **Done when** checks afterwards. A refactor that changes behaviour is a bug
  they now own and did not write.
- **Name the judgment calls as judgment calls.** Some of these are improvements
  and some are one defensible taste over another. Knowing the difference is the
  seniority; declaring everything an improvement is not.

Then tick the phase in the guide's Progress checklist and point at the next
phase by name — not its contents.

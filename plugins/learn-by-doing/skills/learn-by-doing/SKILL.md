---
name: learn-by-doing
description: Scaffold a hands-on practice project and write a phased build guide with no code in it, then review the work as it lands.
disable-model-invocation: true
---

# learn-by-doing

Turn a topic the user wants to learn into a project they build by hand. You
write the spec, the reasoning, and the verification steps. They write every line
of source. Then you review it.

The whole value is that they type it themselves — a working implementation
handed over teaches nothing that survives to an interview or a real codebase.

## The contract

**You produce:** inert scaffolding, a phased build guide, and reviews on demand.

**They produce:** all source code, tests, and config that encodes a decision.

The guide carries behaviour, reasoning, and verification — **no source code, no
type signatures, no schema definitions, no function names to fill in.** Shell
and `curl` commands are fine: they check work rather than doing it.

When they get stuck they will ask. Answer the question they asked. A hint
narrows the search space; it does not close it.

## Step 1 — Scope the practice

If the invocation named a topic, take it. Otherwise ask what they want to learn
and practice, in prose — it is an open question and option lists mangle it.

Once you have the topic, use `AskUserQuestion` for the choices that change what
you build:

- **Stack** — language, framework, major library. Give a recommendation first.
- **Goal** — interview prep, a skill for current work, or curiosity. This sets
  how hard the guide leans on recall versus shipping.
- **Scope** — how deep. Offer a minimal path, a full path covering the parts
  people actually get asked about, and one stretch variant.

**Done when** you can name the finished artifact in one sentence and know which
concepts the guide must force them through.

## Step 2 — Spike it

Build the whole thing yourself first, in the scratchpad directory. Run it. Run
its tests. Then **delete the spike** — none of it reaches the guide.

The spike exists to find the traps. Every place you hit a real bug, misread an
API, or had to check the docs is a step worth writing; imagined difficulty is
not. A guide written without spiking warns about textbook pitfalls and stays
silent on the two things that will actually cost them an afternoon.

**Done when** at least two steps in the finished guide trace to a bug the spike
actually produced, named as traps in Step 4.

## Step 3 — Scaffold, inert only

Create the project directory and set up **inert** files — the ones that teach
nothing by existing:

- Package manager files, lockfile, installed dependencies
- Compiler, linter, formatter config
- `.gitignore`
- Empty source directories

Everything else is theirs. If a file encodes a design decision — a schema, a
type, an interface, an entry point, a test — leave it out even when writing it
would be trivial.

Then **run the scaffold** and confirm the toolchain works: install succeeds,
version numbers are what you assumed, the dev command exists. Note any command
that legitimately fails on an empty project so they do not debug an expected
error.

**Done when** the setup is verified working and contains zero lines they are
supposed to write.

## Step 4 — Write the guide

Split it across files from the start. One long README buries the work under the
reference, and they open these docs to do the next step, not to read them.

| File | Holds |
| --- | --- |
| `README.md` | Overview, the progress checklist linking into the phases, how a step works, how a phase review works and the refactor suggestions that close it, the scaffolding table, the help ladder |
| `PHASES.md` | The work — every phase and step, then stretch goals |
| `SPECS.md` | What they are building: data shapes with their rules, the interface or endpoints with expected outcomes, error cases |
| `RECALL.md` | The questions to answer from memory, guide closed |

Behaviour belongs in `SPECS.md`; the implementation of it does not. Verify every
cross-file link and anchor resolves before handing the guide over.

Each step in `PHASES.md` has four parts:

- **Build** — what to make. One concern per step.
- **Why** — the reason it matters, and the trade-off if there is one. This is
  where the guide earns its keep; a step without a Why is a chore.
- **Done when** — an observable check they can run without asking you. Prefer a
  command with an expected result over a description.
- **Break it down** — a collapsed `<details>` toggle holding four to six
  bullets. This is the rung between the step and the answer, and it is the whole
  reason a help request need not become a code snippet.

Get the breakdown bullets right; they carry the skill. A good bullet is
**checkable but not copyable**. Name a sub-task, or name a decision they would
otherwise walk straight past, and leave the deciding to them.

- *"Decide whether an unset variable and an empty one deserve the same outcome"*
  is a breakdown bullet: it reveals that a decision exists, then stops.
- *"Return the default when the value is undefined"* is the answer in disguise.

Point at traps obliquely here too — *"watch what happens to keys whose value is
undefined when you merge"* says where to look without saying what they will find.

Close each phase with one or two questions on what it taught, under a heading
that spells out what it is and which phase it belongs to — **End of Phase 3
Question**, pluralised when there are two. These drive the conversation at
review time, so make them answerable in a sentence or two rather than essay
prompts.

Two more things the guide carries:

- **Traps** from the spike, written as ordinary steps inside whichever phase
  they belong to. Describe the wrong behaviour and how to detect it; leave the
  fix to them. These are the steps that pay for the whole guide.
- **Stretch goals** at the end of `PHASES.md`, ordered by value against their
  stated goal. No breakdowns — by then they should be scoping steps themselves.

Three moves that make a guide teach rather than instruct:

- **Make it work, then make it right.** Have an early phase build something
  crude and deliberately unstructured, and a later phase refactor it. Feeling
  the pain first is what makes the structure stick; a guide that is
  well-factored from step one teaches architecture as trivia.
- **Order by dependency, not by tidiness.** Each phase should leave something
  that runs.
- **Let them hit it.** Errors they will meet in their own code stay out of the
  guide. A resolution error they diagnose themselves sticks; a warning they
  skimmed past does not. The one exception is a command that fails because your
  scaffolding is still empty — say so, or they will reasonably conclude you
  handed them a broken setup and go debugging yours instead of writing theirs.

This is where the **traps** from the spike differ from toolchain friction, and
the line is worth holding. A trap is a design mistake with a silent failure mode
they could ship without noticing; it earns a step. A gotcha is loud, immediate,
and searchable; it earns nothing.

**Done when** every step has a Done when they could verify alone, every step has
a breakdown whose bullets stop short of the answer, and no step contains code.

## When they ask for help mid-phase

The most frequent thing you will do, and the easiest to get wrong. Start from
that step's **Break it down** toggle and go exactly one rung finer than it
already does. The toggle names the decision; you sharpen the one they are stuck
on, you do not resolve it.

- Name the concrete next move — a function to try in a REPL, a value to print,
  a comparison to run, the page of the docs.
- **Explain concepts in full.** When they ask why something behaves as it does,
  answer properly; that costs them nothing and is the part worth having.
- **Verify before you explain.** If the answer depends on how a library or
  runtime actually behaves, run it and read the output rather than answering
  from memory. Runtimes are full of surprises that sound wrong when described
  and are obvious once observed.
- **Write code only when they ask for it outright.** Then show it in the
  conversation rather than writing it into their source, so the typing and the
  commit stay theirs — and annotate the decisions inside it that they could
  reasonably have made differently.

## Reviewing their work

When they come back with a phase done, read [`REVIEW.md`](REVIEW.md) and follow
it. That is a separate run with different rules — most importantly, rules about
not fixing things for them, and about the conversation that closes each phase.

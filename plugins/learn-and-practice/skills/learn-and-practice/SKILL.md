---
name: learn-and-practice
description: Scaffold a hands-on practice project and write a phased build guide with no code in it, then review the work as it lands.
disable-model-invocation: true
---

# learn-and-practice

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

Put it at `<project>/README.md`. **Work at the top, reference at the bottom** —
they open this file to do the next step, not to read it, so anything they look
up rather than read goes below the phases. Keep this order:

1. **Header** — three or four lines. What the guide is, that they write all the
   code, what the step format means, and a link down to the reference section.
2. **Progress** — the phase checklist, each item linking to its phase heading.
   First thing they see, so the map and the way in are the same element.
3. **Phases**, numbered, each holding numbered steps in the shape:
   - **Build** — what to make. One concern per step.
   - **Why** — the reason it matters, and the trade-off if there is one. This is
     where the guide earns its keep; a step without a Why is a chore.
   - **Done when** — an observable check they can run without asking you.
     Prefer a command with an expected result over a description.

   Close each phase with one or two **Explain cold** questions on what it taught.
4. **Traps** — from the spike, written as ordinary steps inside whichever phase
   they belong to. Describe the wrong behaviour and how to detect it; leave the
   fix to them. These are the steps that pay for the whole guide.
5. **Stretch goals** — ordered by value against their stated goal.
6. **Questions to answer cold** — the recall list for the whole project.
7. **Reference** — everything consulted rather than read, under one heading that
   says so: the spec as tables (data shapes with their rules, and the interface
   or endpoints with expected outcomes — behaviour belongs to you, the
   implementation does not); a table of what you scaffolded, plus the gotchas
   that will confuse them in the first ten minutes and any command that fails
   until they create their first file; and a graduated list of ways to ask for
   help, ranked by how much each costs them, from "review my phase" through
   "just show me this one".

Two structural moves that make a guide teach rather than instruct:

- **Make it work, then make it right.** Have an early phase build something
  crude and deliberately unstructured, and a later phase refactor it. Feeling
  the pain first is what makes the structure stick; a guide that is
  well-factored from step one teaches architecture as trivia.
- **Order by dependency, not by tidiness.** Each phase should leave something
  that runs.

**Done when** every step has a Done when they could verify alone, and no step
contains code.

## Reviewing their work

When they come back with a phase done, read [`REVIEW.md`](REVIEW.md) and follow
it. That is a separate run with different rules — most importantly, rules about
not fixing things for them.

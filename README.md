# claude-skills

Claude Code skills by [Eli Zevin](https://github.com/dizzyjaguar).

Each skill ships as its own plugin, so you install only the ones you want.

## Skills

| Skill | What it does |
| --- | --- |
| [`learn-and-practice`](./plugins/learn-and-practice) | Turn a topic you want to learn into a project you build by hand |

## Install

Add the marketplace once:

```
/plugin marketplace add dizzyjaguar/claude-skills
```

Then install whichever skills you want:

```
/plugin install learn-and-practice@dizzyjaguar
```

To update later, `/plugin marketplace update dizzyjaguar` then reinstall.

---

## `learn-and-practice`

A skill for learning by building, not by reading.

You give it a topic. It asks a few scoping questions, sets up **only the inert
parts** of a project — package manifests, compiler config, installed
dependencies, empty source folders — and writes a phased build guide with
**no code in it**. You write every line yourself. Then it reviews each phase as
you finish it.

```
/learn-and-practice postgres query optimization
/learn-and-practice                       # asks what you want to learn
/learn-and-practice review phase 3        # once you've built something
```

### Why no code

A working implementation handed over teaches nothing that survives to an
interview or an unfamiliar codebase. The guide carries behaviour, reasoning, and
verification commands; the implementation is yours. When you get stuck you ask,
and a hint narrows the search space rather than closing it.

### What makes the guides useful

Two things the skill forces that a guide written from memory misses:

- **It spikes first.** Claude builds the whole project itself in a scratchpad,
  hits the real bugs, then deletes it. The traps that make it into the guide are
  ones that actually happened, not textbook pitfalls. The skill holds itself to
  a bar here: at least two steps must trace to a bug the spike produced.
- **Make it work, then make it right.** An early phase builds something crude
  and unstructured; a later phase refactors it. Feeling the pain first is what
  makes the structure stick.

Every step carries a **Build** (what to make), a **Why** (the trade-off, the
reason an interviewer cares), and a **Done when** (a command you can run to
prove it works). Each phase closes with a question you should be able to answer
cold.

### During review

The skill runs your code rather than reading it, sorts findings into
broken / risky / taste, and **points without patching** — file, line, symptom,
then stops. You find the fix. It only edits your code if you ask.

---

## Developing locally

Skills are just Markdown, so the fastest loop is to symlink your checkout into
`~/.claude/skills/` and skip the plugin machinery entirely. Edits take effect on
the next Claude Code session — no version bumps, no reinstalling:

```sh
git clone https://github.com/dizzyjaguar/claude-skills.git
ln -s "$PWD/claude-skills/plugins/learn-and-practice/skills/learn-and-practice" \
      ~/.claude/skills/learn-and-practice
```

Do this *instead of* `/plugin install`, not alongside it — installing pins a
copy under `~/.claude/plugins/cache/`, and you would then have two versions
disagreeing about which one is live.

To publish a change: commit, bump `version` in the skill's
`.claude-plugin/plugin.json`, and push.

## Adding a skill to this repo

1. `plugins/<name>/skills/<name>/SKILL.md`
2. `plugins/<name>/.claude-plugin/plugin.json` with `name`, `description`, `version`
3. A new entry in `.claude-plugin/marketplace.json` with `"source": "./plugins/<name>"`

Skills invoked only by hand should set `disable-model-invocation: true` in their
frontmatter — the description then costs nothing in context, at the price of you
being the one who remembers the skill exists.

## License

MIT

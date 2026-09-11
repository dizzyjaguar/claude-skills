# 🌵 claude-skills

Claude Code skills by [Eli Zevin](https://www.linkedin.com/in/eli-zevin/)
([@dizzyjaguar](https://github.com/dizzyjaguar)). One plugin each, so you take
only what you want.

| Skill | |
| --- | --- |
| 🛠️ [`learn-by-doing`](./plugins/learn-by-doing) | Do you miss the ol' days when learning meant doin'? |
| 📖 [`review-phase`](./plugins/learn-by-doing/skills/review-phase) | Marks your homework. Ships inside `learn-by-doing` |
| 🎯 [`orchestrate`](./plugins/orchestrate) | One ticket, one fresh agent, you commit |

## Install

```
/plugin marketplace add dizzyjaguar/claude-skills
/plugin install learn-by-doing@dizzyjaguar
/plugin install orchestrate@dizzyjaguar
```

Updating: `/plugin marketplace update dizzyjaguar`, then reinstall.

## 🛠️ learn-by-doing

You name a topic. It sets up the boring parts — manifests, config, deps, empty
folders — and hands you a phased build guide with **zero code in it**. You write
every line. It reviews each phase when you're done.

```
/learn-by-doing postgres query optimization
/learn-by-doing                       # asks what you want to learn
```

Two things keep the guides honest:

- **It builds the thing first.** Claude spikes the whole project in a scratchpad,
  hits the real bugs, then throws it away. The traps in your guide are ones that
  actually happened.
- **Crude first, tidy later.** An early phase makes a mess; a later phase cleans
  it up. Feeling the pain is the point.

Every step has a **Build**, a **Why**, and a **Done when** you can actually run.
When you get stuck, hints narrow the search — they don't end it.

## 📖 review-phase

The other half. Tell it you've finished something and it takes over:

```
review my phase 3
check my 3.2          # one step works too
```

It runs that phase's **Done when** checks against your actual running code
rather than reading it and guessing, sorts what it finds into broken, risky and
taste, then points at the file and line and stops. You find the fix — that's the
exercise.

Then it asks you that phase's question, cold, and waits. Answering badly is the
useful part: the wrong first answer goes into `LESSONS.md` next to where it
eventually landed, because that's the bit worth rereading. After that you get a
refactor pass — how a senior engineer would tighten what you wrote, and why.

Auto-invokes, so you don't have to remember it exists. It ships inside the
`learn-by-doing` plugin; installing that gets you both.

## 🎯 orchestrate

Point it at a Linear ticket, or at a parent ticket, and it runs one ticket
through a fresh agent:

```
/orchestrate ENG-38      # this ticket
/orchestrate ENG-37      # the next ready ticket under this parent
```

A fresh agent per ticket keeps context small: the agent gets the ticket body,
the repos' `CLAUDE.md` files and the branch name, nothing else. Under a parent
it skips Done, Canceled and anything still blocked, and says which ticket it
picked and why.

The agent never touches git. It leaves the changes in the working tree and
reports files changed per repo, the test commands it ran, open questions, and
one Conventional Commits line per repo. You commit, then run it again for the
next ticket. It refuses to start if the last ticket is still uncommitted.

Needs the Linear MCP server, and a workspace `CLAUDE.md` that maps ticket labels
to repo folders and names the test commands.

## Hacking on these

Skip the plugin machinery — symlink your checkout and edits go live next session:

```sh
git clone https://github.com/dizzyjaguar/claude-skills.git
for s in learn-by-doing review-phase; do
  ln -s "$PWD/claude-skills/plugins/learn-by-doing/skills/$s" ~/.claude/skills/$s
done
ln -s "$PWD/claude-skills/plugins/orchestrate/skills/orchestrate" ~/.claude/skills/orchestrate
```

One symlink per skill, not per plugin — a plugin holding two skills needs both.

Do this *instead of* `/plugin install` — running both leaves two copies arguing
over which one is live.

To ship: commit, bump `version` in the plugin's `.claude-plugin/plugin.json`, push.

**Adding a skill?** If it stands alone, it's a plugin of its own — three files:

1. `plugins/<name>/skills/<name>/SKILL.md`
2. `plugins/<name>/.claude-plugin/plugin.json` — `name`, `description`, `version`
3. an entry in `.claude-plugin/marketplace.json` pointing at `./plugins/<name>`

If it's useless without an existing one — the way `review-phase` needs the guide
that `learn-by-doing` writes — drop it in beside that plugin's skill as
`plugins/<plugin>/skills/<name>/SKILL.md` and bump the version. No second
manifest, and no way to install half of a pair that only works whole.

## License

MIT

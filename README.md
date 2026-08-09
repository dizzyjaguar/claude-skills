# 🌵 claude-skills

Claude Code skills by [Eli Zevin](https://www.linkedin.com/in/eli-zevin/)
([@dizzyjaguar](https://github.com/dizzyjaguar)). One plugin each, so you take
only what you want.

| Skill | |
| --- | --- |
| [`learn-by-doing`](./plugins/learn-by-doing) | Do you miss the ol' days when learning meant doin'? |

## Install

```
/plugin marketplace add dizzyjaguar/claude-skills
/plugin install learn-by-doing@dizzyjaguar
```

Updating: `/plugin marketplace update dizzyjaguar`, then reinstall.

## 🌵 learn-by-doing

You name a topic. It sets up the boring parts — manifests, config, deps, empty
folders — and hands you a phased build guide with **zero code in it**. You write
every line. It reviews each phase when you're done.

```
/learn-by-doing postgres query optimization
/learn-by-doing                       # asks what you want to learn
/learn-by-doing review phase 3        # once you've built something
```

Two things keep the guides honest:

- **It builds the thing first.** Claude spikes the whole project in a scratchpad,
  hits the real bugs, then throws it away. The traps in your guide are ones that
  actually happened.
- **Crude first, tidy later.** An early phase makes a mess; a later phase cleans
  it up. Feeling the pain is the point.

Every step has a **Build**, a **Why**, and a **Done when** you can actually run.
When you get stuck, hints narrow the search — they don't end it. Reviews point at
the problem and stop there. You find the fix. Once a phase is green and you've
explained it back, you get a refactor pass — how a senior engineer would tighten
what you wrote, and why — and the whole exchange lands in `LESSONS.md` to reread
before the next session.

## Hacking on these

Skip the plugin machinery — symlink your checkout and edits go live next session:

```sh
git clone https://github.com/dizzyjaguar/claude-skills.git
ln -s "$PWD/claude-skills/plugins/learn-by-doing/skills/learn-by-doing" \
      ~/.claude/skills/learn-by-doing
```

Do this *instead of* `/plugin install` — running both leaves two copies arguing
over which one is live.

To ship: commit, bump `version` in the skill's `.claude-plugin/plugin.json`, push.

**Adding a skill?** Three files:

1. `plugins/<name>/skills/<name>/SKILL.md`
2. `plugins/<name>/.claude-plugin/plugin.json` — `name`, `description`, `version`
3. an entry in `.claude-plugin/marketplace.json` pointing at `./plugins/<name>`

## License

MIT

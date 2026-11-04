# How I build

How I take a game or app from an idea to the App Store and Google Play with AI agents: the steps, the skills and tools I use at each one, and the templates I work from.

I'm [Alex Gofman](https://github.com/alexgofman), a Unity and mobile developer in London. I've shipped 15 products since 2016 (670K+ downloads, featured by Apple twice), including two live games, [Enclaver](https://apps.apple.com/us/app/enclaver-life-simulator-sim/id1493799162) and [Northeld](https://apps.apple.com/app/id1525862814). Agents do most of the typing. I own the architecture, the review and the release.

## The flow

**Idea → Brainstorm → Spec → Plan → Build → Review → Test and ship → Watch and learn**, then back to Brainstorm for the next feature.

| Step | What happens | Skills and tools | What it leaves behind |
|---|---|---|---|
| **1. Brainstorm** | Questions until the goal, the player and the "must not happen" list are clear. Big decisions go to a council of five AI advisors who review each other's answers. | [superpowers](https://github.com/obra/superpowers) `brainstorming`, `llm-council`, `deep-research` | A short brief |
| **2. Spec** | Why, what, non-goals, anti-goals, constraints and failure cases. A fresh agent that sees only the spec lists every place it would have to guess, and those gaps get closed. | `brainstorming` (written design plus builder check) | `docs/spec.md`, approved before any code |
| **3. Plan** | Small tasks. Each one names its files, its failing test and its "done when". | `writing-plans` | `docs/plan.md` |
| **4. Build** | A fresh agent per task, test first. Agents read the Unity Editor (console, scene, components) through MCP. Wide jobs, like localisation, fan out to parallel agents. | Claude Code, `subagent-driven-development`, `dispatching-parallel-agents`, [MCP for Unity](https://github.com/CoplayDev/unity-mcp), [Blender MCP](https://github.com/ahujasid/mcp-for-blender), Higgsfield for art, `frontend-design` for web | One commit per task |
| **5. Review** | A separate agent checks every task twice: first whether it matches the spec, then whether the code is good. A bug gets a root cause before a fix. Nothing is "done" without evidence. | The two-stage review in `subagent-driven-development`, `code-review`, `systematic-debugging`, `verification-before-completion` | Review notes and fixes |
| **6. Test and ship** | Logic that doesn't need the engine goes into plain C# with tests that run outside Unity, so CI checks every push in under a minute. Then device builds, the [release checklist](templates/release-checklist.md) and the store submission. | GitHub Actions, NUnit, Xcode, the App Store Connect and Google Play APIs, AppTweak MCP for store keywords | A green build and a store release |
| **7. Watch and learn** | Scheduled agents read store data every morning and report to Telegram. Every session ends with a journal entry that records its decisions. | launchd jobs calling the Claude API, Telegram, Obsidian | A daily digest and the next brief |

## Rules the agents work under

Every project starts with a `CLAUDE.md` rulebook before the first line of code ([template](templates/CLAUDE.md)). The core of mine:

- **Smallest correct change.** Diffs, not rewrites, and no drive-by refactors.
- **Cause before fix.** Diagnose, explain, wait for a yes, then patch.
- **Ask before architecture.** The save format, data schemas, scene structure and project setup need an explicit yes.
- **No speculative abstractions.** An interface earns its place once it has two real users.
- **Mobile first.** No allocations in `Update`, pooled spawns, and effects measured on a mid-range Android phone.
- **Nothing swallowed.** No silenced exceptions, disabled warnings or commented-out failing code.
- **Unity MCP: reading is free, writing needs a yes.** Scenes and prefabs change through Unity's API, never as text.
- **Evidence before "done".** Test output, CI, a clean console or a device check.
- **Secrets stay out** of prompts, logs and repos.

## Memory and models

- **Obsidian is the shared memory.** It holds an agent guide, a rules file per project, decisions and a daily session journal. Each session starts by reading it and ends by writing to it.
- **Models:** Claude Fable 5.1 to plan and to pull results together, Claude Opus 5.5 for parallel agent work, and the Claude API inside products.

## Where this flow has shipped

| Project | What the flow did |
|---|---|
| [Northeld: Idle Castle RPG](https://apps.apple.com/app/id1525862814) | From a blank project to the App Store and Google Play in about three months, solo, with 16 languages at launch. Claude Code worked under a project rulebook (now about 800 lines) with the Unity Editor connected through MCP. |
| Localisation of two live games | About 1,560 agents worked behind automated QA gates that check placeholders, length limits and missing keys. 16 languages are now live in both games, and 100 store listings (39 App Store + 61 Google Play locales) were written and pushed through the store APIs. |
| Store-analytics agents | The spec and plans were written with superpowers, and 60+ automated tests back them. Two agents run in production: every morning they pull App Store Connect and Firebase data, apply anomaly rules and send a Claude-written digest to Telegram. |
| Game art pipeline | AI-generated icons are chroma-keyed and turned into Unity assets through MCP, with no manual imports. |

## Templates

| File | What it is |
|---|---|
| [`templates/CLAUDE.md`](templates/CLAUDE.md) | Project rules for a Unity mobile game |
| [`templates/spec.md`](templates/spec.md) | The spec I write before any code |
| [`templates/plan.md`](templates/plan.md) | Tasks with tests first |
| [`templates/release-checklist.md`](templates/release-checklist.md) | An iOS and Android release, from the build to 48 hours after it |

Want this flow on your project? [alexgofman.me](https://alexgofman.me) · [LinkedIn](https://www.linkedin.com/in/alexeygofman)

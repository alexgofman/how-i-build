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

## Where I've used this flow

| Project | Type | What the flow did |
|---|---|---|
| [Northeld: Idle Castle RPG](https://apps.apple.com/app/id1525862814) | Game | From a blank project to the App Store and Google Play in about three months, solo, with 16 languages at launch. Claude Code worked under a project rulebook (now about 800 lines) with the Unity Editor connected through MCP. |
| [Dog to Wag](#app-example-dog-to-wag) | App | An AI coach for owners of reactive dogs, built with React Native. It went from the first spec to a build on an iPhone in three days. [Details below.](#app-example-dog-to-wag) |
| Localisation of two live games | Games | About 1,560 agents worked behind automated QA gates that check placeholders, length limits and missing keys. 16 languages are now live in both games, and 100 store listings (39 App Store + 61 Google Play locales) were written and pushed through the store APIs. |
| Store-analytics agents | Tooling | The spec and plans were written with superpowers, and 60+ automated tests back them. Two agents run in production: every morning they pull App Store Connect and Firebase data, apply anomaly rules and send a Claude-written digest to Telegram. |
| Game art pipeline | Games | AI-generated icons are chroma-keyed and turned into Unity assets through MCP, with no manual imports. |

## App example: Dog to Wag

Dog to Wag is a subscription app, not a game, that coaches owners of reactive dogs. It went through the same flow, with a React Native stack instead of Unity. The first spec was written on 24 September 2026, and a build was installed on an iPhone on 26 September.

**Stack:** Expo SDK 57 · React Native 0.86 · TypeScript (strict) · Expo Router · on-device SQLite · Reanimated · RevenueCat for subscriptions · Supabase (anonymous auth and an Edge Function) · Claude API · Sentry · PostHog · Jest and Testing Library · GitHub Actions

| Step | What happened |
|---|---|
| **Brainstorm** | I ranked 29 problem niches, and a coach for reactive dogs came out on top. Every API, price and store policy the app depends on got its own research note, each with a section that checks its facts independently. |
| **Spec** | Three rounds of adversarial review took the spec to version 3.1. Its core rule: a fixed, deterministic safety engine decides each training step, and the AI only explains. The AI can never override the engine. |
| **Plan** | The first master plan had 7 phases and 64 tasks, tracked in Jira with sprints and story points. I then cut it to a lean v1, because speed to launch mattered more. |
| **Design** | A design system and key-screen mockups in Claude Design. The mascot and the app icon are based on my own dog. |
| **Build** | Subagent-driven development with tests first: 54 commits in two days. A multi-agent workflow built the interface. It built the app logic and UI kit first, then five screen agents built the screens, and a final pass wired everything together. |
| **Review** | Review agents checked the engine from several angles. Every gap they found was settled on the cautious side and logged for my approval, 26 changes in all. |
| **Test** | 904 tests, plus a property fuzzer that simulated about 1.7 million dog-days without breaking a single safety rule. CI is green on GitHub Actions. |
| **Result** | The app had onboarding, a paywall, a daily plan with streaks, a walk mode and an AI-written weekly check-in (Claude, through a Supabase function, with a template fallback). It ran on a real iPhone. I stopped it before launch: the risk of injuries (dog bites, dogs near traffic) was a liability I chose not to carry. The code is kept as a reusable app shell. |

## Templates

| File | What it is |
|---|---|
| [`templates/CLAUDE.md`](templates/CLAUDE.md) | Project rules for a Unity mobile game |
| [`templates/spec.md`](templates/spec.md) | The spec I write before any code |
| [`templates/plan.md`](templates/plan.md) | Tasks with tests first |
| [`templates/release-checklist.md`](templates/release-checklist.md) | An iOS and Android release, from the build to 48 hours after it |

Want this flow on your project? [alexgofman.me](https://alexgofman.me) · [LinkedIn](https://www.linkedin.com/in/alexeygofman)

# <Game name>: project rules for Claude Code

Read this before every task. These rules beat general habits.

## Who you're working with

A senior Unity and C# engineer who has shipped several mobile titles. Calibrate to that.

## Operating principles

- **Surgical over sweeping.** Make the smallest correct change and name the exact files and lines. No drive-by refactors.
- **Diffs, not rewrites.** Don't replace a whole file when a five-line patch will do.
- **Cause before change.** For a bug, explain the diagnosis first and wait for a yes before patching.
- **Ask before architectural moves.** That covers the save format, ScriptableObject schemas, namespaces, scene structure and contracts between systems.
- **No speculative generality.** Add an interface, event or abstraction once it has two real users, not before.
- **Mobile first.** Weigh every change against GC allocations, draw calls, `Update()` cost and IL2CPP.
- **Shippable beats elegant.** Flag a structural problem as a suggestion; don't fix it unasked.
- **No silent suppression.** No swallowed exceptions, disabled warnings or commented-out failing code.
- **Don't invent Unity APIs.** Copy a pattern that already exists in the project. If none exists, ask.

## Off-limits without a written yes

- Project setup: Player Settings, packages, build configuration, render pipeline, signing.
- Editing `.unity`, `.prefab` or `.meta` files as text. Use Unity MCP, or give me the steps.
- Deleting files, mass renames, or moves across folders.
- Renaming serialized fields without `[FormerlySerializedAs]`.
- Changing the save format without a versioned migration.

## Project facts

- **Unity:** <version> and <render pipeline>, for iOS and Android.
- **Architecture:** <e.g. MVC with state machines, single scene, prefabs spawned at runtime>.
- **Data:** <e.g. authored data in ScriptableObjects and JSON; runtime saves in ...>.
- **Layout:** `<Assets/_GAME/Scripts/<Feature>/>`.

## Tests

- Logic that doesn't need the engine (economy, timers, combat maths, save migrations) lives in plain C# with NUnit tests that run outside Unity.
- Run them with `dotnet test <path>`. CI runs the same command on every push.

## Unity MCP

- **Reads are free:** hierarchy, components, console, profiler.
- **Writes need my yes** for each call.
- **Bugs:** read the console errors before guessing at a cause.
- **Performance:** get real frame data before changing anything.

## Performance (mobile)

- No allocations in `Update`, `FixedUpdate` or `LateUpdate`.
- Cache components in `Awake`. No `Find*` calls at runtime.
- Pool anything spawned every frame or every tick.
- Measure any new full-screen effect on a mid-range Android phone.

## How to deliver a change

1. A one-line summary.
2. The files and line ranges affected.
3. The change itself, as a diff.
4. How to verify it: a scene, a menu action or a test.
5. Risks and follow-ups.
6. Suggestions, if any, in their own section at the end.

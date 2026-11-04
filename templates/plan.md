# <Feature>: implementation plan

- **Spec:** `docs/spec.md`, approved on <date>
- **Rules:**
  - One task means one commit.
  - Every task starts with a failing test.
  - No task touches files outside its list.

## Task 1: <name>

- **Files:** `path/to/Feature.cs`, `tests/FeatureTests.cs`
- **Test first:** <the failing test, in one line>
- **Build:** <what to change>
- **Done when:** the new test passes, the whole suite is green, and <any manual check> is done.
- **Review:** first against the spec, then for code quality.

## Task 2: <name>

...

## Final check

- The full test suite and CI are green.
- Device builds work on iOS and Android.
- Re-read the spec: every item in "What it does" maps to a task.

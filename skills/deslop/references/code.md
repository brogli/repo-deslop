# Code patterns: source files and comments

## 1. Facts

- Calls to methods, config keys, annotations or CLI flags that do not exist in
  the used library version.
- Version numbers in docs or build files that do not match the lockfile.
- Comments or docs claiming behavior the code does not have.
- Names that lie: `validateUser` that also saves, `enableCache` that does
  something else, a class name that no longer fits what it does.

Fix: verify against the actual dependency source or docs. Renaming changes
code; treat it like any executable change.

## 2. Unspecified features

Code that implements things nobody asked for: extra options, flags, config
keys, fallbacks, retries, caching, alternative code paths, "for future use"
extension points, generic parameters with one caller.

Detection:
- Traceability test against the spec.
- Signals: comments like "for future use", "can be easily extended", "in case we
  need"; config keys read nowhere or set nowhere; parameters always passed the
  same value; enum values never used; interfaces with one implementation.

Fix: never delete silently. List each unspecified feature with its location and
what depends on it, then ask. Some were wanted but not written down. Once
confirmed, remove the feature and its tests, config and docs together.

## 3. Abstraction and structure (stepdown rule)

One level of abstraction per function. The top-level function should read
like a summary: a short list of calls at the same level, so the reader can stop
there or step down into one of them.

Detection:
- Functions mixing levels: business decisions next to string parsing, byte
  handling, or HTTP details.
- Top-level entry points (main, controller methods, handlers) that are long
  and detailed instead of a sequence of named steps.
- Helper functions defined far above the code that calls them, or in random
  order. Stepdown: caller first, callees below, in call order.
- Deep nesting where early returns or an extracted function would give each
  level a name.

Fix: extract and name steps; order callers above callees. Keep extractions
that name a real concept; do not split into one-line wrappers just to shorten
functions (that is its own slop).

## 4. Over-engineering and defensive noise

Common forms:
- Interfaces, factories, strategies, builders, wrappers with one
  implementation and no stated need for more.
- Config for values that never vary.
- Null checks, try/catch, and validation where the value cannot be null or the
  error cannot happen (check the types and callers first).
- Swallowed exceptions: catch, log, continue, when failure should propagate.
- Duplication: re-implementing helpers that exist in the codebase or standard
  library.
- Ignoring local conventions: different naming, error handling, logging, or
  libraries than the rest of the project.
- Dead code: unused parameters, unreachable branches, leftover alternative
  implementations, commented-out code.

Fix: the simplest version that does the stated job, following the codebase's
conventions. When de-slopping, don't replace one abstraction with another.

## 5. Unnecessary detail

Detail no reader or caller needs:
- Log statements at every step; debug output left at info level.
- Error messages and exceptions carrying internal state nobody acts on.
- Public parameters and config exposing internals that have one sensible value.
- Doc comments listing every parameter of a self-explanatory method.
- README or doc sections documenting internal classes and private helpers.

Fix: consumer test. Before removing logs or changing error messages, check
that nothing parses them.

## 6. Comments

A comment should say what the code cannot: why, constraints, non-obvious
consequences, links to specs or bugs. It describes the code as it is now.

**Changelog comments.** Comments narrating history: "Updated to use...",
"Now returns...", "Previously this...", "Changed from X to Y", "Fixed bug",
"As requested", "Refactored for clarity". History belongs in git.
Fix: delete. If the comment hides a real why ("uses X because Y broke under
load"), rewrite it as present-tense rationale.

**Restating the code.** `// increment counter` over `counter++`.
Fix: delete.

**Step narration.** "// Step 1: validate input", "# 2. Save to DB".
Often a sign the function should be split into named steps (see section 3).
Fix: extract functions with those names, or delete the numbering.

**Trivial doc comments.** Javadoc/JSDoc/docstrings on getters, setters and
self-explanatory methods: "Gets the name."
Fix: delete. Keep doc comments on public APIs where they add contract
information (units, nullability, thrown exceptions, thread safety).

**Banners and decoration.** `// ===== Section =====`, emoji in comments.
Fix: delete. If a file needs section banners, it may need splitting.

**Prose slop in comments and docstrings.** Same patterns as prose.md:
"ensuring robust handling", "seamlessly integrates", "It's important to note".

**Comment density.** A file where comment lines approach code lines is a
signal. Read it; usually most comments fail the deletion test.

**Stale comments.** Comments that no longer match the code (wrong facts).

## 7. Test code

- Tests that mock everything and assert only that mocks were called.
- Assertions that cannot fail (`assertNotNull(new Foo())`).
- Tests bent to pass: expected values copied from actual output, skipped or
  loosened assertions.
- Test names and docstrings that describe more than the test checks.

Fix: report; propose the real assertion. Do not delete tests without approval.

---
name: deslop
argument-hint: "[--fix] [scope]"
description: Find and remove AI slop (verbose, hollow, over-detailed, or over-engineered output from earlier LLMs) in a repository or a selected slice of it, so humans and LLMs can understand the code and docs again. Covers prose (README, docs, Markdown, CLAUDE.md/AGENTS.md, commit and PR text) and code (comments, docstrings, unspecified features, over-engineering, stepdown/abstraction-level violations). Use whenever the user asks to de-slop, clean up AI-written text or code, remove LLM verbosity, audit a repo for AI slop, tighten docs or comments written by Claude or another model, or make a codebase readable again, even if they don't say "slop".
---

# deslop

Find and remove AI slop in a repo or a slice of it, without changing meaning or
behavior.

Slop is text or code that adds volume without adding value, produced without
anyone checking whether it is true, needed, or at the right level. AI use alone
does not make something slop; missing judgment does. Good text and code let the
reader stop at any level and still be right.

## Workflow

### 1. Scope

Establish:
- **Slice**: whole repo, directories, files, or a diff range
  (`git diff --name-only <base>...HEAD`).
- **Mode**: report only. If the arguments contain `--fix`, report and then fix
  right away. Without `--fix`, don't edit files, even when the request says
  "fix" or "clean up"; the user can ask for fixes after reading the report.
- **Spec** for the traceability test: issue, PR description, task prompt, or
  design doc. If none exists, use README, public API docs, and call sites as a
  proxy, and say so in the report.

Ask only what you cannot find in the repo.

### 2. Understand the repo first

Before judging any file, read the README, agent instruction files
(CLAUDE.md, AGENTS.md), build files, the directory tree and the entry points.
Know what the repo is for and what its conventions are (naming, error
handling, logging, tests). You judge every file against this.

### 3. Work in slices

Small scope: read every file. Large repo: go module by module (package,
feature folder, bounded context); docs are their own slice, judged against the
code. A quick grep for chat residue or changelog comments can point you at
files, but read the files regardless.

In Claude Code, run one subagent per module, give it the repo summary from
step 2 and point it at this skill, so each context stays small. If you can pick
the subagent model: Opus for judgment-heavy slices (docs, public APIs, core
domain logic), Sonnet for large slices of similar files. Keep step 2, the
cross-module pass, the report and all rewrites in the main session.

While going through slices, note cross-module problems for the report: two
modules doing the same thing, layers with one user, docs describing code that
does not exist, modules nothing calls.

### 4. Read and judge

Before judging prose (docs, Markdown, agent instruction files, commit/PR
text), read `references/prose.md`. Before judging source files, read
`references/code.md`. They list known patterns with a fix for each.

Read each file fully and apply the Tests. Every finding names the test it
fails; a matching pattern alone is not a finding. Judge in this priority order:
1. **Wrong facts**: hallucinated APIs/flags/steps, stale comments, docs that
   contradict code or each other, names that don't match behavior, swallowed
   exceptions.
2. **Over-engineering**: unspecified features, abstractions and config with
   one user, defensive code for impossible cases, docs with more structure
   than content.
3. **Level problems**: hollow passages, detail at the wrong level, buried
   points, stepdown violations, mixed-level paragraphs and functions.
4. **Unnecessary detail**: true, on-topic, needed by no reader.
5. **Filler**: changelog comments, restated code, editorializing, empty
   conclusions, chat residue.
6. **Tone and structure**: inflated significance, promotional adjectives,
   manufactured contrast, participle tails, reflexive triads, formatting noise.
7. **Vocabulary**: last, and only where the sentence is otherwise fine.

Classify each finding with one action:
- **delete**: fails the deletion or consumer test.
- **move**: needed by a reader you can name, wrong place; say where it goes.
  If you can't name the reader, it's a delete.
- **shorten**: same claim, fewer words.
- **rewrite**: the claim is recoverable but badly expressed.
- **ask author**: hollow with no recoverable meaning, unspecified feature,
  forced neutrality, or you are unsure.
- **simplify**: over-engineered; replace with the simplest version that does
  the same job.
- **keep**: false positive; the pattern is used well. Record briefly.

### 5. Report

Write the report before editing: a 2–3 sentence summary; "Needs author
decision" first; then findings per file as `L<n> [action] category: problem.
fix`; cross-module and recurring patterns; keeps and false positives,
briefly. The report must pass the same checks.

### 6. Fix (with `--fix`, or when the user asks after the report)

Follow the Rewrite rules below. Work in small, reviewable commits grouped by
category (e.g. "remove changelog comments", "restructure README top-down").
Commit messages: subject says what, body says why, no file lists. Don't commit
files that had uncommitted changes before you started; leave your edits to
them unstaged and say so.

After editing, reread each rewritten passage with the summary test.

## Core ideas

**Truth.** Everything in the repo is a claim: prose, comments, names, test
names, commit messages. Each must match the code and the other claims. A false
claim is worse than any style problem, because readers act on it.

**Hollowness.** Hard human text is compressed meaning, so effort to decode it
pays off. Slop is the shape of an argument with no thought behind it, so effort
finds nothing, and readers (human or LLM) learn to skim everything.

**Levels.** Most slop is a level failure: too abstract with nothing underneath
("dreamy" claims that commit to nothing), too detailed with no summary to stop
at, or conclusion and implementation detail mixed in one paragraph or function.

The stepdown rule (code: one level per function, callers above callees), the
inverted pyramid/BLUF (prose: most important first) and Minto's Pyramid
Principle are the same rule.

**Unnecessary detail.** Detail no reader needs at any level: obvious facts,
exhaustive enumerations, incidental versions, restated context, caveats for
impossible cases. Every detail costs human and LLM attention, and on-topic but
irrelevant detail distracts most. An appendix of unneeded detail is still
slop.

**Over-engineering.** Building for requirements nobody stated: features,
options, layers, config, fallbacks, templates and document structure "for
later" or "for flexibility". Every reader has to get through it, and each part
is a place for bugs and drift. The target is the simplest version that does the
stated job, in code and in prose.

## Tests

1. **Summary**: restate it in one sentence. Empty or trivial result: hollow.
2. **Deletion**: remove it. Nothing lost: it goes.
3. **Second read**: worse on rereading means hollow; good text gets better.
4. **Top level**: read only the first sentence of each section, or only the
   top-level function body. If you don't know what this is and does, the
   structure is wrong regardless of wording.
5. **Traceability** (code): trace each feature, option and branch to the spec.
   No source: unspecified.
6. **Consumer** (details): who needs it, and would they act differently or
   misunderstand without it?
7. **Simplicity**: is there a simpler version that does the same job for the
   same reader? Then this one is over-engineered.
8. **Verification**: check each factual claim against the code, config and the
   other docs. A contradiction or a misleading name is a wrong fact.
9. **Measurement** (adjectives): what measurable fact is behind "robust",
   "fast", "seamless"? None: cut the word or state the fact.

Also judge on coherence (does each sentence follow from the last) and
relevance (does it serve this document's purpose); expert slop judgments track
these two (Shaib et al., 2025).

## Patterns

The patterns in `references/prose.md` and `references/code.md` describe
2023–2025 models; newer models have tics no list covers. Every pattern also
occurs in good human writing. One sign proves little; several in one place are
a reason to read closely, never a reason to edit. The tests decide.

## Rewrite rules

The fixer is an LLM, and LLM editing changes intended meaning and pushes text
toward neutral positions (Abdulhai et al., 2026). These rules prevent that.

1. **Subtract first.** Delete, move, shorten, simplify, and only then rewrite.
   Deleting a hollow sentence cannot add a false claim; rewriting can.
2. **Never invent meaning.** If the meaning cannot be recovered from the code
   or surrounding text, delete, or mark it:
   `<!-- deslop: unclear what this means; author please state the claim or remove -->`
   An invented claim that sounds right is worse than the original slop.
3. **Claims in = claims out.** For each rewrite, list the original's claims and
   the rewrite's claims. Every original claim is kept, moved, or deliberately
   dropped (say which in the report). The rewrite adds no claim that was not in
   the original or verified in the code.
4. **Keep the author's position.** Do not soften a recommendation into "it
   depends".
5. **Keep the author's voice.** Informal, terse, or odd human phrasing is not
   slop. Change only what fails a test.
6. **Don't overcorrect.** Repetition that clarifies, a contrast that corrects a
   real misconception, a list of parallel items, an em dash in a good sentence:
   all stay.
7. **Don't write new slop or over-engineer the fix.** No new tools, scripts,
   files, abstractions or structure to fix slop; a fix that adds more than it
   removes needs a reason. You have tics no list covers: if you rewrote many
   passages, show the user a before/after sample before continuing. With
   `--fix`, don't stop; put the sample in the final summary.
8. **Preserve behavior.** Comment and doc edits need no tests (rules 2–4 still
   apply). Executable changes need passing tests before and after, or approval.
   Feature removal always needs approval.
9. **Don't rewrite git history.** Report slop in past commits; suggest a
   template.
10. **Uncertain means ask.** List it under "Needs author decision".

## Sources

`references/sources.md` lists where each rule comes from. Read it only when the
user asks.

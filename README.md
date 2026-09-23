# repo-deslop

A Claude Code plugin that finds AI slop in a repository or part of one, and
removes it if you ask. Slop here means false claims, over-engineering, detail at
the wrong level, detail no reader needs, and text that sounds meaningful but
says nothing. It checks prose (README, docs, CLAUDE.md, AGENTS.md, commit and PR
text) and code (comments, docstrings, unrequested features, needless
abstractions). It is for anyone maintaining a repo that LLMs wrote much of, who
wants its code and docs readable again.

## Install

```
/plugin marketplace add brogli/repo-deslop
/plugin install repo-deslop@repo-deslop
```

## Usage

Ask in plain words, for example:

```
Deslop this repo.
Find the AI slop in docs/.
Deslop the files changed in main...HEAD.
```

or run the skill directly with `/repo-deslop:deslop`, optionally followed by a
scope.

By default it only reports. To have it fix what it finds right away, add
`--fix`:

```
/repo-deslop:deslop --fix docs/
```

Without `--fix` you can still ask for fixes after reading the report. Fixes are
committed, grouped by kind of fix; files you had already changed are left
uncommitted.

## How it works

The plugin is one skill and its reference files: no scripts and no
pattern-matching tool. The skill tells Claude to read the files in scope and
judge them against its checks; a phrase that matches a known slop pattern is not
a finding by itself.

[sources.md](skills/deslop/references/sources.md) lists where the rules come
from.

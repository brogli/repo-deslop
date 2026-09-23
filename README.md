# repo-deslop

A Claude Code plugin that finds AI slop in a repository or part of one, and
removes it if you ask. It is for anyone maintaining a repo that LLMs wrote much
of, who wants its code and docs readable again.

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

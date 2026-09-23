# Prose patterns: docs, READMEs, Markdown, commit messages, PR text

## 1. Meaning and level problems

**Invented or contradicting facts.** Hallucinated flags, endpoints, config
keys, version numbers, benchmark numbers, setup steps never tested; two docs
(or two sections) that say different things.
Fix: verify against the code. If the code doesn't settle a contradiction, ask
the author which is true.

**Hollow passages.** Sounds deep, commits to nothing. Abstract nouns
("landscape", "journey", "ecosystem", "paradigm"), stacked metaphors, rhythm
without claims. Fails the summary test.
Fix: if a concrete claim is recoverable from context (code, surrounding text),
state it plainly. If not, delete or flag for the author.

**Too much detail at the wrong level.** Tendency to write at the highest level
of detail everywhere: config flags in the intro, every edge case in the
overview, implementation steps in a "What is this" section.
Fix: move it to where its reader looks (later section, reference page, code
comment, link). If no reader needs it, it is unnecessary detail (next item).

**Unnecessary detail.** True, on-topic, and needed by nobody: incidental
version numbers, exhaustive lists where two examples make the point, every
option when one is recommended, background the reader already has, caveats
for cases that cannot happen, restating what the previous section said.
Fix: delete (consumer test).

**Over-structured documents.** More structure than content: headings for
two-sentence sections, templates filled with "N/A" or generic text, "Overview",
"Features", "Contributing" sections that say nothing specific, frameworks and
matrices for a simple decision.
Fix: remove the structure that has no content; merge short sections.

**Buried point.** The first sentence of a document or section is meta
("This section describes...", "Welcome to...") or a 40-word setup.
Fix: first sentence states the point. Inverted pyramid / BLUF.

**Mixed levels in one paragraph.** Conclusion, rationale, and implementation
detail in one block.
Fix: one level per paragraph. Summary paragraph first, detail paragraphs after.

**Empty conclusion.** "In summary", "Overall", "Ultimately", "By following these
steps you can..." restating what was just said. Shreya Shankar: empty summary
sentences feel conclusive but say nothing.
Fix: delete, or replace with something new (a caveat, a next step, a link).

**Editorializing.** "It's important to note", "It's worth mentioning", "Needless
to say". Tells the reader what matters instead of saying it.
Fix: delete the frame, keep the content.

**Vague attribution.** "Many developers prefer", "Experts agree", "Studies show".
Fix: name the source or delete the claim.

**Forced neutrality.** Every option presented as equally valid, no
recommendation, when the author clearly had one.
Fix: if the original author's position is known (code, commit, issue), state
it. Otherwise flag; do not pick a side for them.

## 2. Tone

**Inflated significance** (Wikipedia: undue emphasis on importance): "stands as
a testament", "plays a pivotal role", "enduring legacy", "game-changer",
"in today's fast-paced world", "ever-evolving landscape".
Fix: state what the thing does. Cut the significance claim.

**Promotional adjectives**: "robust", "seamless", "powerful", "blazing fast",
"production-ready", "enterprise-grade", "elegant", "intuitive".
Fix: measurement test. Replace with the fact ("handles 10k req/s on one core")
or cut.

**Stacked hedges**: "may potentially", "can help ensure".
Fix: say what it does, or say plainly that it is uncertain and why.

## 3. Sentence structure

The first four items below were documented for 2023–2025 models. The problems
behind them (manufactured contrast, fake analysis, reflexive structure, staged
emphasis) may come back in new wording; judge the problem, not the words.

**Negative parallelism**: "It's not just X, it's Y", "less about X and more
about Y", "not merely X, but Y". Corrects a misconception nobody had.
Fix: state Y. Keep the contrast only if X is a real, common misconception.

**Participle tails** (Wikipedia: superficial analysis): ", highlighting...",
", ensuring...", ", reflecting...", ", underscoring...", ", showcasing...".
Adds fake analysis to a plain statement.
Fix: cut the tail, or make its claim a separate sentence if it is real.

**Rule of three**: triads by reflex ("fast, reliable, and secure").
Fix: keep the items that are true and relevant. Two or four is fine.

**Dramatic reveal / staged pause**: "The result: ...", "Here's the thing: ...",
"The catch?", colon or semicolon pauses where "and" or "but" would do, short
fragment sentences for emphasis. Reported as a Claude-era tic (aggregator
sources only, not verified).
Fix: plain sentence.

**Uniform rhythm**: every sentence the same length and shape. Shankar notes this
makes text harder to follow.
Fix: only address this after meaning and level problems are fixed.

## 4. Vocabulary (weakest signal)

Words that became far more frequent with 2023–2024 models (Kobak et al.):
"delve", "underscore", "showcasing", "meticulously", "intricate", "pivotal".
Newer models may use other words. "load-bearing" is reported for Claude
(unverified).

Fix: replace with the plain word only when the sentence is otherwise fine.

## 5. Formatting

- Bold on every other phrase; bold-lead bullets ("- **Fast:** ...") for things
  that are really a paragraph.
- Headers for a three-sentence document.
- Nested bullets for connected reasoning. Shankar: lists help when items are
  parallel and independent; connected ideas need a paragraph.
- Emoji in headings, bullets, commit subjects.
- Tables for two or three items.
- Em dash density far above the rest of the repo.

Fix: remove formatting that does not help scanning. Convert connected bullet
lists to prose. Keep lists for genuinely parallel items (steps, options).

## 6. Residue from chat

Text written for a chat window that ended up in a file: "Great question",
"You're absolutely right", "I hope this helps", "Here's the updated version",
"Let me know if you need anything else", "As an AI", knowledge-cutoff
disclaimers.
Fix: delete (unless the file is about chat output, e.g. a test fixture).

## 7. Document types

**README.** First paragraph: what it is and who it is for, in plain words.
Then how to run it. Detail (config reference, architecture) further down or in
separate files. Common slop: feature lists of adjectives, badges and emoji
before any content, setup steps that do not work.

**Agent instruction files** (CLAUDE.md, AGENTS.md, .cursorrules, skill files,
prompts). Read by LLMs on every run, so density matters more here than
anywhere. Common slop: generic advice the model already follows ("write clean
code", "follow best practices"), restated rules, motivational framing,
contradictory instructions added over time.
Fix: keep only instructions specific to this repo that change behavior. One
rule, one place.

**Commit messages.** Subject says what changed at the top level. Body says why.
Common slop: bullet list of every file touched, "Updated X, Updated Y",
emoji prefixes, adjectives ("improved robustness"), summaries of the diff the
diff already shows.

**PR descriptions and changelogs.** Same as commits: why, user-visible effect,
risk. Not a narrated diff.

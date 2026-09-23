# Sources

Items marked (unverified) come only from secondary reporting.

## Pattern catalogs and measurements

- Wikipedia, "Signs of AI writing", maintained by WikiProject AI Cleanup.
  Field guide built from thousands of flagged edits since 2023. Source of:
  inflated significance, participle-tail analysis, negative parallelism, rule
  of three, editorializing, vague attribution, formatting tells, and the
  co-occurrence principle.
  https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing
  https://en.wikipedia.org/wiki/Wikipedia:WikiProject_AI_Cleanup/Guide_and_resources

- Kobak, González-Márquez, Horvát, Lause (2025). "Delving into LLM-assisted
  writing in biomedical publications through excess vocabulary." Science
  Advances 11(27). 15M+ PubMed abstracts 2010–2024; at least 13.5% of 2024
  abstracts processed with LLMs. Source of the vocabulary examples.
  https://www.science.org/doi/10.1126/sciadv.adt3813
  https://github.com/berenslab/llm-excess-vocab

- Paech, Roush, Goldfeder, Shwartz-Ziv (2025). "Antislop: A Comprehensive
  Framework for Identifying and Eliminating Repetitive Patterns in Language
  Models." Some patterns >1,000x more frequent in LLM output than human text.
  Their word/phrase lists are fiction-heavy; useful if a repo contains
  narrative text.
  https://arxiv.org/abs/2510.15061
  https://github.com/sam-paech/antislop-sampler (Apache-2.0)

## Definitions and dimensions

- Shaib, Chakrabarty, Garcia-Olano, Wallace (2025). "Measuring AI 'Slop' in
  Text." Taxonomy from expert interviews: density, relevance, factuality, bias,
  structure, coherence, tone. Binary slop judgments are somewhat subjective but
  correlate with coherence and relevance.
  https://arxiv.org/abs/2509.19163

- Arvind Narayanan (AI Snake Oil), note on hollowness: the real sign is
  polished writing around mundane ideas; less impressive on second read.
  https://substack.com/@aisnakeoil/note/c-223855800

- Harry G. Frankfurt, "On Bullshit" (essay 1986, book 2005). Speech produced
  with indifference to truth.

- Hicks, Humphries, Slater (2024). "ChatGPT is bullshit." Ethics and
  Information Technology.
  https://link.springer.com/article/10.1007/s10676-024-09775-5

## Rewriting risks

- Abdulhai, White, Wan, Qureshi, Leibo, Kleiman-Weiner, Jaques (2026). "How
  LLMs Distort Our Written Language." LLM editing changes intended meaning, not
  just tone; heavy use led to ~70% more essays staying neutral on the question.
  https://arxiv.org/abs/2603.18161

- Shreya Shankar (2025), "Writing in the Age of LLMs." Red flags (empty summary
  sentences, overused nested bullets, uniform sentence length) and a warning
  against overcorrecting.
  https://www.sh-reya.com/blog/ai-writing/

## Attention and context

- Hong, Troynikov, Huber (2025). "Context Rot: How Increasing Input Tokens
  Impacts LLM Performance." Chroma technical report. 18 models; performance
  degrades as input grows, even on simple tasks; related distractors hurt.
  https://research.trychroma.com/context-rot
- Nelson Cowan (2001). "The magical number 4 in short-term memory."
  Behavioral and Brain Sciences 24(1). Working memory holds about 3–5 chunks.
- John Sweller, cognitive load theory (from 1988): extraneous load is effort
  spent on material that does not serve understanding.

## Structure and abstraction

- Robert C. Martin (2008), *Clean Code*, ch. 3 "Functions": one level of
  abstraction per function; the stepdown rule.
- Barbara Minto (1987), *The Pyramid Principle*.
- Inverted pyramid (news writing) and BLUF, "bottom line up front" (US military
  writing guidance).

## Claude-specific tells (unverified)

- Reports summarizing Hacker News and r/ClaudeAI threads describe Claude-era
  tics: staged "counterfactual, pause, fact" constructions, colon/semicolon
  pauses where "and"/"but" would do, sentence fragments for emphasis, the word
  "load-bearing", "You're absolutely right!". Only aggregator sources were
  found (e.g. explainx.ai, Aug 2026). Treat as weak signals.

## From the user who commissioned this skill

- Slop changes as LLMs evolve, so pattern matching must not carry the skill;
  the model reads and judges. No data persisted on the user's machine.
  Subagents may run on Opus or Sonnet.
- Hollowness asymmetry: effort spent decoding hard human text pays off;
  effort spent on extreme slop does not, because there is no meaning to find.
- Over-detail: writing at the highest level of detail instead of an
  appropriate higher level.
- Unnecessary detail: details nobody needs rot the human reader's context,
  the same way long input rots an LLM's.
- Over-engineering is AI slop too: in code, in docs, and in tooling built
  around the task, including this skill.
- Code: unspecified features; comments that turn into a changelog; ignoring
  the stepdown rule so the reader cannot opt out of detail (applies to prose
  too).

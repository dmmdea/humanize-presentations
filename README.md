# humanize-presentations

**Remove AI tells from slide decks without destroying the ones that were deliberate.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/type-agent%20skill-8A2BE2.svg)](https://skills.sh/)
[![Works with](https://img.shields.io/badge/works%20with-Claude%20Code%20%C2%B7%20Cursor%20%C2%B7%20Codex-lightgrey.svg)](#install)

A skill for AI coding agents that de-slops an existing `.pptx`. It handles what prose humanizers miss: deck-specific tells, the difference between slide text and speaker notes, non-English decks, and the file mechanics that make a repaired deck actually open.

---

## Why this exists

Prose humanizers are excellent at prose. Point one at a slide deck and it does real damage, because its rhythm and voice passes tell you to add sentence fragments, first person, and lived detail. That advice is correct for a speaker's notes and wrong for a slide.

They also miss the tells that actually mark a deck as machine-made. A word list built for essays has nothing to say about topic-label titles, "Thank You / Questions?" slides, or a chart titled `Revenue by Quarter` instead of what the revenue did.

But the harder problem is the opposite one. **Most deck "tells" are also legitimate design choices.** A deck can be repetitive because a rubric demanded it, dense because nobody is going to present it, and titled by topic because the section names mirror an assignment. A checklist run without judgment flags all of it, produces thirty findings, and the three that mattered get lost.

This skill exists to tell those two situations apart.

---

## The core idea: two independent passes

Findings come in two classes, and conflating them is the failure mode this skill is built around.

| | Sentence-level tells | Structural patterns |
|---|---|---|
| **Examples** | `§` and decorative glyphs, em dashes, participle pile-ons, inflation, hedging, AI vocabulary | Title style, repetition, slide shape, text density, speaker notes |
| **When they apply** | **Always.** No rubric or format makes these acceptable | Only after the deck's contract permits it |
| **Gated?** | Never | Yes |

The trap: you establish that a deck's structure is legitimate, and conclude the deck is clean. **A perfectly structured deck can still be full of bot patterns.** Both passes run, every time, independently.

### The contract gate (structure only)

Before flagging anything structural, answer two questions from evidence in the file:

**Is it read alone, or presented live?**

- *Read alone* (leave-behind, submitted deliverable, board pre-read): dense slide text is **correct**, and empty speaker notes are **correct**. Do not thin the text. Do not ask for notes.
- *Presented live*: slide text is support, detail belongs in notes, and missing notes is a real finding.

**Is its structure imposed or chosen?**

- *Imposed* by a rubric, template, or house format: repetition, fixed section names, and identical slide shapes are compliance, not slop.
- *Chosen* by the author: the structural tells apply in full.

When the contract makes a pattern correct, it is not reported at all. Not softened, not as "consider". A list padded with wrong flags gets the right ones ignored.

> **Where this rule came from.** The skill was tested against a real teaching deck: 21 slides, every title a topic label, rigid triads throughout, eight identical slide shapes repeated twice, and 21 empty notes pages. A naive checklist produces roughly thirty findings. Nearly all of them are wrong. The titles mirrored the assignment's required steps, the repeated structure *was* the teaching payload, and the deck stated outright that it had to be understandable without anyone presenting it. Meanwhile the genuine defects, 13 em dashes and a set of participle pile-ons, were sitting untouched in the prose.

---

## Sentence-level tells

Counted, never estimated from impression.

| Tell | Fix |
|---|---|
| **Missing substance** | The strongest signal there is. No concrete detail, no real names, no specific numbers, nothing checkable. Generic competence is the actual defect; everything below is a symptom |
| **Model artifacts** (`oaicite`, `[cite: 1]`, `contentReference`, stray `**` or `##`) | Conclusive when present. Delete |
| **`§` and decorative glyphs** (`¶ ※ ⁂ ❖ ◆ ★ ▪ ►`, `// 01`, `PROTOCOL / 002`) | Delete. `§` is a statutory section sign; on a non-legal deck a single one is conclusive. Keep a glyph only when it is a consistent separator system, not a pseudo-label |
| Em dash | Remove as a **style choice**, not as evidence. Density proves nothing (see below). What still carries signal is the mid-sentence aside `X — the thing that explains X`, especially nested |
| Participle tacked on for fake consequence (`-ing`, or `-ando/-iendo` in Spanish) | Delete, or promote to its own sentence. Keep it only when it states an actual method |
| Significance inflation (*key, pivotal, fundamental, strategic*) | Delete the adjective, or replace it with the number that earns it |
| Hedging and vague attribution (*experts say, it is estimated*) | Name the source or cut the claim |
| Negative parallelism (*not only... but also*) | Once per deck maximum |
| Copula avoidance (*constitutes, represents, positions itself as*) | *is* |

### Re-scan your own fixes

**A uniform pattern replaced by another uniform pattern is not a fix.**

Removing ten `-ing` clauses by opening ten sentences with "and" trades one tell for a worse one, because the replacements cluster exactly where you were working. For N instances of one tell, expect roughly N different repairs: split into two sentences, collapse into a list, recast as a conditional, promote the consequence to the main clause, or leave it when the construction is genuinely working.

This rule exists because the skill's author made this exact mistake during testing, and the user caught it.

### Non-English decks

English word lists do not transfer. **The patterns do.** Map them rather than translating the list: `-ing` pile-on becomes `-ando/-iendo`; *delve/leverage/robust* becomes *abordar/aprovechar/robusto/integral*; *it's worth noting* becomes *cabe destacar/es importante señalar*. Judge structure and function, never vocabulary.

---

## What the 2026 evidence says

**Do not build a verdict on one marker.** Humans identify AI text at roughly **57% accuracy**, barely above chance, and every individual punctuation marker measures weak. Slop is a convergence of signals.

The em dash in particular is finished as a tell. Measured per 1,000 words:

| Source | Rate |
|---|---|
| Human baseline | 3.23 |
| Twain, *Huckleberry Finn* | **10.13** |
| GPT-4.1 | **10.62** |
| Claude Opus, prose-constrained | 0.19 |
| Gemini 2.5 Pro, prose-constrained | 0.00 |
| Llama 3.x | 0.00 |

Twain and GPT-4.1 are indistinguishable on this metric, and newer models suppress dashes deliberately. Removing them is a defensible style choice. Presenting their presence as evidence is not.

**Why they show up at all:** em dashes are markdown leaking into prose. Models trained on markdown-heavy corpora internalize the dash as a structural boundary, so when told to drop formatting the headers and bullets go while the dash survives, because it is already prose-legal. The same leakage produces boldface runs, inline-header lists, and a reflex toward bullets where a sentence belongs.

---

## Structural tells

Applied only once the contract clears them.

| Tell | Fix |
|---|---|
| **Generator template fingerprint**: the deck looks like a filled Gamma, Beautiful.ai, Pitch, or Canva AI template | **The dominant deck tell of 2026.** Reviewers have seen thousands and it fires before anyone reads a word. Real assets, real brand tokens, break the uniform section rhythm |
| Generic AI stock imagery: abstract gradients, glowing circuitry, anonymous smiling teams | Real photos, real screenshots, real data, or nothing |
| Copy auto-shrunk to fit its box | Cut the copy, do not shrink the type |
| Topic-label titles (*Market Overview*) | An action title stating the insight |
| Chart title names the chart, not the finding | State what the data shows |
| Everything in threes, third item padding | Cut to two, or let counts differ |
| Bullets written as full sentences with periods | Fragments, one idea each |
| Filler slides: Agenda echoing section titles, *Key Takeaways*, *Thank You / Questions?* | Delete, and put the ask on the closing slide |
| Bullet wall where a table or one number belongs | Restructure |
| Icon-in-circle + bold title + two-line description, repeated | Break the grid |
| Emoji in headers, purple gradients | Remove |
| Vague verbs in any title (*improve, enhance, optimize, leverage, streamline*) | Quantify or cut |

---

## Two channels

Slide text and speaker notes are opposites, and the most common way to damage a deck is to treat them alike.

- **Slide text**: every sentence-level tell applies. Never add rhythm variation, first person, opinions, or conversational texture. Slide text is not prose, and this holds for read-alone decks too, where density is the point.
- **Speaker notes**: the sentence-level tells, plus varied rhythm and a spoken register. Notes are read aloud, so they should sound like a person talking.

---

## File handling

The skill delegates all `.pptx` mechanics to a PowerPoint skill rather than hand-rolling zip parsing, and encodes four steps that exist because each one failed in testing:

1. **Validate before reading.** A broken file outranks its prose.
2. **Check the container separately.** Standard validators do not check OPC part ordering, so a deck can pass validation and still make PowerPoint offer to Repair it.
3. **Read** with a text extractor, writing to a UTF-8 file rather than a console.
4. **Write to a copy**, with a hit count per replacement rule, correct repackaging, re-validation, and a before/after count of the tell being removed.

### Failure modes

Every row was hit for real during development.

| Symptom | Actual cause | Fix |
|---|---|---|
| `'charmap' codec can't decode byte` on N slides | Validator reading XML as cp1252 on Windows | `PYTHONUTF8=1`. The deck is fine |
| Validator passes, PowerPoint still offers Repair | `[Content_Types].xml` is not the first zip entry | Repackage with it first |
| Accents render as `?` or a replacement glyph | cp1252 stdout | Write UTF-8 to a file, read the file |
| Your edited deck now needs repair | Naive rezip wrote directory entries | Repackage without them |
| A replacement rule reports 0 hits | The span crosses two text runs | Match a shorter span inside one run |
| A deck full of tells declared clean | Sentence-level pass never ran | Both passes are mandatory |

> **The OPC ordering one is worth knowing even if you never use this skill.** A `.pptx` whose zip has `[Content_Types].xml` anywhere but first will be rejected by PowerPoint while passing every validator, opening fine in python-pptx, and rendering correctly in LibreOffice. It is produced by naively rezipping an unpacked deck. The fix is repackaging with the content-types stream written first and no directory entries.

---

## Install

```bash
npx skills add dmmdea/humanize-presentations
```

Or clone into your agent's skills directory:

```bash
git clone https://github.com/dmmdea/humanize-presentations.git ~/.claude/skills/humanize-presentations
```

`~/.agents/skills/` works as a cross-runtime location for Claude Code, Codex, Cursor, Copilot CLI, and others.

## Use

The skill triggers on natural phrasing. You do not need to name it.

```
de-slop this deck
this presentation sounds like AI wrote it
review the text in deck.pptx before I send it
```

It reports findings ranked by damage, and on request produces an edited **copy**, never touching your original.

---

## What it does not do

| Job | Owner |
|---|---|
| Prose, articles, documents | A general prose humanizer |
| Visual and layout QA | A design review skill |
| Building a deck from scratch | A deck-building skill |
| The zip and XML mechanics | A PowerPoint skill, which this one requires |

This skill changes words. Not packaging, not design.

---

## Credits

The sentence-level pattern vocabulary is informed by [jpeggdev/humanize-writing](https://github.com/jpeggdev/humanize-writing) (MIT) and, upstream of that, Wikipedia's [Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) maintained by WikiProject AI Cleanup.

**This skill is self-contained and installs nothing else.** The two are complementary: use a prose humanizer on prose, and this one on decks.

## License

MIT. See [LICENSE](LICENSE).

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

But the harder problem is the opposite one. **Most deck "tells" are also legitimate design choices.** A deck can follow a fixed section cycle because a rubric demanded it, and carry topic titles because the section names mirror an assignment. A checklist run without judgment flags those alongside the real tells, produces thirty findings, and the three that mattered get lost. The opposite failure is worse: a gate wide enough to excuse the rubric also excuses the template, and passes a deck with the section name in the eyebrow and the footer of every slide.

This skill exists to tell those two situations apart.

---

## The core idea: two independent passes

Findings come in two classes, and conflating them is the failure mode this skill is built around.

| | Sentence-level tells | Structural patterns |
|---|---|---|
| **Examples** | Chrome that repeats itself, word count, presenter narration on slides, `§` and decorative glyphs, em dashes, participle pile-ons, inflation, hedging, AI vocabulary | Which sections exist, what they are called, whether a required cycle is followed |
| **When they apply** | **Always.** No rubric or format makes these acceptable | Only after the deck's contract permits it |
| **Gated?** | Never | Yes |

The trap: you establish that a deck's structure is legitimate, and conclude the deck is clean. **A perfectly structured deck can still be full of bot patterns.** Both passes run, every time, independently.

### The contract gate (structure only)

Before flagging anything structural, answer two questions from evidence in the file:

**The contract can excuse exactly three things: which sections exist, what they are called, and a required cycle across parallel cases.** The rubric imposes content. The template imposes chrome. Only the rubric earns protection. A brief that requires a section named `Strategies (Step 2)` protects that title appearing once. It does not protect an eyebrow above it, a footer echoing it, badges beside its items, a callout label on every card, or 130 words on the slide.

**Is it read alone, or presented live?**

- *Read alone* (leave-behind, submitted deliverable, board pre-read): empty speaker notes are correct, and more slides or more evidence is acceptable. It does not license more words per idea, presenter narration on the slide, or any chrome.
- *Presented live*: slide text is support, detail belongs in notes, and missing notes is a real finding.

**Is the section structure imposed or chosen?**

- *Imposed* by a rubric or brief: the required sections, their names, and a required cycle across parallel cases are compliance. Nothing else is.
- *Chosen* by the author: nothing is cleared. Either way, only those three rows are ever gateable; every other tell applies to every deck.

When the contract makes a pattern correct, it is not reported at all. Not softened, not as "consider". But check that the pattern is one of the three the contract can actually cover before clearing it.

> **Where this rule came from, including the part that was wrong.** The skill was tested against a real teaching deck: 21 slides, every title a rubric step, rigid triads, eight slide shapes repeated across two parallel cases, empty notes. The first version of the gate cleared all of it as rubric compliance. That was half right. The section names and the repeated cycle were the assignment's. But the same deck carried the section label in an eyebrow *and* a footer on 17 of 21 slides, three chrome bars on every slide, 131 words per slide on average, and 15 lines of presenter narration typed onto slides. None of that is in any rubric. The gate had been protecting the template, and the user caught it from one screenshot. The gate is now narrow: which sections exist, what they are called, and a required cycle across parallel cases. Nothing else.

---

## Sentence-level tells

Counted, never estimated from impression.

| Tell | Fix |
|---|---|
| **Missing substance** | The strongest signal there is. No concrete detail, no real names, no specific numbers, nothing checkable. Generic competence is the actual defect; everything below is a symptom |
| **Model artifacts** (`oaicite`, `[cite: 1]`, `contentReference`, stray `**` or `##`) | Conclusive when present. Delete |
| **Chrome that repeats itself**: section label in an eyebrow *and* a footer on the same slide, badges, callout labels on every card | Never gated. A section label appears once per slide, as the title. Everything else carrying it goes |
| **Word count**: over ~80 words a slide, every line must justify itself; over ~120 it is a document page turned sideways | Cut, do not shrink the type. Read-alone earns more content, never more words per idea |
| **Presenter's voice written down**: `How to read this:`, `This attacks the cost of...`, `Note: figures are illustrative` | Notes if presented, folded or cut if read alone |
| **`§` and decorative glyphs** (`¶ ※ ⁂ ❖ ◆ ★ ▪ ►`, `// 01`, `PROTOCOL / 002`) | Delete as chrome. `§` is a statutory section sign and a strong prompt to look harder, never proof on its own; `§` followed by a real statute or clause number is a citation and stays. Keep a glyph only when it is a consistent separator system, not a pseudo-label |
| Em dash | Remove as a **style choice**, not as evidence. Density proves nothing (see below). What still carries signal is the mid-sentence aside `X — the thing that explains X`, especially nested |
| Participle tacked on for fake consequence (`-ing`, or `-ando/-iendo` in Spanish) | Delete, or promote to its own sentence. Keep it only when it states an actual method |
| Significance inflation (*key, pivotal, fundamental, strategic*) | Delete the adjective, or replace it with the number that earns it |
| Hedging and vague attribution (*experts say, it is estimated*) | Name the source or cut the claim |
| Negative parallelism (*not only... but also*) | A tic when dense relative to length; recast |
| Copula avoidance (*constitutes, represents, positions itself as*) | *is* |

### Re-scan your own fixes

**A uniform pattern replaced by another uniform pattern is not a fix.**

Removing ten `-ing` clauses by opening ten sentences with "and" trades one tell for a worse one, because the replacements cluster exactly where you were working. For N instances of one tell, expect roughly N different repairs: split into two sentences, collapse into a list, recast as a conditional, promote the consequence to the main clause, or leave it when the construction is genuinely working.

This rule exists because the skill's author made this exact mistake during testing, and the user caught it.

### Non-English decks

English word lists do not transfer. **The patterns do.** Map them rather than translating the list: `-ing` pile-on becomes `-ando/-iendo`. Spanish markers that actually fingerprint generated text: *profundizar en, en un mundo cada vez más, en el ámbito de, es crucial destacar, cabe destacar, potenciar, holístico, sinergia, robusto*. `abordar` and `integral` are ordinary business Spanish; do not flag them on sight. Judge structure and function, never vocabulary.

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

Only three of these are ever gateable: which sections exist, what they are called, and a required cycle across parallel cases. Everything else in this table applies to every deck.

| Tell | Fix |
|---|---|
| **Generator template fingerprint**: the deck looks like a filled Gamma, Beautiful.ai, Pitch, or Canva AI template | Requires rendered slides to judge. Reviewers have seen thousands of template decks and recognize one before reading a word. Real assets, real brand tokens, break the uniform section rhythm |
| Generic AI stock imagery: abstract gradients, glowing circuitry, anonymous smiling teams | Real photos, real screenshots, real data, or nothing |
| Copy auto-shrunk to fit its box | Cut the copy, do not shrink the type |
| Chart title names the chart, not the finding | State what the data shows |
| Everything in threes, third item padding | Let the counts differ; never convert every triad to a pair |
| Bullets written as full sentences with periods | Fragments, one idea each |
| Filler slides: Agenda echoing section titles, *Key Takeaways*, *Thank You / Questions?* | Delete, and put the ask on the closing slide |
| Bullet wall where a table or one number belongs | Restructure |
| Icon-in-circle + bold title + two-line description, repeated | Break the grid |
| Emoji in headers | Remove |
| Topic-label or vague-verb titles (*Market Overview*, *Improving intake*) | A title that states the insight. Exception: a rubric-imposed section name is protected, vague verb or not |

---

## Two channels

Slide text and speaker notes are opposites, and the most common way to damage a deck is to treat them alike.

- **Slide text**: every sentence-level tell applies. Never add rhythm variation, first person, opinions, or conversational texture. Slide text is not prose, and this holds for read-alone decks too: they earn more slides, never more words on a slide.
- **Speaker notes**: the sentence-level tells, plus varied rhythm and a spoken register. Notes are read aloud, so they should sound like a person talking.

---

## File handling

Validation is delegated to a PowerPoint/pptx skill; reading uses a `python-pptx` shape walk (band, placeholder flag, per-slide word counts); container repair and Tier 1 text edits run directly against the OPC zip. Seven steps (A validate, B container check, C lossless repair, D shape walk, E Tier 1 write, F Tier 2 write, G verify), each of which exists because it failed in testing:

1. **Validate before reading.** A broken file outranks its prose.
2. **Check the container separately.** Standard validators do not check OPC part ordering, so a deck can pass validation and still make PowerPoint offer to Repair it.
3. **Read** with a `python-pptx` shape walk (band, placeholder flag, per-slide words), written to a UTF-8 file rather than a console.
4. **Write to a copy**, with a hit count per replacement rule, correct repackaging, re-validation, and a before/after count of the tell being removed.

### Failure modes

Every row was hit for real during development.

| Symptom | Actual cause | Fix |
|---|---|---|
| `'charmap' codec can't decode byte` on N slides | Validator reading XML as cp1252 on Windows | `PYTHONUTF8=1`. The deck is fine |
| Validator passes, PowerPoint still offers Repair | `[Content_Types].xml` is not the first zip entry | Repackage with it first |
| Accents render as `?` or a replacement glyph | cp1252 stdout | Write UTF-8 to a file, read the file |
| Your edited deck now needs repair | Naive rezip wrote directory entries | Repackage without them |
| A replacement rule reports 0 hits | Span crosses two runs, or the text lives in layouts, masters, charts, or diagrams | Grep the whole `ppt/` tree for a short fragment first; a shortened span that matches elsewhere is a silent wrong replacement |
| A deck full of tells declared clean | Sentence-level pass never ran | Both passes are mandatory |

> **The OPC ordering one is worth knowing even if you never use this skill.** A `.pptx` whose zip has `[Content_Types].xml` anywhere but first will be rejected by PowerPoint while passing every validator, opening fine in python-pptx, and rendering correctly in LibreOffice. It is produced by naively rezipping an unpacked deck. The fix is repackaging with the content-types stream written first and no directory entries.

---

## Two edit tiers, and who decides

| | Tier 1: wording | Tier 2: structure and content |
|---|---|---|
| Keeps | Every text box, every idea, the layout | Nothing is guaranteed to survive |
| Examples | Em dash repairs, participle recasts, dropping an inflated adjective, plain verb for a noun phrase | Deleting eyebrows and footers, removing presenter narration, cutting body text to budget, deleting a filler slide |
| Applied | On request, to a copy | **Only what the operator selects**, from a menu the skill presents with the analysis |

Tier 1 leaves the deck looking the same and reading better. Tier 2 changes what the deck *is*, and on a designed template it changes the look of every slide. The skill's job is to put the analysis and the options in front of the operator, then wait.

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

**This skill depends on no other writing skill.** It requires a PowerPoint/pptx skill for validation and `python-pptx` for shape work. The two humanizers are complementary: use a prose humanizer on prose, and this one on decks.

## License

MIT. See [LICENSE](LICENSE).

---
name: humanize-presentations
description: Use when an existing slide deck reads as AI-generated, generic, or templated, or when asked to de-slop, humanize, tighten, or retouch a .pptx that already exists, or to review a deck's wording before it ships. Symptoms include em dashes everywhere, topic-label titles, everything in threes, filler slides, bullet walls, and hedged claims with no numbers. Not for building a deck from scratch.
---

# Humanize Presentations

Run an anti-slop pass over an existing `.pptx`: find the AI tells, fix them, and hand back an edited copy that opens.

This skill is self-contained. It needs no other writing skill.

## The Job

1. Open the deck safely (File Handling).
2. Run **both** passes: sentence-level, then structural.
3. Report findings ranked by damage.
4. On request, produce an edited **copy**. Never touch the original.

## Two Classes of Finding

| | Sentence-level | Structural |
|---|---|---|
| Examples | Em dashes, participles, inflation, hedging, AI vocabulary | Title style, repetition, slide shape, text density, notes |
| Applies | **Always.** No format makes these acceptable | Only if the deck's contract permits |

**A well-structured deck can still be full of bot patterns.** Establishing that the structure is deliberate never clears the prose. Run the sentence-level pass on every deck, always, even when the structure is exemplary.

---

## Pass 1: Sentence-Level Tells

Applies to every deck regardless of contract. Count what is countable; judge the rest sentence by sentence.

### No single marker is proof

**Slop is a convergence of signals, never one character.** Humans identify AI text at roughly 57% accuracy, barely above chance, and every individual punctuation marker has been measured as weak. Do not build a verdict on one tell. Build it on several tells landing in the same passage, plus the substance test below.

**The substance test is the strongest signal available.** Does the text contain concrete detail, real names, specific numbers, or lived experience? Generic competence with nothing checkable behind it is the actual defect. Everything else in this pass is a symptom of that.

### Em dashes and en dashes

**Remove them as a style choice, not as a detection claim.** Slide prose is tighter without them, and the aside pattern below is genuinely worth catching. But the frequency argument is dead: measured against a human baseline of 3.23 per 1,000 words, Twain scores 10.13 and GPT-4.1 scores 10.62. Told to write prose, Claude drops 98% and Gemini to zero. Density separates nothing.

What still carries signal is **function, not count**: `X — the thing that explains X` injected mid-sentence, and especially a nested pair inside one sentence. Judge the construction, never the tally.

Replace with a comma, colon, period, or parentheses, chosen per sentence. Use `·` **only if the deck already uses `·` as a separator**, and only for chrome such as footers and subtitles, never mid-sentence.

En dashes are correct in numeric ranges (`2024–2026`). Anywhere else, treat them as em dashes.

> Why these appear at all: em dashes are markdown leaking into prose. Models trained on markdown-saturated text internalize the dash as a structural boundary, and when told to drop formatting the headers and bullets go while the dash survives, because it is already prose-legal. Expect the same leakage as boldface runs, inline-header lists, and a reflex toward bullets over sentences.

### Decorative glyphs

**`§` is a strong tell.** It is a statutory section sign, and outside a legal document essentially no human decorates a slide with it, while an AI reaching for visual sophistication produces `§ 01`, `§ SECCIÓN 2`, `§ Overview`. Treat it as a strong prompt to look harder, not as proof on its own.

The same family, used as chrome rather than for meaning:

`§ ¶ ※ ⁂ ❖ ◈ ◆ ★ ✦ ✱ ▪ ► ▸ ‣ ∎ ⟡` and slash-chrome such as `// 01`, `/// SECTION`, `‹ 02 ›`

**Distinguish decoration from a separator system.** A glyph used consistently to separate items, such as `·` in a footer on every slide, is a design choice; leave it. A glyph used as a pseudo-label prefixing a title or a number is slop; delete the glyph and keep the words.

Fake-technical chrome labels are the same failure even without a glyph: `PROTOCOL / 002`, `MODULE 03`, `FIG. 01` on a deck with no protocols, modules, or figures. Delete them.

### Participles tacked on for fake consequence

`-ing` in English, `-ando/-iendo` in Spanish. The tell is a participle clause bolted to a finished sentence to manufacture significance: *"...affecting the NPS"*, *"...generando percepción de trato injusto"*.

Keep it when it states an actual method: *"by automating the triage"*. Cut or recast when it only restates a consequence.

### The rest

| Tell | Fix |
|---|---|
| Significance inflation (*key, pivotal, fundamental, strategic, crucial*) | Delete the adjective, or replace it with the number that earns it |
| Hedging and vague attribution (*experts say, it is estimated, studies show*) | Name the source, or cut the claim |
| AI vocabulary (*delve, leverage, robust, seamless, comprehensive, landscape, ecosystem*) | Plain equivalent, or restructure the line |
| Negative parallelism (*not only... but also*) | Once per deck maximum |
| Copula avoidance (*constitutes, represents, serves as, positions itself as*) | *is* |
| Filler connectives (*Moreover, Furthermore, Additionally*) | Usually just delete |
| Vague verbs in any title (*improve, enhance, optimize, leverage, streamline*) | Quantify or cut |
| Binary contrast (*It's not X. It's Y.*) | State Y directly |
| Throat-clearing opener (*Here's the thing, Let's be clear, In today's world*) | Delete; start at the first line that says something |
| Faux-insight setup (*What nobody tells you, The truth is*) | Delete the setup, keep the claim if it survives |
| Colon reveal used for drama (*The result: growth*) | Write the sentence |
| Fake-profound closer (*The future is already here, Only time will tell*) | End on a fact or the ask |
| Dramatic fragments used as emphasis (*Every time. No exceptions.*) | Fine once a deck, a tic beyond that |
| Synonym cycling for one concept across a slide | Pick one term and repeat it |

### Model artifacts

Leftover generation markup is conclusive when present. Search for `oaicite`, `[cite: 1]`, `contentReference`, `:contentReference[oaicite:0]`, stray `**` or `##` inside a text box, and citation brackets pointing at nothing. Also watch vocabulary that fingerprints a specific model, such as heavy *underscore*, *causal*, *empirical*, and *correlate*.

### Re-scan your own fixes

**A uniform pattern replaced by another uniform pattern is not a fix.** Removing ten `-ing` clauses by opening ten sentences with "and" trades one tell for a worse one, because the replacements cluster exactly where you were working.

For N instances of one tell, expect roughly N different repairs: split into two sentences, collapse into a list, recast as a conditional, promote the consequence to the main clause, or leave it alone. After any batch fix, re-read the edited passages as if someone else wrote them. If your repairs share a shape, redo them.

### Non-English decks

English word lists do not transfer; the patterns do. Map them instead of translating: `-ing` becomes `-ando/-iendo`; *delve/leverage/robust* becomes *abordar/aprovechar/robusto/integral*; *it's worth noting* becomes *cabe destacar/es importante señalar*. Judge structure and function.

---

## Pass 2: Structural Tells

**Establish the deck's contract first.** Most structural tells are also legitimate design choices, and a list padded with wrong flags gets the right ones ignored.

**Is it read alone, or presented live?**
Read alone (leave-behind, submitted deliverable, board pre-read): dense slide text is correct and empty speaker notes are correct. Do not thin the text, do not ask for notes.
Presented live: slide text is support, detail belongs in notes, and missing notes is a real finding.
*Evidence:* an explicit statement in the deck, a submission requirement, or populated notes.

**Is the structure imposed or chosen?**
Imposed by a rubric, template, or house format: repetition, fixed section names, and identical slide shapes are compliance, not slop.
Chosen by the author: the table below applies in full.
*Evidence:* step numbers in titles, section names mirroring a brief, a stated methodology.

**When the contract makes a pattern correct, do not report it at all.** Not softened, not as "consider".

| Tell | Fix |
|---|---|
| Topic-label titles (*Market Overview*) | A title stating the insight: what the data shows, not what the slide is about |
| Chart title names the chart, not the finding | State what happened |
| Everything in threes, third item padding | Let the counts differ |
| Bullets written as full sentences with periods | Fragments, one idea each |
| Filler slides: Agenda echoing section titles, *Key Takeaways*, *Thank You / Questions?* | Delete; put the ask on the closing slide |
| Bullet wall where a table or a single number belongs | Restructure |
| Icon-in-circle + bold title + two-line description, repeated identically | Vary the layout |
| Emoji in headers | Remove |
| **Generator template fingerprint**: the deck looks like a filled Gamma, Beautiful.ai, Pitch, or Canva AI template | The dominant deck tell of 2026. Reviewers have seen thousands. Replace stock imagery with real assets, apply real brand tokens, and break the uniform section rhythm |
| Generic AI stock imagery: abstract gradients, glowing circuitry, anonymous smiling teams | Real photos, real screenshots, real data, or nothing |
| Copy visibly overflowing or auto-shrunk to fit its box | Cut the copy, do not shrink the type |

Deeper visual and layout critique (palette, spacing, hierarchy) is a design review, not this pass.

---

## Slide Text vs Speaker Notes

They are opposites, and treating them alike is the most common way to damage a deck.

- **Slide text:** apply every sentence-level tell above. Do **not** add rhythm variation, first person, opinions, or conversational texture. Slide text is not prose, and this holds for read-alone decks too, where density is the point.
- **Speaker notes:** apply the sentence-level tells, and additionally let the rhythm vary and sound spoken, because they are.

---

## File Handling

**Use your environment's PowerPoint/pptx skill for all file mechanics.** Locate its scripts once and reuse the path:

```bash
S=$(dirname "$(find ~/.claude ~/.agents ~/.config -path '*skills/pptx/SKILL.md' 2>/dev/null | head -1)")
```

**Every Python and validator call needs `PYTHONUTF8=1`.** Without it, non-ASCII decks fail in ways that look exactly like corruption.

### A. Validate before reading

A broken file outranks its prose. Report and fix this before commenting on wording.

```bash
PYTHONUTF8=1 PYTHONIOENCODING=utf-8 python "$S/scripts/office/validate.py" deck.pptx
```

### B. Check the container

Validators check schema and relationships. They do **not** check OPC part order, so a deck can pass validation and still make PowerPoint offer to Repair it.

```python
import zipfile
n = [i.filename for i in zipfile.ZipFile(path).infolist()]
assert n[0] == '[Content_Types].xml'        # OPC requires it first
assert not any(x.endswith('/') for x in n)  # directory entries mean a naive rezip
```

### C. Repair, if either check failed

Rewrite the container losslessly. This changes no content.

```python
zin = zipfile.ZipFile(src)
parts = [i.filename for i in zin.infolist() if not i.filename.endswith('/')]
order = ['[Content_Types].xml'] + [n for n in parts if n != '[Content_Types].xml']
with zipfile.ZipFile(dst, 'w', zipfile.ZIP_DEFLATED) as zo:
    for n in order:
        zo.writestr(n, zin.read(n))
```

Then confirm every part is byte-identical to the source before continuing.

### D. Read

`markitdown deck.pptx` when available. Otherwise extract `<a:t>` runs from `ppt/slides/*.xml` and **write them to a UTF-8 file, then read the file**. Printing accented text to a Windows console mangles it.

### E. Write, always to a copy

1. Replace inside `<a:t>` content in `ppt/slides/*.xml` and `ppt/notesSlides/*.xml`.
2. **Report a hit count per replacement rule.** Zero hits means the span crosses two runs, not that the text is absent. Shorten the span and retry; never assume a rule applied.
3. Repackage per step C.
4. Re-run A and B on the output.
5. **Count the tell before and after.** Before greater than zero, after equal to zero, or the edit did not land.

**Name the output for what changed.** A file called `_FIXED` that repaired the container while leaving every tell in place misleads whoever opens it.

### Failure modes

| Symptom | Cause | Fix |
|---|---|---|
| `'charmap' codec can't decode byte` on N slides | Validator reading XML as cp1252 | `PYTHONUTF8=1`. The deck is fine |
| Validator passes, PowerPoint still offers Repair | `[Content_Types].xml` not the first entry | Step C |
| `markitdown: command not found` | Not installed | Extract `<a:t>` runs directly |
| Accents render as `?` or a replacement glyph | cp1252 stdout | Write UTF-8 to a file, read the file |
| Your edited deck now needs repair | Naive rezip wrote directory entries | Step C |
| A replacement reports 0 hits | Span crosses two `<a:t>` runs | Match a shorter span |
| Deck full of tells declared clean | Sentence-level pass skipped | Both passes are mandatory |

---

## Output

Report findings, most damaging first. For each: the slide number, the quoted text, the tell, and the replacement. Keep it to what matters; if there are thirty findings and three are load-bearing, lead with those three.

Close with counts: tells found, tells fixed, and the output path.

When nothing is wrong, say so plainly. A clean deck is a valid result.

## Common Mistakes

| Mistake | Fix |
|---|---|
| Clean structure read as a clean deck | The passes are independent. Always run the sentence-level scan |
| Gating sentence-level tells behind the contract | The gate covers structure only. Em dashes are never rubric-mandated |
| Estimating counts from impression | Count the countable ones |
| Missing `§` and its family | Scan for decorative glyphs explicitly; they are a Pass 1 tell, never contract-gated |
| Declaring a verdict from one marker | Slop is convergent. Require several signals plus the substance test |
| Treating em dash count as evidence | Density separates nothing. Judge the aside construction instead |
| Every repair for one tell coming out the same shape | Re-scan your own edits and vary them |
| Thinning a read-alone deck's text | Density is the point when nobody presents it |
| Demanding speaker notes for a submitted document | Empty notes are correct there |
| Flagging rubric-imposed section names | Compliance is not slop |
| Adding personality to slide text | That is for notes, never slides |
| Editing the original file | Always work on a copy |
| Commenting on content before validating | A broken deck outranks its prose |
| Offering the fix instead of producing it | If asked for a de-slop, hand back the edited copy |

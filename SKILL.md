---
name: humanize-presentations
description: Use when an existing slide deck reads as AI-generated, generic, or templated, or when asked to de-slop, humanize, tighten, retouch, or review the wording of a .pptx that already exists. Symptoms include the section name repeated in an eyebrow and a footer on the same slide, over 80 words per slide, presenter narration typed onto slides, generic claims with nothing checkable behind them, a deck that looks like a filled Gamma or Beautiful.ai template, topic-label titles, everything in threes, filler slides, bullet walls, decorative section glyphs, and leftover generation markup. Not for building a deck from scratch.
---

# Humanize Presentations

Run an anti-slop pass over an existing `.pptx`: measure it, find the tells, fix the wording, and put the structural choices in front of the operator.

This skill depends on no other writing skill. It requires a PowerPoint/pptx skill for validation and uses `python-pptx` for reading and editing shapes.

## The Job

1. Open the deck safely (File Handling A to D). Report a broken file before saying anything about its wording.
2. Measure the deck (the Analysis numbers in Output). The numbers frame every finding.
3. Run Pass 1 (text) and Pass 2 (slide and deck). Apply Rubric Protection to exactly the three things it covers.
4. Report: analysis, then Tier 1, then the Tier 2 menu.
5. Which verbs mean which action:
   - **de-slop / humanize / fix / clean / tighten** → apply Tier 1 to a copy and return it, without asking.
   - **review / check / what's wrong / before it ships** → analysis only. No file written.
   - Tier 2 is a menu in both cases. Never touch the original.

## What Gets Fixed and Who Decides

| | Tier 1: wording | Tier 2: structure and content |
|---|---|---|
| Keeps | Every shape, every line, every idea, the layout | Nothing is guaranteed to survive |
| Changes | Characters inside a line that survives | Which shapes exist, which lines exist, which slides exist, how much is on a slide |
| Examples | Em dash repairs, participle recasts, dropping an inflated adjective, plain verb for a noun phrase, deleting a glyph from inside a string, fixing a typo | Deleting eyebrows, footers, badges, callout labels; removing or relocating presenter narration; cutting body text to budget; rewriting a title; breaking a triad; turning a bullet wall into a table; deleting a filler slide |
| Applied | On request, to a copy | **Only what the operator selects from the menu, after the menu is on screen** |

**Arbitration.** If an edit removes or relocates a whole line, paragraph, shape, or slide, it is Tier 2, regardless of which pass found it. Tier 1 changes only characters inside a line that survives. Title rewrites are Tier 2 because a title asserts a claim.

**Pre-authorization.** "Do all of it" spoken before the analysis exists is not authorization for Tier 2. Present the menu; "do all of it" said with the menu on screen is. Every edit, either tier, goes to a copy.

**Why the split.** Tier 1 leaves the deck looking the same and reading better. Tier 2 changes what the deck is: it removes things the author or the generator put there, and on a designed template it changes the look of every slide. That decision belongs to the operator with the analysis in front of them.

## Ranking

One order, used everywhere findings are listed:

1. Missing substance (nothing checkable behind the claims)
2. Chrome that repeats itself
3. Word budget
4. Presenter's voice written down
5. Model artifacts (rare, but conclusive when present)
6. Everything else, by how many slides it touches

**No single marker is proof.** Humans identify AI text at roughly 57%, barely above chance, and every punctuation marker measured so far is weak on its own. Slop is a convergence of signals. Build a verdict on several tells landing together plus the substance test, never on one character.

## Rubric Protection

A brief, rubric, or house format can protect **exactly three things**, and only when there is evidence it imposed them:

1. Which sections exist
2. What those sections are called
3. A required cycle across parallel cases (the same sections repeated for case A and case B)

*Evidence:* step numbers in titles, section names mirroring a brief, a stated methodology, a required cycle across parallel sections.

Protected means: do not report the section's existence, its name, or the repetition of the cycle. That is all. **The rubric imposes content. The template imposes chrome.** Nothing about how a slide is dressed, how many words are on it, what narration sits on it, or what decorates it is ever protected. A brief that requires a section named `Estrategias (Paso 2)` protects that title appearing once. It does not protect an eyebrow above it, a footer echoing it, badges beside its items, a callout label on every card, or 130 words on the slide.

**Read-alone versus presented.** Establish which from evidence (an explicit statement, a submission requirement, populated notes). Read-alone means empty speaker notes are correct and the deck may run to more slides. It does not change the per-slide word budget, license narration on slides, or protect chrome. Presented means detail belongs in notes and missing notes is a real finding.

**When protection applies, do not report the pattern at all.** Not softened, not as "consider". But check that the pattern is one of the three before clearing it. A well-structured deck can still be full of bot patterns.

---

## Pass 1: Text

Applies to every deck. Nothing here is protected by any rubric.

### Substance

Does the text contain concrete detail, real names, specific numbers, lived experience? Generic competence with nothing checkable behind it is the actual defect. Everything else in this pass is a symptom of it. This is finding number one when present.

### Chrome that repeats itself

A generator dresses every slide the same way: an eyebrow or kicker above the title, the title, a section footer below, a deck footer below that, number or letter badges beside each item, a bold callout label on every card. Read one slide top to bottom and count how many shapes carry only labels.

The signature is the **same section label twice on one slide**: `RUTA A · SOPORTE` in the eyebrow and `Ruta A · Estrategias` in the footer, or `CONTEXTO` above and `Contexto` below. Hand-built decks rarely do this. Corporate templates sometimes do, through a breadcrumb in the header and a mandated footer, both set on the slide master. So before attributing it to a generator, check whether the box is a master or layout placeholder (File Handling D reports this). If it is the template's, say so: removing it changes every slide and may break a brand requirement, and that goes on the menu with that warning.

**Rule: a section label appears at most once per slide, as the title.** Eyebrows, kickers, section footers, badge numbers that exist only to number, and repeated callout labels (`Trade-off:`, `Note:`, `Dependency:`) stamped on every card all go. One deck-wide footer carrying the deck's name may stay. Three chrome bars per slide is the generator.

All chrome removal is Tier 2.

### Word budget

One rule, counted in the deck's own language: **flag every slide over 80 words; over 120 is never excusable.** Scale both by roughly 1.2 for Spanish. Count with the shape walk in File Handling D, not by impression.

Read-alone buys more slides, not more words on a slide. The per-slide budget is identical. Cut, do not shrink the type. Cutting is Tier 2 and goes on the menu per slide or as a batch.

### Presenter's voice written down

Lines a human would say aloud while pointing at the slide, typed onto it instead: `How to read this map:`, `This attacks the cost of the case that already happened.`, `What the team would measure to know it is working:`. Narration, not content.

**Not narration, never cut under this rule:** disclaimers (`figures are illustrative`), source lines, units, methodology notes, and any line that is the only key to a chart or image. If you cannot verify from the extracted text that the information appears elsewhere on the slide, keep the line and list it as a question for the operator.

On a presented deck narration moves to speaker notes. On a read-alone deck it is folded into the content it narrates or cut. Either is Tier 2.

### Register

Long noun phrases and technical vocabulary where the presenter would use a plain verb: *the routing of the case* for *routing*, *a well-labelled historical dataset* for *good labels*, *implementation of the verification protocol* for *verifying*. On a slide, use the words the presenter would say. Density of uncommon words across a deck is a stronger tell than any single one. Rewriting a phrase inside a surviving line is Tier 1.

### Em dashes and en dashes

Remove them as a style choice, not as a detection claim. Measured against a human baseline of 3.23 per 1,000 words, Twain scores 10.13 and GPT-4.1 scores 10.62; told to write prose, Claude drops 98% and Gemini to zero. Density separates nothing, and newer models suppress them on purpose. What still carries signal is function: `X — the thing that explains X` injected mid-sentence, and especially a nested pair inside one sentence.

Replace with a comma, colon, period, or parentheses, chosen per sentence. Use `·` only inside text that survives the chrome rule and only where the deck already uses `·` as a separator. En dashes are correct in numeric ranges (`2024–2026`); anywhere else treat them as em dashes. Tier 1.

> Why they appear: markdown leaking into prose. Models trained on markdown-heavy text hold the dash as a structural boundary; told to drop formatting, headers and bullets go and the dash survives because it is already prose-legal. Expect the same leakage as boldface runs, inline-header lists, and a reflex toward bullets over sentences.

### Decorative glyphs

`§` is a strong prompt to look harder, not proof on its own. It is a statutory section sign; a generator reaching for sophistication produces `§ 01`, `§ SECCIÓN 2`, `§ Overview`. **Exception: `§` followed by a real statute or clause number is a citation. Keep it.** The family used as chrome: `¶ ※ ⁂ ❖ ◈ ◆ ★ ✦ ✱ ▪ ► ▸ ‣ ∎ ⟡`, slash-chrome such as `// 01`, `/// SECTION`, `‹ 02 ›`, and fake-technical labels with no glyph at all: `PROTOCOL / 002`, `MODULE 03`, `FIG. 01` on a deck with no protocols, modules, or figures.

Deleting a glyph from inside a string that survives is Tier 1. Deleting the whole label shape is Tier 2. The separator question (`·` used consistently) applies only to chrome that survives the chrome rule; if the box carries a section label, the box goes and the glyph is moot.

### Participles tacked on for fake consequence

`-ing` in English, `-ando/-iendo` in Spanish, bolted to a finished sentence to manufacture significance: *"...affecting the NPS"*, *"...generando percepción de trato injusto"*. Keep it when it states an actual method: *"by automating the triage"*. Recast inside the line or split into a new sentence; both are Tier 1, the line survives.

### The rest

| Tell | Fix | Tier |
|---|---|---|
| Significance inflation (*key, pivotal, fundamental, strategic, crucial*) | Delete the adjective, or replace it with the number that earns it | 1 |
| Hedging and vague attribution (*experts say, it is estimated, studies show*) | Name the source if the deck has one; otherwise list the claim for the operator | 1 if named, else menu |
| AI vocabulary (*delve, leverage, robust, seamless, comprehensive, landscape, ecosystem*) | Plain equivalent | 1 |
| Negative parallelism (*not only... but also*) | A tic when dense relative to length; recast | 1 |
| Copula avoidance (*constitutes, represents, serves as, positions itself as*) | *is* | 1 |
| Filler connectives (*Moreover, Furthermore, Additionally*) | Delete the word | 1 |
| Binary contrast (*It's not X. It's Y.*) | State Y | 1 |
| Throat-clearing opener (*Here's the thing, Let's be clear, In today's world*) | Delete the line | 2 |
| Faux-insight setup (*What nobody tells you, The truth is*) | Delete the setup words, keep the claim | 1 |
| Colon reveal used for drama (*The result: growth*) | Write the sentence | 1 |
| Fake-profound closer (*The future is already here, Only time will tell*) | Delete the line | 2 |
| Dramatic fragments as emphasis (*Every time. No exceptions.*) | A tic when dense; recast | 1 |
| Synonym cycling for one concept across a slide | Pick one term | 1 |

### Model artifacts

Conclusive when present. Search the whole `ppt/` tree for `oaicite`, `[cite: 1]`, `contentReference`, `:contentReference[oaicite:0]`, stray `**` or `##` inside a text box, and citation brackets pointing at nothing. Vocabulary that fingerprints a specific model (heavy *underscore*, *causal*, *empirical*, *correlate*) is a weaker signal in the same family.

### Re-scan your own fixes

A uniform pattern replaced by another uniform pattern is not a fix. Removing ten `-ing` clauses by opening ten sentences with "and" trades one tell for a worse one, because the replacements cluster where you were working. For N instances of one tell, expect roughly N different repairs. After any batch fix, re-read the edited passages as if someone else wrote them. If your repairs share a shape, redo them.

### Non-English decks

English word lists do not transfer; the patterns do. Spanish markers that actually fingerprint generated text: *profundizar en, ahondar en, en un mundo cada vez más, en el ámbito de, es crucial destacar, es fundamental señalar, cabe destacar, potenciar, holístico, sinergia, robusto*. **`abordar` and `integral` are ordinary business Spanish; do not flag them on sight.** Map `-ing` to `-ando/-iendo`. Judge structure and function.

---

## Pass 2: Slide and Deck

Applies to every deck. Only the three things under Rubric Protection are ever cleared.

### Titles

Two failures. A **topic label** (`Market Overview`, `Riesgos`) names the subject and says nothing; a **vague-verb title** (`Improving intake`, `Optimización del proceso`) claims motion without a number. Both are fixed by a title that states the insight: what the data shows, not what the slide is about.

**Rubric exception, applied first:** if the title is a rubric-imposed section name (`Estrategias (Paso 2)`), it is protected. Do not report it, even if it contains a vague verb. Every title rewrite that is reported is Tier 2.

### The rest

| Tell | Fix | Tier |
|---|---|---|
| Chart title names the chart, not the finding | State what happened | 2 |
| Everything in threes, third item padding | Let the counts differ; never convert every triad to a pair | 2 |
| Bullets written as full sentences with periods | Fragments, one idea each | 1 if only punctuation and function words change, else 2 |
| Filler slides: Agenda echoing section titles, *Key Takeaways*, *Thank You / Questions?* | Delete; put the ask on the closing slide | 2 |
| Bullet wall where a table or one number belongs | Restructure | 2 |
| Emoji in headers | Remove | 1 |

### Requires rendering

These cannot be judged from extracted text. Render first (the pptx skill's `scripts/thumbnail.py deck.pptx <prefix>`, or LibreOffice to PDF to images). If you cannot render, write **not assessed, no rendering** against each and do not guess.

| Tell | Observable route |
|---|---|
| Generator template fingerprint: the deck looks like a filled Gamma, Beautiful.ai, Pitch, or Canva AI template | Rendered slides only. Reviewers have seen thousands; it fires before anyone reads a word |
| Generic AI stock imagery: abstract gradients, glowing circuitry, anonymous smiling teams | Rendered slides. Proxy without rendering: list `ppt/media/`; many large stock-looking images and no screenshots is a prompt to render |
| Icon-in-circle + bold title + two-line description, repeated identically | Rendered slides, or the shape walk showing the same group repeated with the same geometry |
| Copy auto-shrunk to fit its box | Countable without rendering: grep `ppt/slides/*.xml` for `<a:normAutofit` with a `fontScale` attribute |

---

## Slide Text vs Speaker Notes

Opposites, and treating them alike is the most common way to damage a deck.

- **Slide text:** every Pass 1 tell applies. Do not add rhythm variation, first person, opinions, or conversational texture. Slide text is not prose. This holds for read-alone decks.
- **Speaker notes:** the Pass 1 tells apply, and the notes may vary in rhythm and sound spoken, because they are.

---

## Output

Three parts, in this order.

**1. Analysis.** The deck's numbers first: slides; words per slide (average, max, count over 80, count over 120); shapes per slide carrying only labels; slides where a section label appears in both the top and bottom band; narration lines; `normAutofit fontScale` count; the rubric-protection call (which of the three apply, with the evidence); and the read-alone or presented call. Then every finding, in Ranking order, with slide number, quoted text, the tell, the fix, and the tier. Lead with the few that carry the most damage, but list all of them.

**2. Tier 1.** What was applied to the copy, or what will be on request. Close with counts by tell, before and after, and the output path.

**3. Tier 2 menu.** One line per option, grouped, each with the slide count it touches, what it removes or changes, what the slide looks like after, and any warning (template-level placeholder, brand footer, disclaimer). Example shape:

> **A. Chrome** (21 slides). Delete the eyebrow and the section footer on every slide; keep one deck-wide footer. Each slide loses two label bars and keeps its title. Changes the template's look. Per-slide shapes, not master placeholders.
> **B. Badges and callout labels** (10 slides). Delete the numbered badges and the repeated `Trade-off:` label; keep the trade-off text as a plain second line.
> **C. Presenter narration** (14 lines, 9 slides). Cut the lines listed. One line kept pending your answer: slide 11's `figures are illustrative` reads as a disclaimer.
> **D. Word budget** (19 slides over 80 words, 12 over 120). Cut body text to budget, preserving every number and every named trade-off. This rewrites, not trims; approve per slide or as a batch.

State which options you would take and why, then stop.

When nothing is wrong, say so plainly. A clean deck is a valid result.

---

## File Handling

**Use your environment's PowerPoint/pptx skill for validation.** Locate it once:

```bash
S=$(dirname "$(find ~/.claude ~/.agents ~/.config -path '*skills/pptx/SKILL.md' 2>/dev/null | head -1)")
[ -z "$S" ] || [ "$S" = "." ] && { echo "pptx skill not found"; exit 1; }
```

**Every Python and validator call needs `PYTHONUTF8=1`.** Without it, non-ASCII decks fail in ways that look exactly like corruption. If `python-pptx` is missing: `pip install python-pptx`.

Throughout: `src` is the operator's file, `dst` is the copy you write. Never write to `src`.

### A. Validate before reading

```bash
PYTHONUTF8=1 PYTHONIOENCODING=utf-8 python "$S/scripts/office/validate.py" "$src"
```

A broken file outranks its prose. Report and repair this before commenting on wording.

### B. Check the container

Validators check schema and relationships. They do not check OPC part order, so a deck can pass validation and still make PowerPoint offer to Repair it.

```python
import zipfile
src = 'deck.pptx'
n = [i.filename for i in zipfile.ZipFile(src).infolist()]
assert n[0] == '[Content_Types].xml'         # OPC requires it first
assert not any(x.endswith('/') for x in n)   # directory entries mean a naive rezip
```

### C. Repair, if either check failed

Rewrite the container losslessly, then prove it.

```python
import zipfile
src, dst = 'deck.pptx', 'deck_repaired.pptx'
zin = zipfile.ZipFile(src)
parts = [i.filename for i in zin.infolist() if not i.filename.endswith('/')]
CT = '[Content_Types].xml'
with zipfile.ZipFile(dst, 'w', zipfile.ZIP_DEFLATED) as zo:
    for name in [CT] + [p for p in parts if p != CT]:
        zo.writestr(name, zin.read(name))
zout = zipfile.ZipFile(dst)
assert set(parts) == {i.filename for i in zout.infolist()}
assert all(zin.read(p) == zout.read(p) for p in parts)   # every part byte-identical
```

The archive itself will differ (timestamps, compression); the parts must not.

### D. Read: the shape walk

`markitdown` and raw `<a:t>` extraction flatten to text and cannot count shapes or place them. Use `python-pptx` and record, per slide: shape count, words, and for each text-bearing shape its vertical band, whether it is a placeholder, and its text. Also read `ppt/slideLayouts/*.xml` and `ppt/slideMasters/*.xml`: a label living there is template-level and changing it changes every slide.

```python
from pptx import Presentation
src = 'deck.pptx'
p = Presentation(src); H = p.slide_height
for i, s in enumerate(p.slides, 1):
    for sh in s.shapes:
        if not sh.has_text_frame or not sh.text_frame.text.strip():
            continue
        y = (sh.top or 0) / H
        band = 'top' if y < 0.15 else ('bottom' if y > 0.85 else 'body')
        # record (i, band, sh.is_placeholder, sh.text_frame.text.strip())
```

Per-slide words = the sum over its text shapes; a chrome echo = a label token appearing in both a top and a bottom shape of the same slide. Write the walk to a UTF-8 file and read that. Printing accented text to a Windows console mangles it.

### E. Tier 1 write: characters inside a run

Replace inside `<a:t>` content in `ppt/slides/*.xml` and `ppt/notesSlides/*.xml`, then repackage per C. **Report a hit count per replacement rule.** Zero hits means the span crosses two runs, or the text lives in a part you did not open (layouts, masters, `ppt/charts/`, `ppt/diagrams/`), or the string does not match byte-for-byte. Grep the whole `ppt/` tree for a six-to-eight character fragment before shortening any span; a shortened span that finally matches somewhere else is a silent wrong replacement.

### F. Tier 2 write: shapes, lines, notes, slides

Only after the operator selects from the menu. Use `python-pptx` on a copy; re-run A and B on the output regardless.

- **Delete a shape** (eyebrow, footer, badge, label): remove the element, never blank its text. `sh._element.getparent().remove(sh._element)`. A blanked run leaves an empty box in its layout slot.
- **Move narration to notes:** `s.notes_slide.notes_text_frame.text = ...` creates the notes part if missing. Then delete the shape as above.
- **Delete a slide:** follow the pptx skill's edit path: unpack, remove the entry from `<p:sldIdLst>` in `ppt/presentation.xml`, run its `scripts/clean.py` to drop the orphaned part, media, and rels, repack from inside the directory. Do not hand-delete parts.
- **Master or layout placeholder:** editing it changes every slide. It goes on the menu with that warning and is done only if selected.

### G. Verify

Before greater than zero, after equal to zero, or the edit did not land. Count by type: replacement hits for Tier 1; shapes per slide and slide count for Tier 2. Re-run A and B on `dst`. Name the output for what changed; a file called `_FIXED` that repaired the container while leaving every tell in place misleads whoever opens it.

### Failure modes

| Symptom | Cause | Fix |
|---|---|---|
| `'charmap' codec can't decode byte` on N slides | Validator reading XML as cp1252 | `PYTHONUTF8=1`. The deck is fine |
| Validator passes, PowerPoint still offers Repair | `[Content_Types].xml` not the first entry | Step C |
| `pptx skill not found` | Discovery returned nothing | Install a pptx skill or set `S` by hand |
| Accents render as `?` or a replacement glyph | cp1252 stdout | Write UTF-8 to a file, read the file |
| Your edited deck now needs repair | Naive rezip wrote directory entries | Step C |
| A replacement reports 0 hits | Span crosses runs, or lives in layouts, masters, charts, diagrams | Grep the whole tree first (E) |
| Deleted a label but the box is still there | Blanked the run instead of removing the shape | Step F |
| Chrome you cannot find in `ppt/slides/` | It is a layout or master placeholder | Read layouts and masters (D); menu with warning |
| Deck full of tells declared clean | Pass 1 skipped after Rubric Protection cleared the structure | Protection covers three things. Run Pass 1 always |

## Common Mistakes

| Mistake | Fix |
|---|---|
| Clearing chrome, word count, or narration as rubric compliance | Protection covers which sections exist, their names, and a required cycle. Nothing else |
| Reading a read-alone contract as license for padding | It buys more slides, not more words on a slide |
| Passing a slide with the section name in the eyebrow and the footer | Same label twice on one slide. Cut to one, as the title, via the menu |
| Deleting a footer without checking whether it is a master placeholder | Layout and master edits change every slide. Warn on the menu |
| Cutting `figures are illustrative` as narration | Disclaimers, sources, units, and chart keys are never narration |
| Applying Tier 2 without the menu on screen | Structure and content are the operator's call. Present, then wait |
| Rewriting a title as Tier 1 | A title asserts a claim. Titles are Tier 2 |
| Rewriting a rubric-imposed section title | Protected, even if it contains a vague verb |
| Every repair for one tell coming out the same shape | Re-scan your own edits and vary them |
| Converting every triad to a pair | The same uniformity with a different count |
| Flagging `abordar` or `integral` as AI Spanish | Ordinary business Spanish |
| Judging template fingerprint or stock imagery from text | Requires rendering. Say "not assessed" if you cannot |
| Estimating counts from impression | The shape walk counts. Use it |
| Commenting on content before validating | A broken deck outranks its prose |
| Offering Tier 1 instead of producing it on a de-slop request | Hand back the copy with Tier 1 applied |

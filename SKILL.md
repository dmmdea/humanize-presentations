---
name: humanize-presentations
description: Use when an existing slide deck reads as AI-generated, generic, or templated: topic-label titles, everything in threes, filler slides, bullet walls, decorative chrome. Also use when asked to de-slop, humanize, tighten, or retouch a .pptx that already exists, or to review a deck's text before it ships. Not for building a deck from scratch, and not for visual or layout design review.
---

# Humanize Presentations

## Overview

`humanize-writing` is tuned for prose. Applied whole to a deck it does real damage. Its rhythm and voice passes tell you to add fragments, first person, and lived detail, which is right for a speaker's notes and wrong for a slide.

This skill covers what that one misses: deck-specific tells, and which of them apply to *this* deck.

## Two Classes of Finding, Never Confused

**Sentence-level tells always apply.** Em dashes, participle pile-ons, inflation, hedging, AI vocabulary. No rubric, template, or format makes these acceptable. Scan for them on every deck, always. Skip to Sentence-Level Tells and run that pass no matter what the contract turns out to be.

**Structural patterns are contract-dependent.** Title style, repetition, slide shape, text density, speaker notes. These need the gate below before you flag them.

The failure this ordering prevents: establishing that a deck's structure is legitimate, then concluding the deck is clean. A deck can be perfectly structured and still be full of bot patterns. Run both passes.

## Establish the Deck's Contract (Structural Findings Only)

**Do this before flagging anything structural.** Most structural "tells" are also legitimate design choices, and which they are depends entirely on the contract. Read the deck, then answer two questions from evidence in the file:

**1. Is it read alone, or presented live?**
- *Read alone* (leave-behind, submitted deliverable, board pre-read) → dense slide text is **correct**. Empty speaker notes are **correct**. Do not thin the text and do not ask for notes.
- *Presented live* → slide text is support, detail belongs in notes. Missing notes is a real finding.

Evidence: an explicit statement in the deck, a submission requirement, notes present and populated, or slide density that is consistent by design rather than by accident.

**2. Is its structure imposed or chosen?**
- *Imposed* by a rubric, assignment, template, or house format → repetition, fixed section names, and identical slide shapes are **compliance, not slop**. Do not flag them.
- *Chosen* by the author → the tells below apply in full.

Evidence: step numbers in titles, section names that mirror a brief, a stated methodology, a repeated cycle across parallel sections.

**When the contract makes a pattern correct, do not report it as a finding at all**. Not as a softened one, not as "consider". A list padded with wrong flags gets the right ones ignored.

## Sentence-Level Tells, Always Applied

Count these; never estimate from impression.

| Tell | Fix |
|---|---|
| **Em dash** (—) | **House rule: none.** Replace with a comma, colon, period, parentheses, or `·`. Flag every one, and flag the aside pattern hardest: "X — the thing that explains X" mid-sentence is the AI signature even at a count of one |
| Participle tacked on to add fake consequence: `-ing` in English, `-ando/-iendo` in Spanish ("affecting the NPS", "generando percepción de…") | Delete it, or make it its own sentence with a real source. Keep it only when it states an actual method ("by automating the triage") |
| Significance inflation ("clave", "estratégico", "fundamental", "pivotal", "key") | Delete the adjective or replace with the number that earns it |
| Hedging, vague attribution ("los expertos", "se estima que") | Name the source or cut the claim |
| Negative parallelism ("no solo… sino también") | Once per deck maximum |
| Copula avoidance ("constituye", "representa", "se posiciona como") | "es" / "is" |

### Re-Scan Your Own Fixes

**A uniform pattern replaced by another uniform pattern is not a fix.** Removing ten `-ando` clauses by opening ten sentences with "y" trades one tell for a worse one, because the replacements cluster in the passages you just touched.

After any batch fix, re-scan the edited text as if someone else wrote it. If your repairs share a shape, redo them. For a set of N instances of one tell, expect roughly N different repairs: split into two sentences, collapse into a single list, recast as a conditional, promote the consequence to the main clause, or leave it when the construction is genuinely doing work.

Judge each instance in its own sentence. The same participle can be slop in one line and correct in the next.

### Non-English Decks

`humanize-writing`'s `ai-tells.md` is an English word list and does **not** transfer. The *patterns* do. Map them: `-ing` pile-on → `-ando/-iendo`; "delve/leverage/robust" → "abordar/aprovechar/robusto/integral"; "It's worth noting" → "cabe destacar/es importante señalar". Judge structure and function, never the English vocabulary list.

## Structural Tells, Contract Permitting

Only after the gate above clears them:

| Tell | Fix |
|---|---|
| Topic-label titles ("Market Overview") | Action title stating the insight (see `consulting-frameworks`) |
| Chart title names the chart, not the finding | State what the data shows |
| Everything in threes, with the third padding | Cut to two, or let counts differ |
| Bullets written as full sentences with periods | Fragments; one idea each |
| Filler slides: Agenda echoing the section titles, "Key Takeaways", "Thank You / Questions?" | Delete; put the ask on the closing slide |
| Bullet wall where a table or one number belongs | Restructure |
| Icon-in-circle + bold title + two-line description, repeated | Break the grid |
| Emoji in headers, decorative chrome tags, purple gradients | Remove |
| Vague verbs in any title (improve, enhance, optimize, leverage, streamline) | Quantify or cut |

## Two Channels

- **Slide text**: apply `humanize-writing` passes 2, 3, 5, 6 only (inflation, AI vocabulary, boldface/emoji, hedging). Never passes 5-rhythm or 8-soul; slide text is not prose.
- **Speaker notes**: apply passes 5 and 8. Notes are spoken and should sound like a person talking.

Read-alone decks have no second channel. Treat their slide text as prose and apply the full skill.

## File Handling

**REQUIRED SKILL: `pptx` (document-skills). Invoke it before touching any .pptx.** Never hand-roll `zipfile` parsing as a shortcut: doing so skips validation, and a deck can be broken before you ever read it.

Set `$S` to the pptx skill directory. **Every Python or validator call needs `PYTHONUTF8=1`** or non-ASCII decks fail in ways that look like corruption and are not.

**Step 1, validate before reading.** Run this before commenting on any deck's content. If it fails, report that first; a broken file outranks its prose.

```bash
PYTHONUTF8=1 PYTHONIOENCODING=utf-8 python "$S/scripts/office/validate.py" deck.pptx
```

**Step 2, check the container. `validate.py` does not do this and will pass a deck PowerPoint refuses to open.**

```python
z = zipfile.ZipFile(p); n = [i.filename for i in z.infolist()]
assert n[0] == '[Content_Types].xml'          # OPC requires it first, or PowerPoint offers Repair
assert not any(x.endswith('/') for x in n)    # directory entries mean a naive rezip
```

**Step 3, read.** `markitdown deck.pptx` when installed. When it is not, extract `<a:t>` runs from `ppt/slides/*.xml` yourself and **write them to a UTF-8 file**, then read that file. Printing Spanish to a cp1252 console mangles it into unreadable output.

**Step 4, write, always to a copy.** Never edit the user's original, and never write in place.

1. Replace inside `<a:t>` content in `ppt/slides/*.xml` and `ppt/notesSlides/*.xml`.
2. **Report a hit count per replacement rule.** Zero hits means the text is split across runs, not that the text is absent. Fix the rule; never assume it applied.
3. Repackage with `[Content_Types].xml` written first, no directory entries, `ZIP_DEFLATED`.
4. Re-run steps 1 and 2 on the output.
5. **Count the tell before and after.** Before greater than zero and after equal to zero, or the edit did not land.

**Name the output for what changed.** A file called `_FIXED` that repaired the container while leaving every tell in place is a lie to the person who opens it.

## Failure Modes

Each of these was hit for real; none are hypothetical.

| Symptom | Cause | Fix |
|---|---|---|
| `Could not check N slide part(s): 'charmap' codec can't decode byte` | Validator reading XML as Windows cp1252 | `PYTHONUTF8=1`. Not a corrupt deck |
| validate.py passes, PowerPoint still offers Repair | `[Content_Types].xml` not the first zip entry | Repackage with it first. validate.py never checks this |
| `markitdown: command not found` | Not installed on this machine | Extract `<a:t>` runs directly |
| Accents render as `?` or `�` in output | cp1252 stdout | Write UTF-8 to a file, read the file |
| Your edited deck now prompts for Repair | Naive rezip wrote directory entries | Repackage per step 4.3 |
| A replacement reports 0 hits | The span crosses two `<a:t>` runs | Match a shorter span that sits inside one run |
| Deck full of tells declared clean | Sentence-level pass never run | Both passes are mandatory. See Two Classes of Finding |

## Other Owners

Visual and layout QA belongs to `gstack-design-review`. Building from scratch belongs to `mba-case-deck` or `business-redactor`. This skill changes words, not packaging and not design.

## Common Mistakes

| Mistake | Fix |
|---|---|
| **Clean structure read as a clean deck** | The two passes are independent. Run the sentence-level scan even when the structure is exemplary |
| Gating sentence-level tells behind the contract | The gate covers structure only. Em dashes are never rubric-mandated |
| Estimating dash or tell counts from impression | Count them |
| Every fix for one tell coming out the same shape | Re-scan your own edits; vary the repair |
| Flagging structural patterns before establishing the contract | Contract first, for structure |
| Thinning a read-alone deck's text | Density is the point when nobody presents it |
| Demanding speaker notes for a submitted document | Empty notes are correct there |
| Flagging rubric-imposed section names | Compliance is not slop |
| Reporting a long list where three findings matter | Rank by damage; drop the rest |
| Hand-rolling `zipfile` instead of invoking `pptx` | The skill is required, not advisory |
| Commenting on content before validating the file | Validate first; a broken deck outranks its prose |
| Offering the fix repeatedly instead of applying it | If the user asked for a de-slop, produce the edited copy |

---
name: "lecture-study-notes"
description: "Turn lecture slides, tutorials and diagrams into A4 study-note PDFs: 'In simple words' box per slide, red/blue/grey marking, exam drills, glossary and keywords."
---

# Lecture Study Notes (Colour 985 method) — v2

Use this whenever the user uploads lecture slides, a tutorial sheet, or a course diagram and wants study notes, slide explanations, or tutorial answers. If nothing is attached, ask for the slides (.pdf or .pptx) first; never invent slide content.

## Output rules (always)
- **One separate PDF per uploaded file** (e.g. `Week8_ML_Slide_Notes.pdf`, `Tutorial8_ML_Answers.pdf`, `ML_Steps_MindMap_Notes.pdf`).
- **A4 portrait**, margins 14 mm, **sans-serif font** (Inter, fallback DejaVu Sans), body 9.6 pt.
- **English** only, written in plain, simple language.
- **Every slide card starts with an "In simple words" box** (1–3 sentences a 10-year-old can understand).
- **Every professional/academic term** on a slide gets a synonym or plain-English explanation (Terms table + Glossary A–Z).
- **Every slide is included**, also divider/picture-only slides (short card with "In simple words" only).
- Footer: `<Course code> <Course name> | <Doc title> | page N` (not on cover). If the course code is not in the slides, leave it out and ask the user; never guess.
- Deliver with SendUserFile; reply with a short summary only.

## The Colour marking rule (+ the simple-words box)
| Colour | Label | Put here | Mental test |
|---|---|---|---|
| Yellow `#c9a000` / border `#e0b000` on `#fffaea` | IN SIMPLE WORDS | Whole slide in 1–3 easy sentences, everyday picture, no jargon. Always first. | "Could a 10-year-old get it?" |
| Red `#d62828` on `#fdecec` | RED · MEMORISE | Definitions ("X is…"), lists/steps (1,2,3), model/theory/algorithm names, formulas | "What **is** it?" |
| Blue `#1d63d6` on `#e8f0fc` | BLUE · UNDERSTAND | Purpose, why it exists, background, how it works | "What is it **for**?" |
| Grey `#8e8e93` on `#f4f4f5` | NO LINE · EXAMPLE | Examples, stories, pictures, case studies | "**For example**…" |

Limits: max 2–3 red items per slide; never make a whole paragraph red. If a slide is only pictures/examples, it gets no red. The yellow box is not exam wording — it is the entry point for reading.

## Extra boxes
- **In simple words** (yellow, see table): 1–3 short sentences explaining the whole slide as if to a 10-year-old. Use an everyday picture (friends, games, food, school, Grab/Shopee) — no jargon; if a term can't be avoided, explain it in the same sentence. Always the **first** box, right under the slide image.
- **Caution** (`#b26b00` on `#fff6e5`): slide wording that is imprecise, has a typo, or differs from the standard textbook answer (e.g. "high bias → overfitting" should be underfitting). Give the correct version.
- **Exam drill** (`#6a3fb5` on `#f3eefc`): format `Exam Q: …` + model answer, for questions or calculations on the slide (recompute numbers to verify).
- **Terms table** (header `Term | Synonym / plain English`): EVERY professional/academic word on the slide (e.g. phenomenon, attributed, subjective, artifact, SaaS, aggregation), not just the main concept.
- **Close-book keywords**: 2–4 short English keyword chips (black outline) per slide, labelled `CLOSE-BOOK KEYWORDS`.

**Card order (fixed):** Slide image → In simple words → Red → Blue → Grey → Caution → Exam drill → Terms → Keywords.

## Document 1 – Slide notes (lecture deck)
1. **Cover**: tag "Study notes · Week N", title, course, lecturer, contents line.
2. **How to use these notes** page:
   - Row 1: three legend cards — RED · MEMORISE (definitions, lists/steps, model/method names, **Exam material.**), BLUE · UNDERSTAND (what it is for, why it exists, how it works — understand, don't memorise word for word), NO LINE · EXAMPLE (examples and stories, read once then move on).
   - Grey rule bar: `"what it IS" → RED | "what it's FOR" → BLUE | "for example" → no line`.
   - Row 2: three legend cards — IN SIMPLE WORDS (each slide explained as if to a 10-year-old; read this first), CAUTION (slide wording that is unclear or differs from textbooks, with the correct version), EXAM DRILL (model answers to the questions the slides ask).
   - **4 steps for every slide**: 1) read the slide title first; 2) read "In simple words", then ask of each sentence: what it IS? what it's FOR? an EXAMPLE?; 3) learn only the 1–2 red items per slide; 4) close the book and write the 2–4 close-book keywords on paper.
   - **2 don'ts**: don't colour everything red (max 2–3 per slide); don't get stuck — mark "?" and keep reading.
3. **One card per slide** (`break-inside: avoid`):
   - Header: black badge `SLIDE n` + slide title.
   - The original slide image (render every page with `pdftoppm -r 110 -jpeg -jpegopt quality=80`) so all diagrams are included.
   - Boxes in the fixed card order above.
4. **One-page revision summary**: table of slide number + all keyword chips.
5. **Glossary A–Z**: every term + synonym + slide number(s).

## Document 2 – Tutorial answers
- Cover + How-to-use legend page (same as above).
- One card per question with:
  - Yellow **In simple words**: what the question is really asking, in 1–2 easy sentences.
  - Red **Answer** (tables for comparisons/lists of examples).
  - Blue **Why I chose this answer** (link each choice to specific lecture slide numbers and lecture terms).
  - Purple **Short exam version** (3–4 lines).
  - Terms table + close-book keywords.
- Tutorial glossary table at the end.
- Say clearly when an answer uses knowledge beyond the slides.

## Document 3 – Diagram / mind map / single-page handout
- Trim whitespace from the image (`convert -trim +repage -border 20`) and show it large on its own page.
- Yellow **In simple words** for the whole diagram.
- A memory trick (acronym sentence) and a table mapping it to the lecture's version of the same process.
- One card per branch with In simple words / Red / Blue / Grey / Terms / Keywords.
- End with a "close-book check" task.

## Build method
1. Extract text: `pdftotext -layout`; render pages: `pdftoppm -r 110 -jpeg` (files are `p-01.jpg`… for <100 pages); view contact sheets (`montage ... -tile 4x -geometry 360x270+3+3`) to read picture/diagram-only slides. For .pptx, convert to PDF first (`soffice --headless --convert-to pdf`).
2. Write content as Python data, one dict per slide. Use distinct keys so nothing clashes: `no` (slide number), `ti` (title), `s` (simple words), `r` (red list), `b` (blue list), `n` (example list), `c` (caution list), `d` (list of (question, answer)), `t` (list of (term, synonym)), `k` (keywords).
3. Render HTML → PDF with WeasyPrint (`pip install --break-system-packages weasyprint`). Build the CSS as a normal string (no `%` formatting — CSS contains `%`).
4. Check: page size A4, preview several pages (cover, how-to, a dense card, glossary), keep file size reasonable (JPEG images).

## Core CSS
```css
@page { size: A4 portrait; margin: 14mm 14mm 16mm 14mm; }
html { font-family: Inter, 'DejaVu Sans', sans-serif; font-size: 9.6pt; line-height: 1.42; color: #1d1d1f; }
.card { break-inside: avoid; margin: 0 0 5mm; border: 1px solid #e2e2e6; border-radius: 2.5mm; padding: 3mm 3.5mm; }
.card .no { font-weight: 800; font-size: 8pt; color: #fff; background: #1d1d1f; border-radius: 1mm; padding: .6mm 2mm; }
.card img { display: block; margin: 0 auto 2.5mm; height: 66mm; width: auto; border: 1px solid #ddd; }
.blk { border-radius: 1.5mm; padding: 2mm 3mm; margin: 1.5mm 0; }
.simple { background: #fffaea; border-left: 4px solid #e0b000; } .simple .lab { background: #c9a000; }
.r { background: #fdecec; border-left: 4px solid #d62828; } .r .lab { background: #d62828; }
.b { background: #e8f0fc; border-left: 4px solid #1d63d6; } .b .lab { background: #1d63d6; }
.n { background: #f4f4f5; border-left: 4px solid #b9b9bf; color: #555; } .n .lab { background: #8e8e93; }
.fix { background: #fff6e5; border-left: 4px solid #b26b00; } .fix .lab { background: #b26b00; }
.drill { background: #f3eefc; border-left: 4px solid #6a3fb5; } .drill .lab { background: #6a3fb5; }
.lab { display: inline-block; font-size: 7pt; font-weight: 800; letter-spacing: .08em; text-transform: uppercase; padding: .5mm 2mm; border-radius: 1mm; color: #fff; }
.chip { display: inline-block; border: 1.3px solid #1d1d1f; border-radius: 4mm; padding: .3mm 2.4mm; margin: .6mm 1mm .6mm 0; font-size: 8.3pt; font-weight: 600; }
.legend { display: grid; grid-template-columns: repeat(3, 1fr); gap: 3mm; margin: 0 0 4mm; }
.legend > div { border-radius: 2.5mm; padding: 3mm; }
.rulebar { background: #f2f2f4; border-radius: 2.5mm; padding: 3mm; text-align: center; font-size: 11pt; margin: 0 0 4mm; }
table { border-collapse: collapse; width: 100%; font-size: 8.8pt; } th, td { border: 1px solid #ddd; padding: 1.4mm 2mm; } th { background: #f2f2f4; }
```

## Follow-up questions in chat
When the user asks about one slide (e.g. "examples of Narrow vs General AI", "explain X vs Y"), answer in chat:
- Start with 1–2 "in simple words" sentences.
- Comparison table mapped to the slide's own rows/criteria.
- Concrete everyday examples (Malaysia-relevant where natural, e.g. Grab, Shopee, Touch 'n Go).
- Link back to lecture slide numbers.
- Explain any professional term used.
- End with a one-sentence exam answer, a memory trick, and close-book keywords.
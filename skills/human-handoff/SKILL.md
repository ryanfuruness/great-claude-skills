---
name: human-handoff
description: Use before handing an AI-generated document, deck, page or report to people. Read it as a person would, front to back, and reshape it into a form they can take in.
---

# Handing AI-generated work to human readers

When an AI produces a document, deck or report, the content is usually all there; what fails is the form. The gap has one root: an AI reads differently from a person. It takes in a whole artifact at once, holds every page and the entire history of the work that produced it in view at the same time, and has room for far more detail than a person can carry. A person reads once, front to back, knowing only what the pages so far have told them, with attention for a few points per page. Almost everything that makes AI-made work hard for people comes from writing and checking it from the first position for a reader in the second. This skill is the pass that closes that gap. It assumes the content is right and changes only how the content reaches the reader.

## Read it as a person would, in order

The default check fails for a specific reason. When the author rereads, every reference resolves, because the author holds the whole thing: a term used on one page and explained four pages later feels explained. The same is true of any check that takes in the whole artifact at once. So simulate a first-time reader deliberately. Go through the artifact in order and keep an explicit running list of what the reader has been told so far: every term, name, number, label and claim, added at the point where it is introduced. At each use, check the list. Anything used before it is on the list is a gap, however obvious it feels.

The list catches three common forms of the same problem:

- **Arriving before being introduced.** A name, concept or conclusion is used ahead of the part that sets it up, usually because the author knew it was coming.
- **References that only resolve with the full context.** Phrases like "the hardest part", "the two options" or "the second round" point at something the author holds and the reader was never shown. The first mention of anything significant must carry its own why, what it is and why it is here, at that point and not later.
- **The working vocabulary of the making.** Labels, codes, abbreviations, stage numbers, file names and shorthand coined while doing the work read as ordinary words to the author and as noise or puzzles to the reader. Replace them with plain words, or define them where they first appear. The same goes for any trace of how the work was made, such as earlier drafts, corrections or what was tried first, which belongs to the author's history and not the reader's.

The fix is rarely a footnote. Usually it means moving an introduction earlier, adding the clause that says what something is, or reordering so that each part sets up the next.

## Cut to what a person can carry

The second symptom of the same root is bandwidth. An AI does not feel density, so it keeps everything it knows and gives it all equal weight. A person reading a page takes away one or two things; if the page offers ten at the same volume, they take away whichever caught their eye. Give each page or section one job at one altitude. An overview page should not also carry the mechanism, and a page of evidence should not also draw the conclusion. Then read only the headings, in order: as claims rather than topic labels, they should tell the whole story at the top altitude.

An AI's default way of delivering also adds structure that people read as meaning. Equal-sized blocks imply equal importance, bold implies emphasis, a box implies a separate thing, and a colour or symbol implies a category. When these are used for symmetry or decoration, the reader decodes meaning that is not there. So every element should be something the reader would miss if it were cut, and every encoding (colour, symbol, weight, term) should mean one thing across the whole artifact. Repetition, legends nothing uses, rows of numbers that show effort rather than inform, and hedges or restated summaries all go.

## Keep the substance: the escape hatch

Cutting to a size a person can take in can remove detail that matters: the evidence a skeptical reader may want, the full table behind a summary, the reasoning behind a judgement call. When a cut would lose substance, or when you find yourself shrinking type or compressing sentences to make things fit, move the detail instead of deleting it. Put it in an appendix at the back, or in a sidecar document such as a markdown file alongside the main artifact, and leave a one-line pointer where it was. The main artifact carries what the reader needs to follow and decide; the appendix or sidecar carries everything else for whoever wants it.

## Look at what the reader will see

People see the rendered output, not the source. Overlaps, awkward wraps, a single word stranded on its own line, uneven spacing and inconsistencies between pages exist only in the render. Look at every page as it renders, then at all the pages side by side, where drift in styles and encodings shows. On each page, ask where the eye lands first and whether that is the point of the page. Visual quality is not decoration: readers judge care from it before they read a word, and a clean page lets the content through.

## Get eyes that don't share your context

Even with deliberate effort, the author cannot fully unknow what it knows. Fresh reviewers, as subagents where they are available, can do this, provided they are set up to read the way a person does. Give them only the rendered artifact and one line on who will read it. Do not give them the brief, the sources, drafts or how the work was made: that history is exactly the bias being removed.

- **A first-time reader.** A reviewer handed the whole artifact will read it the way an AI does, all at once, and fill early gaps from later pages; this is how a confusing first mention survives review. So hand the pages over one at a time, as separate files, and ask the reviewer to write after each page, before opening the next: what it took away, what it had to take on trust, and anything that appeared without introduction. At the end, ask where it got lost and what it would ask the author.
- **A visual pass.** Defects, consistency of encodings across pages, noise that could be cut, and whether each page's hierarchy points at its point.

Treat a reviewer's confusion as the finding, even when its suggested fix is wrong: the place a fresh reader stumbles is where a real reader will. After substantial changes, run the first-time reader again, because restructuring moves introductions around and creates new gaps.

Scale this to the artifact. Separate reviewers suit a deck or report that someone will act on. For a short document, do the in-order read with the running list yourself, then look at the rendered result.

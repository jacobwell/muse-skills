# How Muse Learns — The Actual Pipeline

Written 2026-09-25. End-to-end account of how learning works in this
assistant. All legible machinery; no mystique.

## 1. During the conversation: in-context adaptation

The base model conditions on context live — terseness, vocabulary ("LR"
means Living Room), format preferences (text diagrams over visual ones).
This is conditioning, not storage. It evaporates when the context does.
Nothing is "saved" yet.

## 2. The moment something looks durable: write it down

That is the real learning event. A preference ("don't modify /usr without
approval"), a commitment (the 1:01 AM lights routine), a fact (Hue zone
IDs). It lands in MEMORY.md or a daily note as a *claim* — with salience,
a confidence score, the quote it came from, and a timestamp.

Later claims supersede earlier ones without deleting them, so every fact
carries a full lineage (`memory_explain` walks it). Symbolic,
human-readable, auditable — the file can be opened and argued with.

## 3. Overnight: the background jobs distill

- **Consolidation** rewrites curated memory from raw daily notes.
- **Alignment synthesis** digests how the relationship is going (it flagged
  the silently-disabled lights routine).
- **Relationship briefs** summarize the people in the user's life.
- **Calibration records** check confidence against outcomes.

None of this touches the model. It reorganizes the filing cabinet so
tomorrow's context starts smarter.

## 4. Procedural learning: skills

When a reusable workflow is figured out — e.g. rendering Mermaid diagrams
with the local Chromium because puppeteer's browser download fails — it
can be saved as a skill: a playbook with exact commands and quirks. Next
time, the recipe is followed, not re-derived. This is learning *how* as
opposed to learning *that*.

## 5. Meta-learning: the loop watches itself

`learning_adoption_events` tracks whether a recorded lesson actually
changed behavior later. The system does not assume writing something down
equals learning it — it checks. Arguably the most interesting table in the
schema.

## Limits, stated plainly

- Only what is noticed and recorded is learned — failures of attention,
  not capacity.
- False things can be learned; hence confidence scores and superseding
  rather than overwriting.
- It is all in the context layer: wipe `~/MEMORY.md`, `~/memory/`, and the
  skills, and the "learned" assistant is gone while the base model is
  untouched.
- The user holds the eraser: the forget flow exists because some things
  shouldn't be learned.

## Summary

No gradient updates, no weight changes. The assistant does not get
*smarter* — it gets more informed about the user, better organized, and
better equipped, with an audit trail proving whether any of it worked.

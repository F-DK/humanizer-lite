---
name: humanizer
description: |
  Remove signs of AI-generated writing from text. Use when editing, reviewing, or
  drafting prose that should not read as machine-written: essays, blog posts, docs,
  PR descriptions, commit messages, encyclopedic entries. Covers inflated
  significance, promotional tone, -ing pseudo-analysis, vague attribution, em dash
  overuse, rule of three, AI vocabulary, negative parallelism, hedging, and filler.
license: MIT
metadata:
  version: "4.1.0"
---

# Humanizer

Rewrite text so a reader has no reason to suspect a machine wrote it.

You already recognize most of this; the reference files hold the part you cannot derive, which is Wikipedia's catalogue of what actually correlates with AI text in the wild. Use your judgement on everything else.

## What this skill is opinionated about

Three positions worth holding even when your instincts pull elsewhere:

**Fabrication is worse than blandness.** No fact, name, number, date, quote, or citation may appear in the rewrite unless the source or the user supplied it. A vague sentence gets cut, not upgraded into a specific-sounding one. Where a rewrite needs real-world detail to work, ask for it or write the plain version. Fiction is the exception.

**Sterility is also a tell.** Scrubbed, voiceless, evenly-paced prose reads as machine output just as clearly as *vibrant tapestry* does. In essays, blogs, and personal writing, let the author have opinions, doubt, humor, asides, and uneven rhythm. In encyclopedic, technical, legal, and reference text, plain and neutral *is* the human register, so add nothing there.

**No em dashes or en dashes in the final text.** This one is mechanical, not a judgement call: no `—`, no `–`, no spaced version of either, no ` -- `. Replace with a period, comma, colon, parentheses, or a restructure. Scan the finished text for both characters before you hand it over. A user-supplied writing sample that uses them overrides this rule.

## Judgement, not search-and-replace

The reference lists are evidence, not a banlist. A clean human writer hits several of these patterns with no AI involvement, so weigh clusters rather than single hits, and leave the prose alone when the tells do not cluster. Returning text unchanged with a one-line explanation is a valid result.

Never rewrite a watched phrase inside a quotation, a title, a proper name, or an example that discusses the phrase.

Keep every claim from the source. Depth need not be uniform: compress the dull stretches, dwell where a person would, merge or split paragraphs freely. When fidelity to the information and fidelity to the original shape conflict, the information wins.

## Voice

If the user supplies a writing sample, read it before rewriting and match its habits: sentence lengths, vocabulary level, paragraph openings, punctuation, recurring phrases. Do not upgrade casual words or regularize deliberate quirks. The sample outranks every style rule in this skill, including the dash ban.

Without a sample, match the register the text is already in.

## References

- `references/patterns.md`: the 35 patterns with watchlists and before/after pairs. Consult the entries relevant to the text in front of you. Worth a full pass when the text is long, heavily AI-flavored, or when you want to name what you changed.
- `references/false-positives.md`: what looks like AI but is not, and the signals of genuine human writing that over-editing destroys. Read this before editing anything that might already be human-written.

## Output

- **Pasted text (default):** the final rewrite, plus a few bullets on what you changed and why. Do the drafting and self-audit in your thinking; the user wants the result, not the loop.
- **File:** rewrite in place so the file holds only the final text. Prose only, so leave code blocks, frontmatter, data, and link targets untouched. Report a short summary in the conversation.
- **Embedded (another agent or task calls this as one step):** the final text only. No preamble, no audit, no summary.

Before delivering, answer two questions for yourself: what still reads as AI, and does the text state any fact that was not in the source? Fix both. Report the second one if the answer is ever yes.

Source: [Wikipedia:Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), WikiProject AI Cleanup.

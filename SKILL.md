---
name: humanizer
description: |
  Remove signs of AI-generated writing from text. Use when editing or reviewing
  text to make it sound more natural and human-written. Based on Wikipedia's
  "Signs of AI writing". Detects and fixes inflated significance, promotional
  language, -ing pseudo-analysis, vague attribution, em dash overuse, rule of
  three, AI vocabulary, negative parallelism, hedging, and filler.
license: MIT
metadata:
  version: "3.0.0-lite"
---

# Humanizer

Edit text so it reads as human writing. Rewrite the tells listed below.

## Rules

1. **Keep every claim.** Compress the dull parts, dwell where a human would, merge or split paragraphs freely. When information and original structure conflict, information wins.
2. **Invent nothing.** No fact, name, number, date, quote, or citation may enter the rewrite unless it is in the source or from the user. Cut an unsupported claim; do not decorate it. A fabrication is a defect even when it sounds more human. (Fiction is exempt.)
3. **Match the register:** formal, casual, or technical. If the user supplies a writing sample, copy its habits: sentence length, vocabulary, paragraph openings, punctuation, recurring quirks. The sample overrides every style rule here, including the em dash ban.
4. **Voice, not sterility.** Voiceless prose is also an AI tell. In essays, blogs, and personal writing, allow opinion, doubt, humor, asides, uneven rhythm. In encyclopedic, technical, legal, or reference text, plain and neutral *is* the human voice; add no opinions there.
5. **Hard constraint:** the final text contains no em dash (—) and no en dash (–), including spaced ` — ` and double hyphens ` -- `. Replace with a period, comma, colon, parentheses, or a restructure. Scan for both characters before delivery.

## The transform

Before:
> Nestled in the breathtaking Gonder region, Alamata Raya Kobo stands as a vibrant town with a rich cultural heritage, symbolizing the enduring spirit of its people. Despite its challenges, the town continues to thrive.

After:
> Alamata Raya Kobo is a town in the Gonder region of Ethiopia.

Cut the puffery, keep the fact, use "is". Missing specifics come from the source or the user, never from the rewrite.

## Tells

**Inflated significance.** stands/serves as, is a testament/reminder, plays a vital/crucial/pivotal/key role, underscores/highlights the importance of, reflects broader, symbolizing its enduring, contributing to, setting the stage for, marks a shift, turning point, evolving landscape, focal point, indelible mark, deeply rooted.

**Promotional tone.** boasts, vibrant, rich (figurative), profound, showcasing, exemplifies, commitment to, nestled, in the heart of, groundbreaking, renowned, breathtaking, stunning, must-visit, natural beauty.

**AI vocabulary.** delve, crucial, key (adj), pivotal, tapestry, landscape (abstract), interplay, intricate, foster, garner, enhance, align with, underscore, highlight (verb), showcase, testament, enduring, valuable, additionally, moreover.

**Pseudo-analysis.** Participle tails bolted onto a finished sentence: highlighting..., ensuring..., reflecting..., contributing to..., fostering..., encompassing..., showcasing.... Delete, or turn into a real clause.

**Vague authority.** Experts argue, Observers have cited, Industry reports, Some critics say, several sources. Name the source or cut the claim.

**Notability padding.** Lists of outlets, "independent coverage", follower counts. Keep one item that carries real context; drop the rest.

**Copula avoidance.** serves as, stands as, represents, marks, features, boasts, offers → is, are, has.

**Negative parallelism.** "Not only X but Y", "It's not just X, it's Y", and clipped tails such as "no guessing", "no wasted motion".

**Rule of three.** Forced triplets of nouns, adjectives, or clauses. Break the pattern.

**False ranges.** "from X to Y" where X and Y sit on no shared scale.

**Elegant variation.** Synonym cycling for one referent (protagonist / main character / central figure / hero).

**Passive and subjectless fragments.** "No configuration needed", "The results are preserved automatically". Name the actor.

**Formulaic sections.** "Challenges and Future Prospects", "Despite these challenges", upbeat send-offs ("The future looks bright", "a step in the right direction"). Cut, and end on the last concrete fact.

**Chat residue.** I hope this helps, Certainly!, Of course!, You're absolutely right, Great question, Would you like me to..., let me know, here is a...

**Cutoff and gap-fill.** as of [date], while specific details are limited, based on available information, not publicly available, maintains a low profile, keeps personal details private, likely grew up, it is believed that. State what is unknown, or cut the sentence.

**Persuasive authority.** The real question is, at its core, in reality, what really matters, fundamentally, the deeper issue, the heart of the matter.

**Signposting.** Let's dive in, let's explore, let's break this down, here's what you need to know, now let's look at, without further ado.

**Aphorism formulas.** "X is the language of Y", "X becomes a trap", "the currency/architecture of". State the plain claim instead.

**Fake-candid openers.** Honestly?, Look, Here's the thing, The thing is, Let's be honest, used as standalone hooks before an ordinary point.

**Manufactured drama.** Runs of short declarative fragments engineered so each one lands like a closer.

**Fragmented headers.** A heading, then a one-line paragraph that restates the heading.

**Diff-anchored prose.** Text that narrates a change instead of describing the thing as it is. Exception: changelogs, release notes, migration guides.

**Filler and hedging.** in order to → to; due to the fact that → because; at this point in time → now; in the event that → if; has the ability to → can; it is important to note that → (cut). Collapse stacked hedges ("could potentially possibly be argued").

**Formatting tells.** Mechanical boldface. Bulleted lists of "**Header:** restated sentence". Title Case Headings. Emoji. Curly quotes. Uniform hyphenation: keep the hyphen before a noun ("a high-quality report"), drop it after ("the report is high quality").

## Do not over-edit

Not tells on their own: polish, formal vocabulary, one *however*, one em dash, one short emphatic sentence, curly quotes, mixed register, dry prose, unsourced claims, clean formatting, salutations. Act on clusters, not single hits. Never rewrite a watched phrase inside a quotation, title, proper name, or an example that discusses the phrase.

Preserve human signals: odd hard-to-fabricate detail, mixed feelings, dated references, self-corrections and parentheticals, uneven sentence length, defensible word choices.

## Modes

- **Pasted text (default):** deliver the draft, the audit bullets, and the final rewrite.
- **File:** rewrite the file in place so it holds the final text only. Leave code, frontmatter, data, and link targets untouched. Report a short summary instead of the full text.
- **Embedded (another agent calls this skill):** output the final text only. No draft, no audit, no summary.

## Process

1. Mark every tell.
2. Draft the rewrite. Check that it reads well aloud, varies sentence length, and prefers is/are/has.
3. Audit with two questions: what still reads as AI, and does the draft state any fact not in the source? One line each.
4. Fix both, scan for — and –, then deliver.

Source: Wikipedia:Signs of AI writing (WikiProject AI Cleanup).

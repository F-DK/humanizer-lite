# Humanizer Lite

A portable agent skill that removes signs of AI-generated writing from text. It is plain Markdown, so it runs in any harness that supports skill-style instructions.

This is a fork of [blader/humanizer](https://github.com/blader/humanizer) that splits the skill in two: a short `SKILL.md` the agent always loads, and a `references/` folder it opens only when the text calls for it. Same patterns, same Wikipedia source, a much smaller prompt per invocation.

It installs under the skill name `humanizer`, the same name upstream uses. Remove any existing humanizer install first, or you will have two copies competing.

## Installation

### Skills CLI

Install globally with the cross-agent skills CLI so Humanizer is available in every project:

```bash
npx skills add f-dk/humanizer-lite --global
```

Update an existing install:

```bash
npx skills update humanizer --global
```

To install globally into every supported agent harness:

```bash
npx skills add f-dk/humanizer-lite --global --agent '*'
```

To target one configured harness, pass its agent name:

```bash
npx skills add f-dk/humanizer-lite --global --agent <agent-name>
```

Omit `--global` for a project-local install that can be committed and shared with collaborators. Start a new agent session or reload skills after installation.

### Claude Code plugin

Claude Code users can also install Humanizer as a plugin:

```
/plugin marketplace add F-DK/humanizer-lite
/plugin install humanizer@humanizer-lite
```

The skill is then invoked as `/humanizer:humanizer`.

### Claude Cowork (and Claude apps)

Cowork loads a skill from a ZIP whose root is the skill folder. Build one:

```bash
mkdir -p dist/humanizer-lite && cp -R SKILL.md references dist/humanizer-lite/
sed -i '' 's/^name: humanizer$/name: humanizer-lite/' dist/humanizer-lite/SKILL.md
(cd dist && zip -qr humanizer-lite.zip humanizer-lite) && rm -rf dist/humanizer-lite
```

Then upload `dist/humanizer-lite.zip` in Claude under **Customize > Skills > Add**. The rename keeps it distinct from an installed upstream `humanizer`; drop the `sed` line if you don't have one. On Linux use `sed -i` without the `''`.

### Manual

Any agent harness can use the skill directly because the runtime artifacts are plain Markdown: `SKILL.md` plus the `references/` folder it loads on demand. Install them wherever your harness expects skill directories.

For example:

```bash
git clone https://github.com/F-DK/humanizer-lite.git /path/to/your/skills/humanizer
```

Or, if you already have this repo cloned:

```bash
mkdir -p /path/to/your/skills/humanizer
cp -R SKILL.md references /path/to/your/skills/humanizer/
```

## Usage

Invoke the skill however your agent harness exposes installed skills. Common forms include a slash command or a direct request:

```
/humanizer

[paste your text here]
```

```
Please humanize this text: [your text]
```

Point it at a file and the skill rewrites it in place:

```
Humanize the prose in docs/launch-post.md
```

### Voice calibration

To match your personal writing style, provide a sample of your own writing:

```
/humanizer

Here's a sample of my writing for voice matching:
[paste 2-3 paragraphs of your own writing]

Now humanize this text:
[paste AI text to humanize]
```

The skill will analyze your sentence rhythm, word choices, and quirks, then apply them to the rewrite instead of producing generic "clean" output.

## Overview

Based on [Wikipedia's "Signs of AI writing"](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), maintained by WikiProject AI Cleanup. The guide is compiled from cleanup work on AI-generated text across Wikipedia.

`SKILL.md` stays short so it costs little to keep loaded: it carries the rules the agent cannot derive on its own, then pulls in `references/patterns.md` (the 36 patterns with watchlists and before/after pairs) and `references/false-positives.md` (what looks like AI but is not) only when the text in front of it needs them.

The skill also runs a self-audit before delivering, to catch lingering AI-isms and any fact the rewrite invented.

Rewrites follow a no-fabrication rule: they never add facts, names, dates, or citations that aren't in the source text. Specificity has to come from the source or the author, not from the rewrite.

### Key insight from Wikipedia

> "LLMs use statistical algorithms to guess what should come next. The result tends toward the most statistically likely result that applies to the widest variety of cases."

## The 36 patterns

Grouped as in [`references/patterns.md`](references/patterns.md), where each entry has its full watchlist.

### Content patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 1 | **Significance inflation** | "marking a pivotal moment in the evolution of..." | "was established in 1989 as part of a wider decentralization" |
| 2 | **Notability name-dropping** | "cited in NYT, BBC, FT, and The Hindu" | Trim the list; keep only sourced context |
| 3 | **Superficial -ing analyses** | "symbolizing... reflecting... showcasing..." | Remove, or keep only what the source supports |
| 4 | **Promotional language** | "nestled within the breathtaking region" | "is a town in the Gonder region" |
| 5 | **Vague attributions** | "Experts believe it plays a crucial role" | Name a real source or cut the claim |
| 6 | **Formulaic challenges** | "Despite challenges... continues to thrive" | Keep the sourced facts; cut the boosterism |

### Language and grammar patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 7 | **AI vocabulary** | "Actually... additionally... testament... landscape... showcasing" | "also... remain common" |
| 8 | **Copula avoidance** | "serves as... features... boasts" | "is... has" |
| 9 | **Negative parallelisms / tailing negations** | "It's not just X, it's Y", "..., no guessing" | State the point directly |
| 10 | **Rule of three** | "innovation, inspiration, and insights" | Use natural number of items |
| 11 | **Synonym cycling / repeated openings** | "protagonist... main character... central figure... hero" | "protagonist" (repeat when clearest) |
| 12 | **False ranges** | "from the Big Bang to dark matter" | List topics directly |
| 13 | **Passive voice / subjectless fragments** | "No configuration file needed" | Name the actor when it helps clarity |

### Style patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 14 | **Em/en dashes** | "institutions—not the people—yet this continues—" | Cut them: periods, commas, colons, or parentheses |
| 15 | **Boldface overuse** | "**OKRs**, **KPIs**, **BMC**" | "OKRs, KPIs, BMC" |
| 16 | **Inline-header lists** | "**Performance:** Performance improved" | Convert to prose |
| 17 | **Title Case Headings** | "Strategic Negotiations And Partnerships" | "Strategic negotiations and partnerships" |
| 18 | **Emojis** | "🚀 Launch Phase: 💡 Key Insight:" | Remove emojis |
| 19 | **Curly quotes** | `said “the project”` | `said "the project"` |

### Communication patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 20 | **Chatbot artifacts** | "I hope this helps! Let me know if..." | Remove entirely |
| 21 | **Cutoff disclaimers** | "While details are limited in available sources..." | Find sources or remove |
| 22 | **Sycophantic tone** | "Great question! You're absolutely right!" | Respond directly |

### Filler and hedging

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 23 | **Filler phrases** | "In order to", "Due to the fact that" | "To", "Because" |
| 24 | **Excessive hedging** | "could potentially possibly" | "may" |
| 25 | **Generic conclusions** | "The future looks bright" | Specific plans or facts |
| 26 | **Hyphenated word pairs** | "cross-functional, data-driven, client-facing" | Drop hyphens on common word pairs |

### Rhetoric and cadence

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 27 | **Persuasive authority tropes** | "At its core, what matters is..." | State the point directly |
| 28 | **Signposting announcements** | "Let's dive in", "Here's what you need to know" | Start with the content |
| 29 | **Fragmented headers** | "## Performance" + "Speed matters." | Let the heading do the work |
| 30 | **Diff-anchored writing** | "This function was added to replace..." | Describe what it does, not what changed |
| 31 | **Manufactured punchlines / staccato drama** | "It had no preference. No prior. No nostalgia." | Use varied sentence lengths and concrete claims |
| 32 | **Aphorism formulas** | "Symmetry is the language of trust" | Replace the formula with the actual claim |
| 33 | **Conversational rhetorical openers** | "Honestly? It depends..." | Remove the fake-candid setup |
| 34 | **Shadowboxing** | "I'm not saying documentation doesn't matter..." | Cut the defense; state the real claim |
| 35 | **Rejecting fake alternatives** | "A tempting approach would be... but" | Cut the option nobody would pick |
| 36 | **Hinge colons** | "If you're coming from X: instead of Y, you Z" | Drop the setup; state the point |

## Full example

*(Illustration note: the rewrite below adds specifics, like the month and the neighborhoods, that stand in for details the author would supply. In a real session those come from the user; the skill asks rather than invents.)*

**Before (AI-sounding):**
> I recently spent five unforgettable days in Lisbon, and let me tell you — this city completely stole my heart. From the moment I arrived, I knew I was somewhere truly special.
>
> Nestled along the banks of the Tagus River, Lisbon stands as a vibrant testament to Portugal's enduring spirit, where rich history and modern energy intertwine at every turn. Yes, the famous hills are challenging — my legs certainly felt it! — but every climb rewards you with breathtaking, panoramic views that make it all worthwhile.
>
> No trip would be complete without riding the iconic Tram 28, winding through the city's most historic neighborhoods. And the food? Simply divine. The original pastéis de nata at Pastéis de Belém are a beloved national treasure, and savoring one still warm was a moment I will never forget.
>
> But what truly makes Lisbon special isn't just the sights — it's the feeling. Wander a few steps off the main squares and you'll discover a quieter, more authentic side: sun-drenched alleys, charming tiled facades, and friendly locals going about their daily lives, inviting you to slow down and savor every moment.
>
> Of course, the must-see São Jorge Castle offers stunning views over the rooftops below. The queues can be long, but the experience is absolutely worth it for any history buff or curious traveler.
>
> Would I go back? Absolutely. Lisbon isn't just a place to visit — it's a place to fall in love with, again and again. If you're dreaming of your next getaway, this is one destination that promises memories to last a lifetime. ✨

**After (Humanized):**
> I spent five days in Lisbon last October and still have mixed feelings about it. Beautiful, yes. Also harder on the knees than anyone warned me.
>
> The hills are the whole story and somehow never make the brochures. My hotel was up in Alfama, which photographs beautifully and translates, in practice, to climbing what felt like a six-story staircase every time I wanted coffee. By the second day my calves had opinions. I started planning each walk around which way was downhill, which is a strange way to see a city but probably an honest one.
>
> Everyone says to ride Tram 28, so I did, wedged against a stranger's backpack for forty minutes while three tour groups filmed the same corner. I would walk the route next time, or go before breakfast. The custard tarts, though, earn the fuss. I had one at a plain little place in Graça, still warm, and for about thirty seconds I understood why people build trips around pastry.
>
> What I did not expect was how quiet the city gets away from the main squares. Two blocks uphill from any plaza it turns into laundry lines, chipped tile, open windows, and old men watching football with the sound turned up. That is the Lisbon I keep thinking about, not the castle.
>
> The castle is fine. The view is great, the queue is long, and I spent more time shuffling toward the entrance than looking at anything once I got inside. If I had only two days, I would trade it for an afternoon of getting lost.
>
> I would go back, but in spring and with better shoes. Lisbon does not bend over backward to make things easy for you. I think I liked that, even when my legs disagreed.

## References

- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) - Primary source
- [WikiProject AI Cleanup](https://en.wikipedia.org/wiki/Wikipedia:WikiProject_AI_Cleanup) - Maintaining organization

## Version history

- **4.2.0** - Reviewed Cursor's [unslop](https://github.com/cursor/plugins/blob/main/pstack/skills/unslop/SKILL.md) skill and folded in what it had that this one lacked, without growing the list where an existing entry could carry it. Added pattern #36 (colons used as mid-sentence hinges). Extended #7 with technical-sounding abstractions (substrate, wedge, vector, flywheel) and plain-word swaps (utilize, leverage, facilitate); extended #4 to feature-marketing prose in technical writing, with a portability test for sentences that would fit any project's docs; added a false-positive carve-out to #16 so legitimate bold lead-ins survive; noted in #14 that replacing every dash with parentheses swaps one tic for another. 36 patterns total.
- **4.1.0** - Synced with upstream v2.11.2: added patterns #34 (shadowboxing, defending against objections the text never raised) and #35 (rejecting fake alternatives, editorial scar tissue left in the final draft), extended #11 to repeated sentence openings, broadened #28 to casual announcements, added `quietly` and figurative `gate/gated/gating` to the #7 vocabulary list, and added the matching false-positive guards for real disclaimers, real alternatives, and deliberate repetition. Packaging: `plugin.json` now declares `"skills": ["./"]` so Claude Desktop discovers the plugin, and the validator reads files as UTF-8 so it works on Windows locales. 35 patterns total.
- **4.0.0** - Split the skill into a short `SKILL.md` and two on-demand references (`references/patterns.md`, `references/false-positives.md`), cutting the prompt an invocation loads from 412 lines (29.6 KB) to 57 lines (4.4 KB) while keeping the full catalogue available on demand. The prompt now states the three non-derivable rules (no fabrication, sterility is also a tell, no em/en dashes) and leaves the rest to judgement, with the pattern lists framed as evidence rather than a banlist. Forked from upstream 2.9.1; no change to the 33 patterns.

Entries below are inherited from upstream [blader/humanizer](https://github.com/blader/humanizer); this fork branched at 2.9.1.

- **2.9.1** - Improved distribution and portability: removed nonportable frontmatter and tool preapprovals, made global installation the documented default, added package validation, and removed the duplicated long-form example from the runtime prompt. No change to the 33 patterns.
- **2.9.0** - Added a no-fabrication rule: rewrites may not invent facts, names, dates, or citations not present in the source, and every example that modeled invented specifics was re-cut to use only source information (fixes #187). Replaced paragraph-count parity with an information-over-shape rule, made a user's voice sample outrank the em dash ban, and added invocation modes (pasted text / file / embedded). No change to the 33 patterns.
- **2.8.3** - Moved the skill version from the unsupported top-level frontmatter key to `metadata.version` for Agent Skills and Claude compatibility. No change to the 33 patterns.
- **2.8.2** - Replaced the full before/after example with a first-person Lisbon trip recap. The after now keeps the same topic, perspective, and rough length as the before while removing the AI tells without becoming clipped or slogan-like. No change to the 33 patterns.
- **2.8.1** - Added cross-agent installation docs, optional Claude Code plugin packaging, and a compact secondhand-text false-positive guard. No change to the 33 patterns.
- **2.8.0** - Added style/cadence patterns #31-33 for manufactured punchlines, aphorism formulas, and conversational rhetorical openers; expanded #20 to catch offer-to-continue chatbot closers. 33 patterns total.
- **2.7.0** - Added pattern #30 (diff-anchored writing); made em/en dashes a hard cut rather than "overuse"; expanded #21 to cover speculative gap-filling ("maintains a low profile"). 30 patterns total.
- **2.6.0** - Cleanup pass: consolidated the duplicated workflow sections, gated the personality guidance to content where voice is wanted, removed the model-fingerprinting subsection, and condensed the worked example. No change to the 29 patterns.
- **2.5.1** - Added a passive-voice / subjectless-fragment rule, raising the total to 29 patterns
- **2.5.0** - Added patterns for persuasive framing, signposting, and fragmented headers; expanded negative parallelisms to cover tailing negations; tightened wording around em dash overuse; fixed frontmatter wording to use "filler phrases"
- **2.4.0** - Added voice calibration: match the user's personal writing style from samples
- **2.3.0** - Added pattern #25: hyphenated word pair overuse
- **2.2.0** - Added a final "obviously AI generated" audit + second-pass rewrite prompts
- **2.1.1** - Fixed pattern #18 example (curly quotes vs straight quotes)
- **2.1.0** - Added before/after examples for all 24 patterns
- **2.0.0** - Complete rewrite based on raw Wikipedia article content
- **1.0.0** - Initial release

## License

MIT

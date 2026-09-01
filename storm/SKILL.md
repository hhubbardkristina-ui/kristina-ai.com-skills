---
name: storm
description: >-
  Multi-perspective deep research using a five-lens adaptation of Stanford's STORM methodology
  (Practitioner, Academic, Skeptic, Economist, Historian). Use this whenever asked to
  research anything — a topic, a claim, a market, a content or video idea, a tool, a statistic,
  a trend, or "look into X" — even if STORM or research isn't mentioned explicitly. Also use
  inside scheduled tasks that research content ideas. Produces a verified briefing written in
  plain language and visualised wherever possible, with a fast read on how the five perspectives
  agree or disagree, findings ranked by reliability, and citations checked against primary sources.
---

# STORM Research

An adaptation of Stanford's STORM methodology (Shao et al., NAACL 2024: "Assisting in Writing Wikipedia-like Articles From Scratch with Large Language Models"). The original insight: research done from a single perspective has blind spots it cannot see, so STORM discovers multiple perspectives and lets each one ask its own questions before anything gets synthesised. This skill fixes the perspectives as five standing lenses and adds an explicit contradiction map, and adversarial citation verification before anything is reported as fact.

Why this beats a single research pass: one prompt produces one angle, and the gaps stay invisible because nobody is looking for them. Five lenses looking at the same question independently surface disagreements, and the disagreements are usually where the interesting truth lives.

## Who this is written for

Write every brief for a reader who is sharp and decision-capable but NOT a technical AI specialist:

- Lead with the answer in plain English, THEN show the evidence. Never make the reader wade through methodology to reach the point.
- Define every specialist term the first time it appears, inline, in one clause. If a term can't be explained in a clause, give it a line in the "Terms used here" section.
- No naked statistics. Translate numbers into a plain-language comparison the reader can act on.
- Prefer a concrete example over an abstract statement of a finding.
- The test: could the reader read this once, explain it back, and act on it? If not, it isn't done.

## Visualise whenever possible

Whenever a finding has a visual shape, show it, don't just describe it:

- Anything scored, banded, or ranked → a colour-coded table using 🟢 / 🟡 / 🔴 (🟢 = go, 🟡 = caution, 🔴 = stop).
- A trade-off or spectrum → a simple left-to-right scale.
- A sequence or history → a short timeline.
- Two axes (e.g. value vs feasibility) → a 2×2 grid with each item placed in a quadrant.
- Where the environment can render it, also offer an interactive version, not only an inline table.

Put the visual next to "The short version" so the picture and the plain-English answer are read together.

## The five lenses

Each lens researches the SAME question independently, but hunts for different evidence and reads sources differently:

| Lens | Looks for | Prefers as sources | Characteristic question |
|---|---|---|---|
| 🧑‍💼 **Practitioner** | Real-world execution, what actually happens when people do this, frontline friction, practical takeaways | Case studies, practitioner writeups, forums and community threads, how-to postmortems | "What happens when someone actually tries this?" |
| 🎓 **Academic** | Rigorous data, studies, theory, definitions, effect sizes | Peer-reviewed papers, institutional research, large surveys, original datasets | "What does the strongest evidence actually show?" |
| 🤨 **Skeptic** | Weak evidence, hype, unstated assumptions, risks, failure modes, who benefits from the claim being believed | Critical analyses, debunkings, negative results, disclosures, methodology critiques | "What would have to be true for this to be wrong?" |
| 📈 **Economist** | Financial viability, ROI, costs, unit economics, productivity impact, who pays and who profits | Financial reports, pricing data, market analyses, benchmark studies | "Does the money actually work?" |
| 🏛️ **Historian** | Precedent: how similar things played out before, past technology adoptions, patterns and cycles | Historical analyses, retrospectives, adoption-curve studies, old coverage of similar moments | "When has something like this happened before, and how did it end?" |

## Process

### Step 1: Scope

Sharpen the research goal before deploying anything. In an interactive session, ask clarifying questions until the question is specific: what decision or output does this research feed, what would a useful answer look like, what's already known or assumed. In an automated run (scheduled task, subagent), derive the scope from the request, state the assumptions made, and proceed.

Write the scoped question down in one sentence. Every lens gets this same sentence.

### Step 2: Deploy the five lenses in parallel

Spawn five subagents in a single turn (one per lens) using the Agent tool. Each lens prompt contains: the scoped question, the lens identity and its characteristic question, its source preferences, and this required return format:

- 3 to 5 findings, each as: the claim, the evidence, the source (title + URL), a confidence grade (strong / moderate / weak), and why this lens finds it credible or suspicious
- 1 to 2 things this lens looked for and could NOT find (absence of evidence is a finding)
- The lens's one-paragraph verdict on the question

Budget guidance per lens: 3 to 5 sources, no more. Depth comes from the five angles, not from any single lens boiling the ocean.

If subagents are unavailable in the environment, run the five lenses sequentially as separate research passes, keeping their findings strictly separated until Step 3. Never let an earlier lens's findings steer a later lens's searching.

### Step 3: Map the contradictions

Lay the five sets of findings side by side and build the contradiction map: every point where two or more lenses disagree, plus points where one lens found something the others missed entirely. For each contradiction, judge which side has the stronger evidence (source quality, recency, independence, sample size) and record the verdict with reasoning. Do not smooth disagreements over: a contradiction with a verdict is more valuable than a false consensus.

### Step 4: Synthesise

Merge everything into one briefing using the report structure below. Every finding carries its lens attribution and confidence grade. Findings supported by multiple lenses independently rank highest; single-lens findings are marked as such; contradicted findings appear with the contradiction, never silently resolved.

Before writing, translate. The raw findings will be full of specialist language and statistics, the synthesis is where you convert them into the plain-language, visualised, answer-first brief described in "Who this is written for" and "Visualise whenever possible". Correct-but-unreadable is a failed brief.

### Step 5: Adversarial verification

Before finalising, verify every citation graded **strong** that appears in "The short version" or "The answer, explained", the sections the reader actually reads, not just whichever citations feel like the report's headline conclusions. (In a rich environment, spawn a verifier subagent; otherwise do it inline.) For each: open the source, confirm it actually says what the finding claims, and mark it **confirmed**, **corrected** (fix the finding), or **demoted** (evidence weaker than claimed).

A citation that has NOT been independently opened and confirmed may not be reported as "strong" in the delivered brief, cap it at "moderate" and say so ("lens-graded strong, not independently verified"). Strong is a claim about the evidence, not about how the finding read on the page. A report that says "we couldn't verify this" is doing its job; a report that quietly passes along an unchecked "strong" claim is not.

## Report structure

ALWAYS use this template. The applied answer comes FIRST; the research apparatus supports it, it does not lead.

```
# STORM Brief: [scoped question]
*Date | Mode: full / light | Lenses run: 5*

## The short version
[3–5 sentences, plain English, every specialist term defined inline. What's the answer,
what should the reader DO with it, and the one caveat that actually matters.]

## Picture of the answer
[The visual: a 🟢🟡🔴 colour-coded table, a 2×2 grid, a scale, or a timeline, whichever
fits the answer. Skip only if the answer genuinely has no visual shape. Where the
environment can render it, offer an interactive version too.]

## How the five perspectives line up
[3–5 sentences, plain language: what each lens brought, where they agreed, where they
clashed, and who won the clash. Name them in plain terms, the practitioner, the
academics, the skeptic, the economist, the historian, so the reader can see at a glance who is
saying what. This is the fast read; the detailed contradiction map further down is the
evidence for it.]

## The answer, explained
[The heart of the brief and its longest section, the criteria, framework, or
recommendation written as guidance the reader can act on, in plain language with concrete
examples. Define terms as they appear. Weave the evidence in as support ("this holds up
because…"), not as a wall of citations.]

## Terms used here
[Every specialist term, one line each: term, plain meaning. Omit if none were needed.]

## The evidence behind it (for when the reader wants to check the work)
[The findings ranked by reliability, lens attributions, confidence grades, source links.
Still rigorous; just no longer the headline.]
1. [Finding] — [lens attributions] — [confidence] — [source links]
   [One sentence: why this ranking]
...

## Where the experts disagree
[The contradiction map. Each row written as a sentence a non-specialist can follow.]
| Where lenses disagree | Positions | Verdict + why |

## What no lens could verify
[Claims that survived with weak or no evidence, explicitly listed, in plain English.]

## Sources
[Full list, grouped by lens.]
```

## Modes

**Full STORM** (default): all five steps, five parallel subagents, verification pass. Use for research feeding real decisions: strategy, client work, business model questions, anything that will be acted on or published.

**Light STORM**: when time or the environment is constrained (scheduled runs processing multiple ideas, quick checks), run all five lenses as one structured sequential pass (five short searches, one per lens, strictly lens-by-lens), keep the "How the five perspectives line up" read and the contradiction map, compress verification to spot-checking the top 2 citations, but the strong-claim cap in Step 5 still applies: anything not among those 2 spot-checks stays at "moderate" or lower. Say in the report header that light mode was used. Even in light mode, keep the brief plain-language and visualised, those are never the thing that gets cut.

## Honesty rules

- Never invent a citation, never cite a source that wasn't actually read
- Confidence grades reflect the evidence, not the vibe; a finding everyone repeats but nobody sources is weak, no matter how often it's repeated
- "Strong" in the delivered report means independently verified per Step 5, not just lens-graded, an unverified strong claim is a defect, same class as an invented citation
- If the five lenses converge suspiciously fast, say so: convergence can mean truth, or it can mean every source is echoing the same original claim (the Historian should hunt for the original)
- The Skeptic's findings get equal standing in the synthesis; a report where the Skeptic changed nothing probably wasn't listening to the Skeptic
- Accessibility is a hard requirement, not a nice-to-have: a brief that is correct but unreadable has failed. An unexplained specialist term is a defect on the same level as an invented citation.

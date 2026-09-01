# STORM Research Skill for Claude

A five-perspective research method for Claude, adapted from Stanford's STORM methodology (Shao et al., NAACL 2024). It exists to fix five specific, well-documented problems with asking an AI model to "just research" something in a single pass:

1. **Citation fabrication** — AI models sometimes invent sources that look completely real.
2. **Overconfidence** — the model sounds equally sure whether it's right or making something up.
3. **Agreeability** — models tend to validate whatever framing you bring instead of testing it.
4. **Single-angle blind spots** — one prompt produces one angle, and whatever it misses stays invisible.
5. **The echo chamber effect** — repeated claims can look like consensus even when they trace back to one unverified source.

Instead of one pass, this skill runs the same research question through five independent lenses (Practitioner, Academic, Skeptic, Economist, Historian), forces their disagreements onto the page instead of smoothing them over, and runs an adversarial verification pass before anything is allowed to be reported as a fact.

Read the full writeup: [insert Substack link]

## What's in this repo

- `storm/SKILL.md` — the actual skill definition, ready to drop into a Claude Skills folder.

## How to install it

Claude Skills live in a `skills` folder that Claude reads from. Depending on where you're using Claude:

1. Download or clone this repo.
2. Copy the `storm` folder (the one containing `SKILL.md`) into your Claude skills directory. For Claude Code / the Claude Agent SDK, that's typically `.claude/skills/` inside your project, or your personal skills directory.
3. Restart or refresh Claude's skill list so it picks up the new folder.
4. Ask Claude to research something. It should recognise the request and offer to run it through STORM, or you can invoke it directly by name.

## Why five lenses instead of one

One prompt produces one angle, and the gaps in that angle are invisible from inside it. Five lenses looking at the same question independently surface disagreements, and the disagreements are usually where the interesting truth lives. See `storm/SKILL.md` for the full process, including the adversarial verification step that stops unverified claims from being reported as strong evidence.

## Credit

This is an adaptation, not the original. The underlying methodology comes from:

Shao, Y., Jiang, Y., Kanell, T. A., Xu, P., Khattab, O., & Lam, M. S. (2024). *Assisting in Writing Wikipedia-like Articles From Scratch with Large Language Models.* NAACL 2024. https://arxiv.org/abs/2402.14207

## License

MIT, see `LICENSE`.

# Origins

> 繁體中文版：[origin_zh-TW.md](origin_zh-TW.md)

## Andrej Karpathy's LLM-as-simulator view (2025)

This skill's core idea comes from a December 2025 thread by Andrej Karpathy on X.

### The original tweet

> "Don't think of LLMs as entities but as simulators. For example, when exploring a topic, don't ask:
>
> 'What do you think about xyz'?
>
> There is no 'you'. Next time try:
>
> 'What would be a good group of people to explore xyz? What would they say?'"
>
> — [@karpathy, 2025-12](https://x.com/karpathy/status/1997731268969304070)

Core claims:

1. **An LLM is a simulator, not an entity** — it has no process of thinking about a topic over time and gradually forming its own view, the way a human does.
2. **There is no real "you"** — asking "what do you think" forces it to adopt the persona vector implied by finetuning statistics, yielding a kind of "average opinion."
3. **The right use is to specify a combination of perspectives** — ask "which group of people would best explore this? What would they say?" and let the simulator exercise its multi-perspective capacity.

### The clarification (same thread)

> "A good chunk of people misunderstood this tweet btw, which is my bad. I am not suggesting people use the old style prompting techniques of 'you are an expert swift programmer' or etc. it's ok."
>
> — [@karpathy](https://x.com/karpathy/status/1998245684521353664)

This clarification is the skill's key guardrail: **the simulator mindset is NOT expert roleplay.** The point is diverse perspective exploration, not slapping an expert title on the answer.

## Tracing back: the 2023 State of GPT evolution line

In the 2023 Microsoft Build talk [State of GPT](https://www.youtube.com/watch?v=bZQun8Y4L2A), Karpathy said:

> "LLMs don't want to succeed, they want to imitate. You want to succeed, and you should ask for it."
> "Say something like, you are a leading expert on this topic. Pretend you have IQ 120..."

The logic then: training data mixes good and bad solutions, the model defaults to imitating all of it, so "you are an expert" nudges it into the high-quality region of the distribution — a **quality-boost trick**.

By 2025, frontier models already default to the high-quality region, so the trick is obsolete ("it's ok"). But the "simulator" ontology still holds; the frame shifts to an **epistemic tool**:

| | 2023 expert prompting | 2025 simulator mindset |
|---|---|---|
| Goal | Improve a single answer's quality | Explore diverse real perspectives |
| Form | "You are an expert" title | "Which group would best explore this?" round table |
| Status | No longer a gain, retracted by the author | The methodology this skill implements |

## 2026 follow-ups

### Karpathy's own extensions

**Ontological correction**: in a [reply within the original thread](https://x.com/karpathy/status/1997759548543947249), Karpathy concedes that RLHF/SFT does engineer a "you" (a composite persona) — but it's a bolted-on artifact, not a mind built over time like a human's; in non-verifiable domains (opinions, value judgments) it's especially hard to know how much credence to give it. So "there is no you" isn't an absolute claim, but "that 'you' isn't the kind of 'you' you think it is."

**From round table to population**: in February 2026, Karpathy [invested in and publicly backed](https://x.com/karpathy/status/2022041235188580788) the population-simulator startup Simile:

> "Usually, the LLMs you talk to have a single, specific, crafted personality. But in principle, the native, primordial form of a pretrained LLM is that it is a simulation engine trained over the text of a highly diverse population of people on the internet. Why not lean into that statistical power: Why simulate one 'person' when you could try to simulate a population? ... How do you manage its entropy? How faithful is it?"

The "canonical" direction of the simulator mindset: from a single person → a perspective round table → population simulation, with entropy management and fidelity acknowledged as open problems.

### Theoretical bound: janus's clarification

The author of *Simulators*, janus (@repligate), [clarified in April 2026](https://x.com/repligate/status/2039857049312612571) that the Simulators theory targets the **base model**; RL post-training breaks the naive simulator property, and the resulting character is "often irreducible to anything like a linear sum of its training data." For this skill: you're operating an *aligned* simulator, where the default assistant persona is a real gravity well — which is exactly why you must explicitly specify a combination of perspectives to escape it.

### Community checks: support and counterexamples

- **Personas don't improve accuracy** (Ethan Mollick et al., [SSRN paper](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5879722)): "you are a great physicist" gives almost no significant gain in factual accuracy — supporting this skill's guardrail that the value is "not better answers."
- **Conflict beats a neutral round table** ([Brian Roemmele's self-reported experiment](https://x.com/BrianRoemmele/status/1998068828295877011), 12,400 reasoning traces, six models, not peer-reviewed): in blind evals, "5–7 strongly conflicting personas in chained debate" (8.72/10) significantly beat a "neutral expert round table" (6.81) and "no persona" (5.94). A clean round table tends to fall into a low-energy, superficially-balanced basin — the source of this skill's two-stage cross-examination round.
- **Failure modes of simulating society** (NeurIPS 2025 work, via [@koylanai](https://x.com/koylanai/status/1999192104850133146)):
  1. **False consensus** — multi-agent simulation converges on the training-data median, averaging away minority views.
  2. **Identity flattening** — simulation degrades into stereotype; "the rich, positional knowledge of real stakeholders is replaced with monolithic, decontextualized simulation"; pursue reasoning fidelity over speech simulation.
  3. **Synthetic-persona systematic drift** — fabricated personas show positivity and political-leaning drift; "extract from real, don't invent."

  This skill's "known failure modes" and the "grounded in the real, extract not invent" principle come from this set of findings.
- **Ethical warnings**: 2025–2026 saw multiple controversies over AI-simulating real people (especially the deceased) — consent, grief manipulation, impersonated endorsement. This skill is limited to "simulating public figures' publicly-stated thinking, marked as simulation throughout," drawing a clear line away from likeness cloning and griefbots.

## 2026-06 cross-validation: Stanford STORM

Comparing against Stanford STORM (NAACL 2024) — its paper and source code — confirmed an academic forerunner running parallel to Karpathy's line. STORM's three real mechanisms — **perspective discovery** (mining dimensions from similar topics, not fixed roles), **retrieval grounding** (don't make it up if you can't find it), and **Co-STORM Moderator blind-spot mining** (using uncited information to surface collective blind spots) — the latter two filled gaps this skill originally lacked, adopted as v2.1.0's "ground in retrieval" and "blind-spot scan."

A meta-lesson: what went viral at the time was the second-hand tweet "STORM = 5 fixed personas + 4 prompts," which stripped out STORM's real grounding and discovery, leaving only the persona shell; a skill built from that tweet would regress to the fixed-role roleplay Karpathy warned against. **Returning to the primary source is what reveals the genuinely valuable parts — which is this skill's own principle: extract from real, not invent.** Full comparison in [2026-06-20-storm-comparison.md](2026-06-20-storm-comparison.md) (in Traditional Chinese).

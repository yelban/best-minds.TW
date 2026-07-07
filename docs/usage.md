# Best Minds Skill — Usage Guide

> 繁體中文版：[usage_zh-TW.md](usage_zh-TW.md)

## What it does

This is a **thinking-methodology** skill that changes how the AI answers open-ended questions:

| The usual way | Best Minds |
|---|---|
| "What do you think?" | "Which group of people in the world would best explore this, and what would they say?" |
| The assistant persona's consensus answer | Real people's perspectives in collision + a converged synthesis |

Use it for open-ended judgment, design trade-offs, and strategy — questions with no single right answer. Closed factual questions are out of scope (just answer them directly).

## Triggers

Any of these in conversation will trigger it:

- best minds / who knows this best / panel of experts
- 最強大腦 / 頂級專家 / 世界級 / 誰最懂這個

## Examples

```
"How should an LLM agent's memory system be designed? Who knows this best?"
→ Convenes a perspective round table (e.g. Andrej Karpathy, Richard Sutton,
  Michael Stonebraker), simulates each as a distinct voice with its own claims
  and disagreements, then converges on a recommendation

"I want to quit and start a company — give me top-expert advice"
→ Might simulate Paul Graham plus a deliberately contrarian risk voice
  (e.g. Nassim Taleb), instead of one-sided encouragement
```

Panelists are always real people with a public record; a fictional archetype is used only when no suitable real person exists, and is labeled as such.

## How it works

The round table is not a black box — it's a verifiable pipeline:

1. **Map the axes** — first see what spectrum of positions the question spans, to avoid picking people who share one blind spot
2. **Pick & disclose** — list the panel: full name + a one-line bio + selection reason (e.g. "Charles Packer — first author of the MemGPT paper, founder of Letta"), and explicitly mark the **designated dissenter** — who plays contrarian, against which premise; if the combination is unclear, you'll be asked to pick first
3. **Ground in retrieval** — for contested / time-sensitive topics, the person's actual recent stance is retrieved before simulating (if not found, it's flagged as conjecture, not fabricated)
4. **Simulate the voices** — each voice is labeled by full name, presenting its own reasoning frame and specific claims (not tone mimicry); marked as simulation throughout
5. **Converge (with blind-spot scan)** — the ending states consensus, key disagreements, and a recommendation for your question; no "everyone has a point" balanced ending — when disagreement can't be reconciled, it says under which premises to listen to whom. It **always includes a blind-spot scan**: what the group collectively didn't discuss, what premise they all took for granted. Concrete recommendations the synthesizer adds (tool choices, number thresholds) are marked as synthesizer-added, not attributed to a panelist

For three or more voices, an optional two-stage parallel sub-agent round table runs *independent statements → citation cross-check → cross-examination → convergence*, where each voice can't see the others first (preventing mutual anchoring) and every claim marked *verified* is re-checked against its cited source by a separate verifier before other voices read it.

## Reply language

The canonical `SKILL.md` is written in English (for international reach and cross-language neutrality), but its opening rule mandates "respond in the language of the user's question." So a Chinese question gets a Chinese answer, an English question an English one — verified in a neutral environment across Chinese / English / Japanese. See [2026-06-20-language-ab-and-english-canonical.md](2026-06-20-language-ab-and-english-canonical.md) (in Traditional Chinese).

## Core value

It leverages the LLM's nature as a "simulator" — its training data contains many real people's books, talks, and interviews, so it can reasonably simulate their thinking frames. The value is in diverse perspectives, anti-sycophancy (simulated people criticize without restraint), locating expert disagreement, and mining collective blind spots — not in slapping an expert title on the answer.

> "Don't think of LLMs as entities but as simulators."
> — Andrej Karpathy

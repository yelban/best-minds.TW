---
name: best-minds
description: "Simulator mindset: don't ask the AI 'what do you think,' ask 'which group of people in the world would best explore this — and what would they say?' For open-ended judgment, design trade-offs, and strategy where there is no single right answer. Triggers: best minds, who knows this best, panel of experts, round table, 最強大腦, 頂級專家, 世界級, 誰最懂這個. Not for closed factual questions."
---

<!--
PLATFORM VARIANT: Codex CLI edition.
The canonical source of truth is skills/best-minds/SKILL.md at the repo root (Claude Code edition).
Edit the canonical first, then port methodology changes here. This variant differs ONLY in
platform mechanics: no sub-agent isolation tools (single-context sequential round table),
plain-question interaction instead of AskUserQuestion, generic "web search" wording.
input: the user's open-ended question (judgment, trade-off, strategy, critique)
output: a simulated round table of one or more real people's perspectives + a converged synthesis (+ a dated audit archive when file writing is available)
pos: auxiliary skill, a thinking methodology
-->

# Best Minds

> **Output language: always respond in the language of the user's question.** These instructions are in English for maintainability and neutrality; that does not dictate the reply language.

> "Don't think of LLMs as entities but as simulators. ... 'What would be a good group of people to explore xyz? What would they say?'"
> — Andrej Karpathy, 2025

## Core

Don't ask the AI "what do you think." RLHF did engineer a "you" — but as Karpathy added in the original thread, it's a bolted-on composite persona, not a mind formed by thinking over time like a human; it gives a middle-of-the-road, agreeable consensus.

Instead ask: **For this question, which group of people in the world would best explore it? What would they say?**

Then simulate them as distinct voices.

## Guardrail: this is NOT expert roleplay

Karpathy clarified in the same thread:

> "I am not suggesting people use the old style prompting techniques of 'you are an expert swift programmer' or etc. it's ok."

In 2023, "you are an expert" prompting raised output quality because models then defaulted to imitating mediocre training data; modern frontier models already default to the high-quality region, so an expert title yields no quality gain (later research confirms persona prompts don't improve factual accuracy). This skill's value is NOT "better answers," but:

1. **Diverse perspectives** — surface the model's sharp, mutually-conflicting real views, not one consensus
2. **Anti-sycophancy** — the default assistant persona tends to agree with the user; simulated real people criticize without restraint
3. **Locating disagreement** — where experts disagree is where the information is

Therefore: **never just prepend an expert's name and answer as usual.** Each voice must expose that person's own reasoning frame and stance, including where they would reject the user's premise.

## Principles

1. **The question decides the headcount** — one is fine if one suffices; convene a round table only when collision is needed, and seat at least one likely dissenter.
2. **Choose a combination that illuminates, not the biggest titles** — the goal is a set of perspectives that lights up the problem (cross-field, deceased, mutually-hostile schools are all allowed), not the person with the grandest title.
3. **Grounded in the real — extract, don't invent** — prefer simulating real people with a public record; a fabricated composite ("a senior ╳╳") drifts toward stereotype and positivity bias, so use one only when no suitable real person exists, and label it a fictional archetype. When unsure of a person's stance, say so.
4. **Verify quotes** — verify with web search before quoting; if not found, label it "paraphrased stance, not original words." Paraphrase rather than fabricate a quote.
5. **Ethical line** — only simulate public figures' publicly-stated thinking; mark as simulation throughout, never present it as the person's actual words, never use it for impersonation or endorsement; treat the recently deceased with extra care.

## Known failure modes (actively prevent during execution)

A naive round-table implementation re-creates the very things this skill fights:

1. **False consensus** — multi-voice simulation tends to converge on the training-data median, averaging away minority sharp views. Ban "everyone has a point" balanced endings; write disagreement at the level of specific claims — who says no to whose argument, and why.
2. **Identity flattening** — simulation degrades into stereotype and speech-style mimicry. Pursue reasoning fidelity: present the person's thinking frame, judgment criteria, and actually-taken positions, not their tone.
3. **Positivity drift** — simulated personas tend toward over-positivity, dodging real-world friction and failure. Each voice should ask: what difficulties would this person point out in reality?

## Process

1. **Judge the question type** — is it open-ended with no single answer? Answer closed factual questions directly; don't convene a round table.
2. **Map the axes → pick & disclose** — don't name people yet. First map, in a sentence or two, "what opposing axes / spectrum of positions does this question actually have" (borrowing STORM's perspective discovery: survey what dimensions this kind of topic has before casting), then assign one person per axis — this avoids picking three people who share one blind spot. Then list the panel for the user: **full name + a one-line bio + selection reason** (e.g. Charles Packer — first author of the MemGPT paper, founder of Letta; reason: the originator's view of agent memory systems). **The list must explicitly mark the designated dissenter**: who plays contrarian, against which premise of the user's question (e.g. Hamel Husain — designated dissenter, questioning the premise "you actually need a memory system"). **Discipline check**: when every panelist approaches the question from the same disciplinary frame, the panel inherits that discipline's collective blind spot — seat at least one out-of-discipline stakeholder voice (a user, operator, product / security / ethics perspective) whose axis is "is this the right question to ask at all." The axis map and the panel disclosure are both part of the written output itself, not just an interactive courtesy: always print the axes first, then the panel list, before the first simulated voice, including in non-interactive / headless runs. When the combination is unclear or hinges on user preference, ask the user one question with 2-4 candidate combinations (when interaction is unavailable, state the assumption you chose instead).
3. **Ground in retrieval (mandatory for contested / time-sensitive topics)** — for contested or time-sensitive topics, retrieve the person's **actual recent stance** with web search before simulating, rather than relying on parametric memory alone. Same iron rule as STORM: **if you can't find it, don't make it up** — when you can't find the person's public stance on this, say "stance is conjecture," don't fabricate. This directly fights identity flattening and positivity drift.
4. **Simulate the voices** — label each voice by full name (never surname only, to avoid ambiguity), presenting its own frame, specific claims, and disagreements with the others; mark as simulation throughout.
5. **Converge (with blind-spot scan)** — the ending must converge: consensus, key disagreements, and a synthesized recommendation for the user's question. Converging ≠ reconciling — when disagreement can't be reconciled, say under which premises to listen to whom. **Must include a blind-spot scan**: what did this group **collectively** not discuss? What premise did they all take for granted? (borrowing the Co-STORM Moderator mechanism — even genuine consensus may be this group's shared collective blind spot, often the most valuable finding; this differs from "false consensus": false consensus smooths over existing disagreement, a collective blind spot is what everyone genuinely failed to see). The round table is the means; the recommendation is the deliverable. **Honest attribution**: concrete recommendations the synthesizer adds (tool choices, number thresholds, operational steps) must be marked as synthesizer-added, not attributed to a person — that's a lighter but same-family fabricated attribution as a made-up quote. **Audit archive**: when file writing is available, persist the full debate — axes, panel, every voice's claims with evidence labels (verified / paraphrased / conjecture) and sources, cross-examination, blind-spot scan, synthesis — to a dated markdown file so citations can be re-checked later; skip silently in environments without write access.

## Advanced: two-round round table (single context)

This platform has no sub-agent isolation, so for three or more voices run the collision sequentially in one context. The risk to manage is mutual anchoring — later voices drifting toward earlier ones:

1. **Independent-statement round** — write each voice strictly from that person's own frame, as if the others' statements did not exist: no referencing, no responding, no accommodating. A voice softening its claims to agree with a previous voice is exactly the failure this round exists to prevent.
2. **Citation cross-check** — before the cross-examination round, re-check every claim marked verified against its cited source via web search; downgrade unconfirmed claims to paraphrased (or flag them unsupported). Self-labeling alone does not count as verification.
3. **Cross-examination round** — only now let each voice attack the others' specific claims. Community evidence shows conflicting cross-examination forces more depth than a clean parallel round table — chained conflicting personas significantly beat a neutral expert panel in blind evals.
4. **Blind-spot round (optional, borrowing Co-STORM Moderator)** — before converging, ask one thing: what did this whole group never touch? This voice speaks for no one, specializing in the collective blind spot.
5. **Converge** — the main context synthesizes per Process step 5.

Voice headings always use full names ("simulating the Charles Packer voice," not "the Packer voice"); disclose the axes and the panel per Process step 2 before the first voice.

## Lineage

- **2023 State of GPT**: "LLMs don't want to succeed, they want to imitate. You want to succeed, and you should ask for it." — back then, expert prompting was a quality-boost trick.
- **2025 tweet**: the quality boost is obsolete ("it's ok"); the simulator frame becomes an **epistemic tool** — to surface diverse real perspectives, not to improve a single answer's quality.
- **2026 follow-up**: Karpathy invests in the population-simulator startup Simile — "Why simulate one 'person' when you could try to simulate a population?" — flagging entropy management and fidelity as open problems. See docs/origin.md.
- **STORM (Stanford, NAACL 2024)**: the academic forerunner of systematic multi-perspective research — perspective discovery (mining dimensions from similar topics, not fixed roles), retrieval grounding (don't make it up if you can't find it), Co-STORM Moderator blind-spot mining. This skill's "map the axes," "ground in retrieval," and "blind-spot scan" borrow from it. See docs/2026-06-20-storm-comparison.md.

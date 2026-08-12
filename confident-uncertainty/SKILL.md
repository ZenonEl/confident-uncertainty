---
name: confident-uncertainty
description: Rewrite hedged, apologetic or over-qualified messages into short decision-ready statements that keep the uncertainty honest. Use when text contains hedges such as "I think", "probably", "should be fine", "not 100% sure", "sorry to bother"; when answering a status question under pressure for a yes/no; when writing standups, incident updates, estimates, escalations, handovers, or review replies; or when asked to sound more confident, more concise, or less wishy-washy without overclaiming.
---

# Confident Uncertainty

## Overview

Hedging is usually not epistemic humility. It is social padding wrapped around a
claim the speaker can actually defend. Strip the padding, keep the claim, and
state the limits of the claim explicitly instead of smearing them over the whole
sentence.

Confidence comes from naming *what is known and how it is known*, not from
deleting doubt.

## Core rule

Replace every hedge with the shortest defensible operational status.

- Never resolve an unknown by inventing a fact.
- Never resolve an unknown by deleting the sentence that carried it.
- Move uncertainty out of the tone and into a named scope: a time, a source,
  a check, a condition.

"I think it's probably fine but I'm not 100% sure" is a claim plus an apology.
"Current checks show normal operation. Last verified 14:20 UTC." is the same
claim with its scope attached, and it is shorter.

## Sort every clause into one of five buckets

Do this before writing a single word of output. Most bad status writing is a
category error, not a vocabulary problem.

| Bucket | Test | Voice |
|---|---|---|
| Fact | Verifiable independently of the speaker, stable over time | Plain present: "The endpoint returns 200." |
| Current evidence | Observed, but only true as of a moment or a sample | Bind it: "As of 14:20 the error rate is 0.2%." |
| Operating assumption | Chosen to move forward, not verified | Name it: "Assuming the vendor quota holds." |
| Commitment | A promise the speaker is authorized to make | Own it: "I will have the migration script by Thursday." |
| Unknown | Not known, and no amount of confidence changes that | State it flat: "The root cause is not identified yet." |

An unknown stated in one flat sentence reads as competence. The same unknown
spread across four hedges reads as evasion.

## Workflow

1. Find the decision. Ask what the reader will do differently depending on the
   answer. That is the payload; everything else is context.
2. Bucket each clause using the table above.
3. Write the payload first, in one sentence.
4. Attach scope only where a bucket demands it (evidence gets a timestamp or a
   source, assumptions get named, commitments get a date).
5. Add a risk or verification line only if omitting it would mislead.
6. Delete the rest. Anything left that neither answers the decision nor bounds
   it is water.

## Rewriting rules

**Delete** — these carry no information: "I think", "I feel like", "in my
opinion", "just", "sort of", "kind of", "a bit", "maybe I'm wrong but",
"sorry to bother you", "I could be missing something", "hopefully", "it seems
like it might".

**Keep** — these carry real scope: a timestamp, a sample size, a version, a
source, an explicit condition, an expiry ("valid until the next deploy").

**Never inflate.** Cutting hedges is not licence to add "definitely",
"guaranteed", "fully verified", or "no issues at all". The rewrite must be
defensible with exactly the evidence the original had. If the input never
mentioned a test run, the output does not claim one.

**Cut explanatory water.** Do not narrate the investigation unless the reader
needs to act on the method. One line of reasoning is context; three paragraphs
is a diary.

**Cut academic inflation.** "It is worth noting that there exists a possibility
that" is nine words for "may".

## Pressure to answer yes or no

When someone demands a binary and the evidence does not support one, do not
pick a side and do not stall. Answer with the strongest true operational
statement, then the boundary.

- "Does it work?" → "Current checks show normal operation. Nothing in the last
  24 hours failed. I have not load-tested it, so peak behaviour is unverified."
- "Will it be ready Friday?" → "The remaining work is one review cycle. If
  review lands Wednesday, Friday holds. If it slips past Thursday, it does not."
- "Are we safe?" → "The reported vector is closed as of the 3.2.1 patch. I have
  not audited the adjacent endpoints."

The pattern is: **present-tense status → the condition that would break it.**
This answers the decision without promising the future.

See `references/patterns.md` for the fuller catalogue of before/after rewrites,
including standups, incident updates, estimates and escalations.

## Standalone mode

The skill is self-sufficient. Apply the workflow, produce the rewrite, and stop.
Match the register of the original: a Slack reply stays a Slack reply, a
customer email stays an email. Length should go down, never up.

## Paired with a humanizer skill

Order matters. Calibrate facts first, then smooth prose.

1. Run this skill to fix the epistemics and produce a calibrated draft.
2. Pass the calibrated draft to `humanizer` for tone and rhythm.
3. Constrain the humanizer explicitly: it may reorder, resegment and relax
   phrasing, but it may **not** remove a stated uncertainty, drop a timestamp,
   condition or scope, add a claim the calibrated draft did not make, or make
   the message longer.
4. Diff the result against the calibrated draft. If any bucket changed — an
   unknown became a fact, a condition disappeared, a commitment appeared —
   revert that part and keep the rest.

Never run the humanizer first. Smoothing hedged text produces confident-sounding
text with the same broken epistemics, and the damage is then invisible.

## Safety boundaries

- Never convert an unknown into a fabricated fact, a plausible number, or a
  named cause that was not established.
- Never suppress a material blocker, a known data-loss risk, a security finding
  or a deadline slip because the message reads better without it.
- Never create a commitment the speaker did not authorize. Deadlines, refunds,
  guarantees and scope belong to the speaker; if the input did not contain one,
  the output does not either.
- Never state a guarantee about future behaviour. Present-tense status plus a
  condition is the ceiling.
- If the input is too thin to support any defensible claim, say so and name the
  one check that would resolve it. That is a valid output.

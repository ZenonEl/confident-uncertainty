# confident-uncertainty

A writing skill for Claude Code and Codex that turns hedged, apologetic,
over-qualified messages into short decision-ready statements — without
manufacturing certainty that the evidence does not support.

Hedging is rarely epistemic humility. It is social padding wrapped around a
claim the speaker can actually defend. This skill strips the padding, keeps the
claim, and states the limits of the claim explicitly instead of smearing doubt
across the whole message.

```text
what you have   ran it twice this morning, both clean, nothing in the error
                log; never reproduced the original trigger; the nightly
                covers that path tonight

before          "I think it should be working now? I tested it a couple of
                 times and didn't see any errors, but I'm not 100% sure it's
                 fully fixed."

after           "Two clean runs this morning, no errors. I have not reproduced
                 the original trigger — tonight's nightly covers that path."
```

Nothing invented, nothing hidden, half the words. The specifics in the rewrite
are recovered from what you already knew — a hedged draft usually throws those
away, and "a couple of times" is two runs whose result you remember perfectly
well.

If you *don't* have them, the output doesn't get them either:

```text
after (thin)    "It ran clean the last few times I tried it. I have not
                 confirmed the original problem is gone."
```

Vaguer, still shorter than the hedge, and still honest. Naming a gap is a valid
answer; inventing a number to fill it is the failure this skill exists to
prevent.

## What it does

Every clause gets sorted into one of five buckets before anything is rewritten:

| Bucket | Voice in the output |
|---|---|
| Fact | Plain present tense |
| Current evidence | Bound to a time, sample or source |
| Operating assumption | Named as an assumption |
| Commitment | Owned, with a date |
| Unknown | Stated flat, in one sentence |

Most bad status writing is a category error rather than a vocabulary problem —
an unknown dressed as a fact, or a solid observation buried under apology. The
bucketing pass fixes the category, and the rewrite follows from it.

The skill then answers the actual decision the reader faces, drops explanatory
water and academic inflation, and surfaces material risk when leaving it out
would mislead.

One rule holds the rest together: **every specific in the output must trace to
the input or to notes you actually have.** Before a rewrite ships, each number,
time, version, cause and date gets pointed at and sourced; anything unsourced
comes out. The constraint is not "say less" — it is "say only what you can
source", and where the evidence is rich the output is correspondingly concrete.

## Install

The installable skill lives in the [`confident-uncertainty/`](confident-uncertainty)
subdirectory of this repository. Clone the repo, then copy or symlink that one
directory into your agent's skills folder.

### Claude Code

```bash
git clone https://github.com/<owner>/confident-uncertainty.git
mkdir -p ~/.claude/skills
cp -r confident-uncertainty/confident-uncertainty ~/.claude/skills/
```

Use it by name in a session:

```text
Use the confident-uncertainty skill to rewrite this status update.
```

Claude Code also picks the skill up on its own when a message is full of hedges
and you ask for something tighter.

### Codex

```bash
git clone https://github.com/<owner>/confident-uncertainty.git
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -r confident-uncertainty/confident-uncertainty "${CODEX_HOME:-$HOME/.codex}/skills/"
```

Invoke it explicitly:

```text
Use $confident-uncertainty to rewrite this hedged status update into a concise,
decision-ready answer.
```

For a project-local install instead of a user-wide one, drop the directory into
the project's `.claude/skills/` or `.codex/skills/` folder.

## Standalone use

No other skill is required. Paste the hedged text, ask for a rewrite, and the
skill produces a calibrated version in the same register — a Slack reply stays a
Slack reply, an email stays an email. Length goes down, never up.

It covers standups, incident updates, estimates, escalations, review comments
and handover notes. The full catalogue of worked rewrites lives in
[`confident-uncertainty/references/patterns.md`](confident-uncertainty/references/patterns.md),
which the agent loads only when it needs it.

## Pairing with a `humanizer` skill

If you also run a `humanizer`-style skill that smooths AI-flavoured prose, the
order is not optional:

1. **Calibrate first.** Run `confident-uncertainty` to fix the epistemics.
2. **Smooth second.** Pass the calibrated draft to `humanizer`.
3. **Constrain it.** The humanizer may reorder, resegment and relax phrasing. It
   may not remove a stated uncertainty, drop a timestamp or condition, add a
   claim the calibrated draft did not make, or make the message longer.
4. **Diff.** If an unknown turned into a fact, a condition vanished, or a
   commitment appeared, revert that part.

Running the humanizer first produces confident-sounding text with the same
broken epistemics — and the damage becomes invisible, because the result reads
well.

## Examples

Every rewrite below is valid *given the stated evidence*. Strip the evidence and
the honest answer shrinks with it — that is the point, not a limitation.

**Pressure for a yes/no answer**

```text
what you have   the dashboard is green; you have not run a load test

Q  "Does it work?"
A  "Current checks show normal operation. I have not load-tested it, so peak
    behaviour is unverified."
```

The pattern is *present-tense status → the condition that would break it*. It
answers the question without promising the future.

Haven't looked at the dashboard? Then the answer is `"I have not checked today —
give me ten minutes and I will tell you."` Still decision-ready: it gives the
asker a time instead of a guess dressed as a status.

**An estimate**

```text
what you have   gut feel of two weeks; the staging migration took three days
                and behaved; production data volume is unmeasured; measuring
                it would take about a day

before  "It's hard to say exactly, maybe two weeks? Could be longer if we hit
         problems with the migration, hard to know at this point."

after   "Two weeks if production migrates like staging did. The open variable is
         production data volume, which nobody has measured — I can measure it in
         about a day and firm the estimate up after."
```

The conditional does the work the hedging was attempting, and it hands the
reader a lever: they can buy certainty by funding the one-day check.

**Escalation**

```text
what you have   the last three export runs were checked; the most recent is
                missing 412 rows; the first bad run was Tuesday; no cause
                found; invoicing goes out Friday

before  "Sorry to bother you — I might be wrong but it looks like there could
         maybe be a problem with the billing export?"

after   "The billing export has been dropping rows since Tuesday — 412 missing
         in the most recent run. I have not found the cause. Invoicing goes out
         Friday, so we need a decision before then on whether to pause it."
```

The Friday deadline is a fact from your notes, not urgency added for effect.
Inventing a deadline to force attention is its own kind of fabrication.

## What it will not do

- Turn an unknown into a fabricated fact, number, date or root cause.
- Conceal a material blocker, security finding or deadline slip to make a
  message read better.
- Create a commitment the speaker did not authorize.
- Guarantee future behaviour. Present-tense status plus a condition is the
  ceiling.

If the input is too thin to support any defensible claim, the skill says so and
names the single check that would resolve it.

## Repository layout

```text
README.md
LICENSE
confident-uncertainty/
  SKILL.md
  agents/openai.yaml
  references/patterns.md
```

## License

MIT. See [LICENSE](LICENSE).

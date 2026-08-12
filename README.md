# confident-uncertainty

A writing skill for Claude Code and Codex that turns hedged, apologetic,
over-qualified messages into short decision-ready statements — without
manufacturing certainty that the evidence does not support.

Hedging is rarely epistemic humility. It is social padding wrapped around a
claim the speaker can actually defend. This skill strips the padding, keeps the
claim, and states the limits of the claim explicitly instead of smearing doubt
across the whole message.

```text
before  "I think it should be working now? I tested it a couple of times and
         didn't see any errors, but I'm not 100% sure it's fully fixed."

after   "Fixed in the last three runs, no errors. I have not reproduced the
         original trigger, so I would not call it confirmed until the next
         nightly."
```

Same evidence. Nothing invented, nothing hidden, half the words.

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

**Pressure for a yes/no answer**

```text
Q  "Does it work?"
A  "Current checks show normal operation. Nothing in the last 24 hours failed.
    I have not load-tested it, so peak behaviour is unverified."
```

The pattern is *present-tense status → the condition that would break it*. It
answers the question without promising the future.

**An estimate**

```text
before  "It's hard to say exactly, maybe two weeks? Could be longer if we hit
         problems with the migration, hard to know at this point."

after   "Two weeks if the migration behaves like the staging run. The unknown is
         production data volume — I can size that in a day and firm up the
         estimate after."
```

The conditional does the work the hedging was attempting, and it hands the
reader a lever: they can buy certainty by funding the one-day check.

**Escalation**

```text
before  "Sorry to bother you — I might be wrong but it looks like there could
         maybe be a problem with the billing export?"

after   "The billing export has been dropping rows since Tuesday — 412 missing
         in the last run. I have not found the cause. This needs a decision
         today on whether to pause invoicing."
```

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

# Rewrite patterns

Worked before/after examples. Read this when the input does not obviously fit
the workflow in SKILL.md, or when the register is unfamiliar.

## How to read these examples

Each example states **what the author has** before it shows the rewrite. That
block is the author's own knowledge — what they saw, ran, measured or planned.
Every specific in the "after" traces back to it.

This matters more than it looks. A hedged draft has usually discarded the
specifics that would make it strong: "a couple of times" is two runs whose
result the author remembers perfectly well. The rewrite recovers those details
from the author. It never manufactures them.

**If the author does not have the notes, the output does not get the specifics.**
Several examples below show the thin-evidence variant next to the rich one.
Naming a gap is a valid, decision-ready answer; inventing a number to fill it is
the failure this skill exists to prevent.

## Contents

- [Status question under pressure](#status-question-under-pressure)
- [The calibrated-status pattern](#the-calibrated-status-pattern)
- [Standup and progress updates](#standup-and-progress-updates)
- [Incident and outage updates](#incident-and-outage-updates)
- [Estimates and deadlines](#estimates-and-deadlines)
- [Escalation and bad news](#escalation-and-bad-news)
- [Review and disagreement](#review-and-disagreement)
- [Handover notes](#handover-notes)
- [When the notes are thin](#when-the-notes-are-thin)
- [Auditing a rewrite](#auditing-a-rewrite)
- [Failure modes to check before shipping](#failure-modes-to-check-before-shipping)

## Status question under pressure

> **What the author has:** ran the fixed build twice this morning, both runs
> clean, nothing in the error log. Never managed to reproduce the original
> trigger. The nightly suite exercises that path and runs tonight.

> **Before:** "I think it should be working now? I tested it a couple of times
> and didn't see any errors, but I'm not 100% sure it's fully fixed."

> **After:** "Two clean runs this morning, no errors. I have not reproduced the
> original trigger — tonight's nightly covers that path."

Sourcing each specific: *two runs* and *this morning* and *no errors* come from
the author's own runs, which the draft had compressed into "a couple of times";
*not reproduced* comes from the draft's own doubt, now stated flatly; *tonight's
nightly* comes from the notes.

The rewrite did not add information — it recovered information the hedging had
thrown away. That is only true because the notes were there. If the author is
not sure whether "a couple" was exactly two, the output says "a few clean runs
this morning" or asks. Upgrading a vague quantity into a precise one is
fabrication even when the precise number happens to be right.

**Thin-evidence variant.** Same draft, but the author has nothing beyond it — no
memory of how many runs, no knowledge of a nightly:

> **After:** "It ran clean the last few times I tried it. I have not confirmed
> the original problem is gone."

Note what dropped out along with the notes: not just the count and the nightly,
but the *reason* for the doubt. The rich version can say the trigger was never
reproduced because the author knows that. The thin version can only report the
doubt itself, which the draft did supply as "not 100% sure it's fully fixed".

Shorter, vaguer, and still a better answer than the original, because the doubt
is now one flat clause instead of a mood spread over two sentences.

## The calibrated-status pattern

> **Before:** "Probably works."

> **After:** "Current checks show normal operation."

This rewrite is valid **only when checks actually exist and the author has seen
them** — a green dashboard, a passing health endpoint, a monitor that would have
paged. Given that, the output is strong: it reports what is observed and
promises nothing about the future, which is exactly what "probably works" was
gesturing at without committing to.

Without any check behind it, the same sentence is worse than the hedge, because
it sounds like evidence and is not. Then the honest forms are:

- "It worked when I last used it. Nothing is monitoring it, so I cannot say what
  it is doing now."
- "Not verified. I can check now."

The second is usually the best answer to a status question you cannot answer: it
converts an unknown into a next step. Offer a duration only if you actually know
how long the check takes — an invented interval is a commitment the speaker
never made.

## Standup and progress updates

> **What the author has:** spent yesterday on the cache. Reproduced two key
> collisions locally. Suspects key generation. Plan for tomorrow: run the full
> key-space comparison, which will show whether it explains the whole failure
> rate.

> **Before:** "So I've been kind of digging into the caching thing, it's a bit
> more complicated than we thought, I think there might be an issue with how
> keys are generated but I need to look into it more before I can say anything
> for sure."

> **After:** "Cache work in progress. Key generation is the leading hypothesis —
> I reproduced two collisions locally. The full key-space comparison runs
> tomorrow and will tell us whether it explains the whole failure rate."

Three hedges become one hypothesis with its evidence attached. "More complicated
than we thought" is deleted: it tells the reader nothing they can act on.

The date is not a reward for confidence — it is the author's own plan, lifted
from the notes. A rewrite must never manufacture a date to make an update feel
firmer. If the notes contain no plan, the last sentence becomes "No date on the
full comparison yet" and the update is still fine.

## Incident and outage updates

> **What the author has:** alerts firing on checkout errors since 09:40. A
> deploy went out at 09:35 and is the only change in that window. Has not
> measured how many users are affected. Rollback not started — that call belongs
> to the on-call lead. Intends to post again in 20 minutes.

> **Before:** "We're seeing some issues that might be affecting some users, we
> think it's probably related to the deploy but we're still investigating and
> hopefully it should resolve soon."

> **After:** "Checkout has been erroring since 09:40. Impact not yet quantified.
> The 09:35 deploy is the only change in that window and is the first thing I am
> checking. Rollback is not started — that call sits with the on-call lead. Next
> update in 20 minutes."

Incident writing is where hedging does the most damage, because readers make
routing decisions from it. Give scope, the current action, who owns the pending
decision, and when the next update lands.

Note what the output does **not** say. The author has no impact measurement, so
the output says `impact not yet quantified` rather than a percentage. It does
not claim a rollback is running, because none is. It does not call the deploy
the cause — only the sole change in the window, which is what the author
actually knows. An unquantified impact is a fact about the investigation; a
fabricated percentage is a lie with a decimal point, and under incident pressure
it is the single most tempting thing to write.

"Hopefully it should resolve soon" is replaced by a commitment the author can
actually keep and already intended: the next update.

## Estimates and deadlines

> **What the author has:** gut estimate of two weeks. The staging migration took
> three days and behaved as expected. Production data volume is unmeasured —
> nobody has looked. Measuring it would take about a day.

> **Before:** "It's hard to say exactly, maybe two weeks? Could be longer if we
> hit problems with the migration, hard to know at this point."

> **After:** "Two weeks if production migrates like staging did. The open
> variable is production data volume, which nobody has measured — I can measure
> it in about a day and firm the estimate up after."

"Two weeks" comes from the draft; the staging comparison, the unmeasured volume
and the one-day sizing come from the notes.

An estimate is an operating assumption, so name the assumption. The conditional
does the work the hedging was attempting, and it hands the reader a lever: they
can buy certainty by funding the one-day check.

## Escalation and bad news

> **What the author has:** the billing export is missing rows. Checked the last
> three runs; the most recent is missing 412. The first bad run was Tuesday. No
> cause found. Invoicing goes out Friday.

> **Before:** "Sorry to bother you with this, I know you're busy — I might be
> wrong but it looks like there could maybe be a problem with the billing
> export, though it's possible I'm misreading it."

> **After:** "The billing export has been dropping rows since Tuesday — 412
> missing in the most recent run. I have not found the cause. Invoicing goes out
> Friday, so we need a decision before then on whether to pause it."

Apology framing buries the severity, which is exactly backwards for escalation.
Lead with impact, state the unknown flatly, name the decision required and its
deadline. The Friday deadline is a fact from the notes, not urgency added for
effect — inventing a deadline to force attention is its own kind of fabrication.

Never soften the blocker to make the message pleasant. Concealing a material
problem is the one failure this skill exists to prevent.

## Review and disagreement

> **What the author has:** read the handler; it opens and parses the config file
> on every request. The service does roughly 4k requests a second at peak
> according to the dashboard. Does not know whether dynamic reload is a
> requirement.

> **Before:** "This might just be me but I feel like this approach could
> potentially cause issues down the line, though you probably know better."

> **After:** "This re-reads and parses the config on every request — about 4k
> extra file reads a second at peak. Caching it at startup avoids that. Is there
> a reason it needs to be dynamic?"

Deference gets replaced by a mechanism, a number from the dashboard, and a
question. The question is not politeness: it carries the author's one genuine
unknown, whether dynamic reload is required. The observation itself is not
uncertain and is not hedged.

## Handover notes

> **What the author has:** ran the suite ten times this week. `test_sync_retry`
> failed twice; everything else passed every time. Cause unknown. Re-running has
> always cleared it, and it has never failed twice consecutively.

> **Before:** "Everything should be fine, I think the tests are all passing,
> there's maybe one flaky thing but it's probably nothing."

> **After:** "All suites pass except `test_sync_retry`, which failed 2 of my 10
> runs this week. Cause unknown. Re-running is the workaround — it has not
> failed twice in a row so far."

"Probably nothing" is the phrase most likely to become someone else's incident.
State the observed rate, the unknown, and the workaround.

"2 of my 10 runs" rather than "1 in 5" or "20% flaky": the author ran it ten
times, and a rate derived from ten samples should be reported as the samples.
Converting a small count into a percentage manufactures confidence the sample
size does not support.

## When the notes are thin

Sometimes there is nothing to recover — no runs, no dashboard, no measurement.
The rewrite still works; it just states less.

| Situation | Wrong (invented) | Right |
|---|---|---|
| Asked for status, has not looked | "Everything looks healthy." | "I have not checked today. I can look now and tell you." |
| Asked for a cause, has a hunch only | "It's the connection pool." | "Untriaged. The connection pool is where I would look first." |
| Asked for impact, has not measured | "Only a few users." | "Impact not yet quantified." |
| Asked for a date, has no plan | "End of the week." | "No date yet — I have not scoped it. I will have one once I do." |
| Asked whether a fix worked, ran nothing | "Should be fine now." | "Not verified — I have not run it since the change." |

Every "right" entry is defensible with nothing but what the speaker actually
has, and none of them claims knowledge the situation column rules out. Where
honesty costs a few extra words over the invented answer, those are the words
that keep it true.

## Auditing a rewrite

Before sending, run the traceability check. Point at each specific and name its
source:

> "Two clean runs" ← author ran it twice
> "this morning" ← author's own timing
> "no errors" ← author read the log
> "tonight's nightly" ← author's notes on the schedule

Anything that cannot be pointed at comes out. If removing it leaves a hole the
reader needs filled, ask the author for the detail rather than supplying it.

## Failure modes to check before shipping

Run this list against the rewrite:

- **Unsourced specific** — any number, date, time, version, cause, test name or
  action that traces to neither the input nor the author's notes. Remove it or
  ask for its source.
- **Upgraded vagueness** — "a couple" became "three", "recently" became a
  timestamp, "some users" became a percentage. The precision is invented even
  when the direction is right.
- **Vanished unknown** — something the input flagged as uncertain now reads as
  settled. Restore it as one flat sentence.
- **Smuggled commitment** — the output promises a date, a fix or a guarantee the
  speaker never made. A date is only legitimate when it is the speaker's own
  plan.
- **Manufactured urgency** — a deadline or consequence added to make the message
  land harder.
- **Buried blocker** — the material problem appears after two sentences of
  context. Move it first.
- **Longer than the original** — the rewrite failed. Calibration removes words;
  it does not add ceremony.
- **False binary** — the output answered yes or no to a question the evidence
  cannot settle. Use present-tense status plus the breaking condition instead.

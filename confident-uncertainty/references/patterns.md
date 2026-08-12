# Rewrite patterns

Worked before/after examples. Read this when the input does not obviously fit
the workflow in SKILL.md, or when the register is unfamiliar.

## Contents

- [Status question under pressure](#status-question-under-pressure)
- [Standup and progress updates](#standup-and-progress-updates)
- [Incident and outage updates](#incident-and-outage-updates)
- [Estimates and deadlines](#estimates-and-deadlines)
- [Escalation and bad news](#escalation-and-bad-news)
- [Review and disagreement](#review-and-disagreement)
- [Handover notes](#handover-notes)
- [Failure modes to check before shipping](#failure-modes-to-check-before-shipping)

## Status question under pressure

> **Before:** "I think it should be working now? I tested it a couple of times
> and didn't see any errors, but I'm not 100% sure it's fully fixed."

> **After:** "Fixed in the last three runs, no errors. I have not reproduced the
> original trigger, so I would not call it confirmed until the next nightly."

What changed: the two hedged sentences collapse into one evidence statement plus
one named gap. Nothing was added — the "three runs" was already in the original
as "a couple of times", and the unverified part is now a specific check with a
date rather than a feeling.

> **Before:** "Probably works."

> **After:** "Current checks show normal operation."

The calibrated form claims exactly what was observed and promises nothing about
the future. It is not a weaker statement than "probably works"; it is the same
statement with its scope made visible.

## Standup and progress updates

> **Before:** "So I've been kind of digging into the caching thing, it's a bit
> more complicated than we thought, I think there might be an issue with how
> keys are generated but I need to look into it more before I can say anything
> for sure."

> **After:** "Cache work is in progress. Key generation looks like the likely
> cause — two collisions reproduced locally. I will know by tomorrow whether it
> explains the whole failure rate."

Three hedges become one hypothesis with evidence attached and one commitment
with a date. "More complicated than we thought" is deleted: it tells the reader
nothing they can act on.

## Incident and outage updates

> **Before:** "We're seeing some issues that might be affecting some users, we
> think it's probably related to the deploy but we're still investigating and
> hopefully it should resolve soon."

> **After:** "Checkout is failing for roughly 8% of requests since 09:40. The
> 09:35 deploy is the leading suspect; rollback is running now. Next update in
> 20 minutes."

Incident writing is where hedging does the most damage — readers make routing
decisions from it. Give scope (what, who, since when), the current action, and
the time of the next update. "Hopefully it should resolve soon" is replaced by a
commitment the author can actually keep: the next update.

If the impact number is genuinely unknown, say `impact not yet quantified`
rather than guessing a percentage. An unquantified impact is a fact about the
investigation; a fabricated percentage is a lie with a decimal point.

## Estimates and deadlines

> **Before:** "It's hard to say exactly, maybe two weeks? Could be longer if we
> hit problems with the migration, hard to know at this point."

> **After:** "Two weeks if the migration behaves like the staging run. The
> unknown is production data volume — I can size that in a day and firm up the
> estimate after."

An estimate is an operating assumption, so name the assumption. The conditional
does the work the hedging was trying to do, and it gives the reader a lever:
they can buy certainty by funding the one-day check.

## Escalation and bad news

> **Before:** "Sorry to bother you with this, I know you're busy — I might be
> wrong but it looks like there could maybe be a problem with the billing
> export, though it's possible I'm misreading it."

> **After:** "The billing export has been dropping rows since Tuesday — 412
> missing in the last run. I have not found the cause. This needs a decision
> today on whether to pause invoicing."

Apology framing buries the severity, which is exactly backwards for escalation.
Lead with impact, state the unknown flatly, name the decision required.

Never soften the blocker to make the message pleasant. Concealing a material
problem is the one failure this skill exists to prevent.

## Review and disagreement

> **Before:** "This might just be me but I feel like this approach could
> potentially cause issues down the line, though you probably know better."

> **After:** "This approach re-reads the config on every request. At current
> traffic that is ~4k extra file reads a second. Caching it at startup avoids
> that — is there a reason it needs to be dynamic?"

Deference gets replaced by a mechanism, a number and a question. The question
preserves the genuine uncertainty (the author may have a reason) without
pretending the observation is uncertain — it is not.

## Handover notes

> **Before:** "Everything should be fine, I think the tests are all passing,
> there's maybe one flaky thing but it's probably nothing."

> **After:** "All suites pass except `test_sync_retry`, which fails about 1 in 5
> runs. Cause unknown. Rerunning is the current workaround; it has never failed
> twice in a row."

"Probably nothing" is the phrase most likely to become someone else's incident.
State the flake rate, the unknown, and the workaround.

## Failure modes to check before shipping

Run this list against the rewrite:

- **Invented precision** — a number, date, cause or percentage that was not in
  the input. Remove it.
- **Vanished unknown** — something the input flagged as uncertain now reads as
  settled. Restore it as one flat sentence.
- **Smuggled commitment** — the output promises a date, a fix or a guarantee the
  speaker never authorized. Remove it.
- **Buried blocker** — the material problem appears after two sentences of
  context. Move it first.
- **Longer than the original** — the rewrite failed. Calibration removes words;
  it does not add ceremony.
- **False binary** — the output answered yes or no to a question the evidence
  cannot settle. Use present-tense status plus the breaking condition instead.

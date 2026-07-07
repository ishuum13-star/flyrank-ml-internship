# Research Question & Provisional Lane

## Provisional Lane
Content Refresh Prioritization (one of the four core lanes — using declining-page 
signals to guide which pages get reviewed first).

## Research Question
Among pages that are losing search visibility, which observable signals 
(impressions, average position, CTR, content age, days since last update) 
best separate pages that are actually declining from pages that are stable 
or growing — and can this be used to rank pages for manual review?

## Unit of Analysis
A single page (identified by an anonymized page ID), measured over a 90-day window.

## Output
A ranked "refresh review queue" — a list of pages ordered by predicted likelihood 
of decline, with the top N flagged as highest priority for a content editor to review.

## The Action Someone Could Take
A content team lead uses the ranked queue to decide which pages to manually 
review and refresh first, instead of reviewing pages in an arbitrary order 
(e.g., alphabetical, or "whatever was published longest ago").

## Cost of a Wrong Recommendation
- **False positive** (flagging a stable/growing page as "needs refresh"): wastes 
  editor time on a page that didn't need attention — a moderate cost, since editor 
  hours are limited and could have gone to a page that was actually declining.
- **False negative** (missing a page that is actually declining): the page keeps 
  losing visibility and traffic until it is caught in a later review cycle — 
  potentially a larger cost, since organic traffic loss compounds over time.

Because both error types have real cost, the queue is a **decision-support ranking**, 
not an automated action — a human reviews the top of the queue before anything changes 
on the site.

## Why This Is Not Just "Train a Model"
A model alone only produces a score. The useful part of this project is:
1. Confirming which signals are genuinely predictive (many common SEO assumptions, 
   like "high search volume = more traffic," turned out not to hold in this data).
2. Choosing a ranking that a human can act on and justify — not a black-box number.
3. Validating that the ranking generalizes across clients (client-holdout validation), 
   since a model that only works on the clients it was trained on isn't useful in practice.
4. Being explicit that patterns found here are *observed and directional*, not proof 
   of how any search engine's algorithm works.

## Note
This lane is provisional and may be confirmed or changed by the end of Week 4, 
per program guidelines.

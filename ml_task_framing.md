# ML Task Framing

## Lane
Content Refresh Prioritization

## Task Type
**Ranking / Scoring** (built on top of a binary classification model).

The underlying model predicts a probability score for each page ("likelihood this 
page is declining"). That score is not used as a final yes/no decision — it is used 
to **rank** all pages, so the top of the ranking becomes the review queue. This is why 
it's a ranking/scoring task rather than a pure classification task: the output that 
matters is the *order* of pages, not a single label per page.

## Target / Proxy
**Proxy label:** `is_declining_label`, derived from `trend_direction == "down"` 
(i.e., a page's traffic/visibility trend over the observation window).

This is a proxy, not a direct measurement of "this page needs a refresh" — a page 
can be declining for reasons unrelated to content quality (seasonality, a client 
site-wide issue, etc.), so the label is a reasonable but imperfect stand-in for the 
real goal.

## Success Metric
**Precision@K** (specifically Precision@20 and Precision@50): of the top K pages 
the ranking flags, what fraction are actually declining?

This metric was chosen over overall accuracy because:
- The review queue is only ever acted on from the top down (an editor won't review 
  all 30,000 pages) — so what matters is whether the *top* of the list is trustworthy.
- It directly measures wasted editor time: a low Precision@K means editors will 
  open pages that didn't need attention.

## What the Output Supports (the Action)
A ranked refresh-review queue that a content team lead uses to decide which pages 
to manually review and refresh first, replacing an arbitrary review order 
(alphabetical, oldest-published-first, etc.).

## Why ML Beats a Fixed Rule Here
This was tested directly in the Week 1 notebooks:

- A hand-written rule (`stale x visible`) scored **Precision@50 = 0.680**.
- A learned decision tree (depth=2, fully readable) scored **Precision@50 ≈ 0.60–0.74** 
  depending on validation setup, and the full random forest pipeline scored 
  **Precision@50 = 0.740** — beating the hand rule by roughly 3x on that metric using 
  client-holdout validation.
- Interestingly, the hand rule was *better* at Precision@20 (0.900 vs the tree's 0.550), 
  showing a fixed rule can still be strong at the very top of a list — but its advantage 
  disappears deeper into the ranking, which is exactly where a fixed rule runs out of 
  signal and a model keeps finding patterns across more features than a human would 
  reasonably encode by hand (impressions, position, content age, CTR, word count, 
  interacting in ways that are hard to write as a single rule).

So: a fixed rule is a reasonable *baseline* (and should always be kept as one, per the 
data contract), but a model captures multi-feature interactions a single rule can't, 
and that advantage compounds as the list gets longer than the top ~20.

## Note
Framing may be refined as more data/features become available; the core task type 
(ranking via a learned score, evaluated with Precision@K) is expected to stay stable 
through the capstone.


# Notes
Need to find a way to encode evolving features. <br>
Some variables are measured repeatedly across visits (`feature_v1`, `feature_v2`, ..., up to `v21`/`v22`).

## Two possible ways to deal with this
- treat them as normal features (probably not the best way)
- find a way to make use of the evolution of the different variables

# Strategy 
Rather than treating repeated measurements as separate features, try to encode each trajectory.

## Strategy 1: one encoded value per variable

Goal: compress each trajectory into a single score.

Possible single-score encodings:
- recency-weighted average (score putting more weight on more recent tests)
- slope over visits
- change between first and last value
- custom composite score combining:
  - level
  - evolution
  - recency
  - stability

Example idea:
- encoded_value = a * level + b * change + c * recent_value - d * variability

Pros:
- compact representation
- fewer features
- easy to plug into a model

Cons:
- information loss
- choice of formula/weights is arbitrary unless learned from data
- less interpretable if several components are mixed into one score

## Strategy 2: small set of encoded features per variable

Goal: keep a compact but richer summary of each trajectory.

Possible encoded features:
- level: first / last / mean
- evolution: slope or last-first
- recency: weighted mean favoring recent visits
- stability: standard deviation or range

Pros:
- keeps more information
- more interpretable
- easier to test which aspect is predictive

Cons:
- creates several features per variable instead of one

# Practical direction

If the priority is compactness, use one encoded value per variable. <br>
If the priority is preserving information, use a small set of encoded features per variable. <br>

A reasonable compromise is:
- one level feature
- one evolution feature
- one recency-aware feature
- one stability feature

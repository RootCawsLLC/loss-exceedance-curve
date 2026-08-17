# Loss Exceedance Curve

An interactive guide to reading a loss exceedance curve — the chart that answers
"what are the odds we lose more than $X?" Build a curve from a Monte Carlo
simulation, then drop real dollar thresholds onto it and read the probability
straight off the axis.

**Live:** https://rootcawsllc.github.io/loss-exceedance-curve/

![The tool with a UK financial-services data-breach benchmark loaded. The picker labels the shard "GBP · starter with source backed UK direct parameters · module governed · 6 medium", followed by a "not good for" caveat and a collapsed list of its six sources; the frequency and loss inputs are filled with that shard's ranges in pounds. Below, a histogram and a loss exceedance curve, both in pounds, with loss reserve at £3.4M crossing the curve at 41%, risk tolerance at £9.6M at 10%, and materiality at £18.3M near zero](preview.png)

## What it does

1. **Define a risk** — three-point estimates (min / most likely / max) for how
   often the risk occurs per year and how much each event costs. Either type
   your own, or load a source-backed benchmark: twelve shards across eight
   countries, each parameter naming the public source it came from, its
   confidence level, and the limitation on its use. Eleven are module-governed
   and state what they are *not* good for; one is evidence-backed only, meaning
   it clears the same evidence bar with no practitioner review behind it, and
   the picker says so rather than showing nothing. Loading a shard sets the
   currency too, so the curve, the axes, and every threshold read in its units.
2. **Run 10,000 simulations** — a PERT distribution for severity, Poisson for
   event counts, summed into an annual loss for each simulated year.
3. **Read the histogram** — the shape of the risk, plus P50 / mean / P90.
4. **Transform into the exceedance curve** — same data, different question.
   Hover anywhere on the curve for a plain-English probability statement.
5. **Add thresholds** — risk tolerance, loss reserve, materiality, and one
   threshold you name yourself. Each renders on the curve and generates a
   sentence you could put in a board deck. With a benchmark loaded these are
   pre-filled from the run itself (P90, P50, P99) rather than from fixed sample
   values, which would otherwise be off by orders of magnitude against a shard
   whose likely loss is six figures.

Amount inputs accept shorthand: `500K`, `5M`, `1.5B`, with or without a currency
symbol.

## Styling

Restyled to match the palette and typography of my portfolio:

| Role | Value |
| --- | --- |
| Paper | `#F7EBED` |
| Panel | `#FFFFFF` |
| Ink | `#3A3336` |
| Accent (rose) | `#8D5860` |
| Amber | `#9A6A24` |
| Sage | `#4C7359` |
| Display type | Fraunces |
| UI + body type | Inter |

The four threshold colors were remapped from the original's saturated
red/blue/orange/purple to muted tones that sit in the rose palette: brick
`#B04A44`, steel `#4A6B8A`, amber `#9A6A24`, plum `#7C5A86`.

## Running locally

No build step — it's one self-contained HTML file that pulls React 18 from a CDN.

```bash
python -m http.server 8000
```

Then open http://localhost:8000. Opening `index.html` directly from the
filesystem works too.

## A note on the model

This is a teaching demo, not a production risk model. PERT distributions have
hard upper bounds, while real cyber losses tend to be fat-tailed — so the tool
structurally underestimates tail risk. 10,000 iterations is fine for learning;
production models typically run 50,000–100,000 for stable tail estimates.

The benchmarks carry their own limits, and loading one does not launder them:

- **They are starting points, not benchmark-grade figures.** Each shard shows its
  own status; most are governed starters, meaning the evidence is real but the
  shard has not cleared human benchmark review.
- **Some frequencies are bridged from another country** where no local per-firm
  rate is published. Every bridged parameter says so in its own limitation.
- **A curve is only as good as its inputs.** A sourced range still produces a
  modelled distribution, not a measurement of your organisation. The benchmark
  tells you roughly where an industry sits; the gap between that and where you
  sit is the part worth arguing about.
- **The picker needs network access.** It reads the benchmarks file over HTTPS
  at load. If that fails the tool says so and still works on hand-entered
  numbers — nothing else depends on it.

## Attribution

Benchmark data comes from
[risk-benchmarks](https://github.com/RootCawsLLC/risk-benchmarks), which derives it
from [RiskShard](https://github.com/raviaxo/RiskShard) by
[raviaxo](https://github.com/raviaxo), AGPL-3.0. Figures, limitations and "not
good for" statements are RiskShard's, carried through unchanged. The underlying
publications are cited, not reproduced.

The file is fetched at runtime rather than copied into this repo, so there is
one copy of the data and one place to regenerate it.

## License

Copyright (c) 2026 RootCaws LLC.

[GNU AGPL v3 or later](LICENSE). If you modify this and run it as a network service, the AGPL requires you to offer your users the modified source under the same terms.

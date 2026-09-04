# Exceedance Workbench

Give one risk a frequency range and a loss range, simulate ten thousand years, and read the
probability of any annual loss straight off an exceedance curve. Then put your own amounts on it:
risk tolerance, loss reserve, materiality, and one line you name yourself.

**Live:** https://rootcawsllc.github.io/loss-exceedance-curve/

![The workbench with a UK financial-services data-breach benchmark loaded. On the left, the scenario picker shows the shard's badges (GBP, its maturity status, module governed, six medium-confidence parameters), a "not good for" caveat and its six sources expanded; the frequency and loss-per-event ranges are filled in pounds. On the right, a stat strip reads no loss in the typical year, an average of a few million, and bad and very bad years above; the exceedance curve below carries three flags where the suggested tolerance, reserve and materiality lines meet it, and a decision-lines table reads the odds off for each](preview.png)

## What it does

- **Inputs on the left, readout on the right.** A frequency range (events per year) and a loss
  range (per event), each as a low, likely and high. Amounts take shorthand such as `500K` or
  `2.5M`. Everything recomputes as soon as a value is applied; **Resample** reruns with fresh
  randomness.
- **Source-backed scenarios.** Instead of typing ranges, load one of twelve shards across eight
  countries. Each names the public source behind every parameter, its confidence, and the
  limitation on its use; each states what it is *not* good for. Loading a shard sets the currency
  as well, so the curve, the axes and every line read in its units.
- **Ten thousand simulated years.** A PERT draw for the annual rate, a Poisson count of events at
  that rate, and an independent PERT loss per event, summed. The same engine the other risk-lab
  tools use.
- **A stat strip**: typical year (median), average year, bad year (1 in 10), very bad year
  (1 in 100), and the share of years with no loss at all.
- **The exceedance curve**, with the probability on the left axis and the same odds as "1 in N" on
  the right. Hover or drag to read any point as a sentence. A segmented control switches to a
  histogram of the same years, with the median, 1-in-10 and 1-in-100 marked.
- **Decision lines.** Four rows in a table: tolerance, reserve, materiality, and a line you name.
  Each gets the chance per year that annual loss reaches it, the same figure as roughly one year
  in N, a flag on the curve, and a sentence. When both tolerance and reserve are set, a short
  reading compares them. **Suggest from this run** fills the three fixed lines from the current
  simulation's 1-in-10, median (or average when the median year has no loss) and 1-in-100 figures,
  which is also what happens automatically when a benchmark is loaded.

## Running locally

No build step: one self-contained HTML file that pulls React 18 from a CDN. Serve the directory
with any static server so the relative fetch of `risk-benchmarks.json` works, for example:

```bash
python -m http.server 8000
```

Then open http://localhost:8000. Opening `index.html` straight from the filesystem also works,
minus the benchmark picker.

## A note on the model

This is a teaching tool, not a production risk model. PERT has hard edges, so no simulated event
ever costs more than the high value entered, while real cyber losses are fat-tailed; the tool
therefore understates tail risk by construction. Ten thousand years is fine for learning. A
production model would use a fatter-tailed severity and run far more iterations before quoting a
1-in-100 figure.

The benchmarks carry their own limits, and loading one does not launder them:

- **They are starting points, not benchmark-grade figures.** Each shard shows its own status; most
  are governed starters, meaning the evidence is real but the shard has not cleared human
  benchmark review.
- **Some frequencies are bridged from another country** where no local per-firm rate is
  published. Every bridged parameter says so in its own limitation.
- **A curve is only as good as its inputs.** A sourced range still produces a modelled
  distribution, not a measurement of your organisation.
- **The picker needs the data file.** It reads `risk-benchmarks.json` at load. If that fails the
  tool says so and still works on hand-entered numbers; nothing else depends on it.

## Attribution

Benchmark data comes from
[risk-benchmarks](https://github.com/RootCawsLLC/risk-benchmarks), which derives it
from [RiskShard](https://github.com/raviaxo/RiskShard) by
[raviaxo](https://github.com/raviaxo), AGPL-3.0. Figures, limitations and "not
good for" statements are RiskShard's, carried through unchanged. The underlying
publications are cited, not reproduced. See [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).

## License

Copyright (c) 2026 RootCaws LLC.

[GNU AGPL v3 or later](LICENSE). If you modify this and run it as a network service, the AGPL requires you to offer your users the modified source under the same terms.

# A pull that depends on the stretch: state-dependent Hawkes excitation for the perpetual–spot basis

A linear Hawkes process of price moves on the Binance BTC perpetual and spot markets gets the structure of the basis right and its speed wrong. This note adds a small neural network that scales every kernel by the state of the basis at the parent event, and asks what it recovers and whether it trades better.

**Paper:** [paper/state-dependent-hawkes-basis.pdf](paper/state-dependent-hawkes-basis.pdf) (9 pages).
A plain-language explanation in Italian is in [paper/state-dependent-hawkes-basis-spiegazione.pdf](paper/state-dependent-hawkes-basis-spiegazione.pdf).

## What it finds

- **The linear model is too slow.** It splits a perpetual move into a permanent part and a transitory part that the spot later absorbs, but it reverts the transitory part two to seven times more slowly than the data, and it cannot pull back a deviation that existed before an event.
- **The state-dependent model.** Each kernel is multiplied by a positive factor that a network with 28 parameters computes from the signed deviation of the basis and the realised volatility. Conditional on the state path the model is a marked linear Hawkes process, so the likelihood stays exact and O(N).
- **Better out of sample.** It improves the out-of-sample likelihood in 56 of 60 four-hour windows on twelve days.
- **The same mechanism every day.** After a move that widens the basis the other leg follows more strongly and the process is locally critical; after a move that brings the basis back the other leg follows less.
- **Train for the decision.** As a signal, the likelihood-trained model has a slightly smaller edge per trade than the linear one and trades more often. Training the modulation with a forecast loss or a profit surrogate raises the gross profit per day two to three times. All profits are gross and below the fees of any Binance tier.

## Data and sources

Twelve days between December 2022 and September 2026, best quotes at microsecond resolution from Tardis.dev. The quotes are licensed and are not in this repository. The model code is part of the author's master thesis and is not public yet.

`paper/src/` holds the LaTeX sources of the paper and of the Italian explanation and the CSV files behind the figures. Build with `latexmk -xelatex paper.tex` from that folder.

## Limits

Hyperparameters were selected on one day that is also reported. The modulation curves beyond two standard deviations of the state are extrapolations, and the frozen-state closed forms are unreliable where the process is near critical.

This is research, not a trading recommendation. Based on the author's master thesis on Hawkes models of the perpetual–spot basis.

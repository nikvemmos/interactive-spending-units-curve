# How Much Can I Safely Spend?

An interactive, self-contained chart for exploring how much annual spending a pool of savings might support over a chosen planning horizon.

Open the live site: **[How Much Can I Safely Spend?](https://nikvemmos.github.io/interactive-spending-units-curve/)**

## The idea

The model starts with a simple question: *if I have this much capital, how much can I spend each year without running out too early?*

The chart is meant as a thinking tool, not a promise or a complete financial plan. It makes the main assumptions visible—annual spending, planning horizon, real return, taxes, a safety buffer, a target floor, and an end reserve—so they can be challenged and changed.

One spending unit is one year of the selected annual spending. For example, with annual spending of €20,000, €1,000,000 equals 50 spending units. The target floor always stays in spending units; the end reserve / bequest is always entered in euros.

## What the chart shows

- **Funding estimate:** the capital, expressed in spending units, needed today to fund spending through each horizon.
- **Chosen floor:** the minimum spending-unit level you want to preserve; it forms the lower edge of the target band.
- **Your plan:** the current capital divided by annual spending, shown at the selected planning horizon.
- **Projected balance:** a teal path that starts at “You are here” and rolls the selected return, withdrawal tax, and annual spending forward toward the five-year end of the chart.
- **Target band:** the space between the chosen floor and the funding estimate. It is a planning range, not a guarantee.

## Maths

Let:

- `S` = annual real spending (€)
- `C` = current capital (€)
- `n` = planning horizon in years
- `r` = expected real return
- `tᵣ` = tax drag on positive returns
- `t𝓌` = tax on withdrawals
- `b` = safety buffer
- `R` = end reserve / bequest (€)
- `F` = target floor in spending units

### Spending units

`current units = C / S`

The euro reserve is converted into the same unit system before it is added to the funding estimate:

`reserve units = R / S`

### Effective return and withdrawal gross-up

The model applies return tax only to positive real returns:

`r_eff = r × (1 − tᵣ)` when `r ≥ 0`

`r_eff = r` when `r < 0`

Withdrawals are grossed up for withdrawal tax:

`gross-up = 1 / max(0.05, 1 − t𝓌)`

### Funding estimate

The present value of one spending unit per year for `n` years is:

`annuity(n, r_eff) = n` when `r_eff = 0`

`annuity(n, r_eff) = [1 − (1 + r_eff)^−n] / r_eff` otherwise

The required spending units are:

`required units = [annuity(n, r_eff) × gross-up + reserve units / (1 + r_eff)^n] × (1 + b)`

The corresponding capital estimate is:

`required capital = required units × S`

The lower boundary is kept from crossing the funding estimate:

`lower boundary = min(F, required units)`

### Projected balance path

Starting from the current spending-unit balance:

`B₀ = C / S`

Each year of the teal path is projected as:

`Bₜ₊₁ = max(0, Bₜ × (1 + r_eff) − gross-up)`

This is intentionally simple: constant real spending, a constant real return, and no stochastic market path.

## Run locally

Open [`interactive_spending_units_curve.html`](./interactive_spending_units_curve.html) in a modern browser. There is no build step and no external dependency.

## Important limitation

This is an educational scenario model. It does not model sequence-of-returns risk, changing spending, pensions or other income, portfolio fees, asset allocation, longevity uncertainty, or jurisdiction-specific tax rules. Treat the output as a way to inspect assumptions—not as personal financial advice.

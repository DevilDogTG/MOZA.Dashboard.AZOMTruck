# ADR-0001: Compute display text in the dashboard, not in plugin mappings

**Date:** 2026-11-08
**Status:** accepted
**Deciders:** Supawat Tanmanee (maintainer), PR #3

## Context
AZOM Truck 1.2.0 shows the same gear letters as the SimHub dashboard Trucker Information: `N`, `R` or `R2`, crawler
`C1`/`C2`, and `DD`/`OD` on direct-drive and overdrive top gears. Trucker Information works these out with SimHub-side
JavaScript.

The wheel reads `v1/gameData/<name>` channels that the MOZA SimHub plugin publishes. Channels outside the plugin's
built-in catalog borrow a spare channel, and the plugin's own diagnostics say a borrowed channel is numeric and cannot
carry text. Mappings live in the plugin's settings rather than in this repo, and each re-uploaded `.mzdash` starts
with a fresh per-dashboard mapping set.

## Options Considered

### Option A: Produce the text in a mapping
Map a custom channel to a `js:` formula that returns the gear letter, as Trucker Information does.
- **Pros:** the logic is a copy of Trucker Information's.
- **Cons:** a borrowed channel cannot carry text, so the letter never reaches the wheel. The logic would also live
  outside version control.

### Option B: Reuse a built-in text channel
Bind the gear widget to an unused built-in string channel (for example `VehicleTag1`) and override its mapping.
- **Pros:** text can reach the wheel.
- **Cons:** the channel name says nothing about gears, and the dashboard silently depends on an unrelated
  override.

### Option C: Feed numbers and compute the text in the dashboard
Publish numeric facts (`Gear`, `ForwardGearCount`, `ReverseGearCount`, `GearRatioType`) and build the letter in the
`.mzdash` binding JavaScript.
- **Pros:** works within the numeric limit; the logic is versioned and reviewed with the dashboard; mappings stay
  plain property paths or small formulas.
- **Cons:** one value (`GearRatioType`, whether a top gear is direct drive or overdrive) still needs a `js:` mapping,
  because the gear-ratio property name depends on the current gear.

## Decision
Use Option C: channels carry numbers, and any text the dashboard shows is computed in the `.mzdash` bindings,
because borrowed channels cannot carry text.

## Consequences
**Easier:** dashboard logic is versioned, testable with mock telemetry, and survives re-uploads. Only the mappings
need re-entering.
**Harder:** each new piece of text needs its inputs exposed as numeric channels. Numeric codes such as
`GearRatioType` (0 normal, 1 DD, 2 OD) must be documented next to their mapping.
**Follow-up:** keep the README's channel-mapping table in step with every channel the dashboard reads.

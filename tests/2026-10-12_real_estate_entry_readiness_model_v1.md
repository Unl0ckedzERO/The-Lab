# Real Estate Entry Readiness Model — v1

Date: 2026-10-12  
Zone: Incubator  
Stage: Evidence Pass / Audit  
Status: Active baseline model  
Source handoff: `handoffs/2026-10-12_intake_to_incubator_real_estate_entry_readiness.md`

## Purpose

Build a conservative decision model for determining when real estate deserves active property research and, later, when an individual property deserves a buy / wait / reject decision.

This model does **not** assume that buying is better than renting, that leverage is inherently good, that owner-occupied multifamily is the best strategy, or that appreciation will rescue weak economics.

The FHA multifamily Reel that triggered this lane is evidence item 1, not the thesis.

## Data Boundary

Keep exact user income, assets, debts, credit details, lender details, addresses, account balances, and other sensitive personal financial information in chat.

This repo may store only:

- reusable formulas and underwriting methods,
- sanitized scenario assumptions,
- strategy comparisons,
- generic thresholds,
- authoritative public financing rules,
- route decisions,
- and future sanitized test results.

## Model Architecture

Use five dimensions totaling 100 points, but apply **hard gates before score**.

| Dimension | Weight | What It Tests |
|---|---:|---|
| Capital readiness | 25 | Can the purchase be funded without destroying liquidity or emergency resilience? |
| Financing readiness | 15 | Is at least one financing path realistically available on verified terms? |
| Deal economics | 30 | Does the strategy/property work under conservative operating and downside assumptions? |
| Market readiness | 15 | Does the target market contain legal, insurable, rentable properties at workable price/rent relationships? |
| Operator readiness | 15 | Can the owner realistically live with and operate the strategy? |

A high score cannot override a hard-gate failure.

## Hard Gates

A strategy or property cannot become a Buy Candidate if any of the following is true:

1. **Closing consumes the safety reserve.**
   - Minimum internal screening standard: after closing, preserve at least three months of essential non-property living expenses **plus** three months of full property carrying cost.
   - Preferred readiness: six months of each.
   - Do not double-count the subject property's housing payment inside non-property living expenses.

2. **The financing path depends on unverified or ineligible income.**
   - Use only lender/program-eligible rent for qualification.
   - Separate "qualifying rent" from "economic rent" in every model.

3. **The base case requires appreciation, refinancing, rent growth, or tax benefits to justify the purchase.**
   - Those may be upside cases, not the reason the deal works.

4. **The stress case creates a credible forced-sale risk.**
   - The model must show how the property survives lower rent, vacancy, higher costs, and a repair shock.

5. **The unit, rental use, ADU, or room-rental plan is not clearly legal and insurable.**

6. **The ownership/tenant arrangement is incompatible with the operator.**
   - A financially attractive roommate strategy is still a reject if the owner will not tolerate roommates.
   - A fourplex is not "better" if the tenant-management burden is unacceptable.

7. **The expected holding period is too short for transaction friction unless the exit math specifically proves otherwise.**
   - Default screening posture: assume a multi-year hold and explicitly model selling costs rather than relying on quick appreciation.

## Minimum User-Specific Inputs

The model should not request statements, account numbers, exact addresses, or full credit reports during baseline screening.

Minimum conversational inputs:

| Input | Minimum Form Needed | Why |
|---|---|---|
| Stable gross monthly income | Approximate amount/range + employment stability | Financing and affordability |
| Recurring monthly debt payments | Approximate total + any material fixed obligations | Financing and monthly resilience |
| Liquid cash available | Amount the user is actually willing to deploy | Capital gate |
| Reserve floor | Minimum cash the user refuses to spend | Prevent over-deployment |
| Credit band | Approximate band; exact report not needed | Loan-path realism |
| Current housing cost | Rent/housing + utilities range | Buy-vs-wait benchmark |
| Location flexibility | Metro/county/region only | Market and legal screening |
| Occupancy tolerance | Duplex/multifamily, roommates, ADU, privacy needs | Strategy fit |
| Operator tolerance | Tenant interaction, repairs, bookkeeping, emergencies | Operator gate |
| Likely hold horizon | Years / uncertainty | Transaction-risk and exit modeling |
| Primary goal | Housing-cost reduction, cash flow, long-term equity, business scaling, or mixed | Strategy ranking |

## Conservative Scenario Set

### Scenario A — Base Conservative

Use for normal underwriting before making a purchase decision.

- Rent: start from verified comparable rent, not asking rent alone.
- Rent level: use the lower end of credible comps or apply a small haircut to the central estimate.
- Vacancy/credit loss: 5%.
- Maintenance reserve: 5% of gross scheduled rent.
- Capital-expenditure reserve: 5% of gross scheduled rent.
- Management: include an 8% economic management allowance even when self-managing so the model does not mistake unpaid labor for free operations.
- Taxes: use current/reassessed estimate where available.
- Insurance: use an actual quote before property-level approval.
- Utilities: include every owner-paid utility.
- HOA/fees: include in full.
- Appreciation: 0%.
- Rent growth: 0%.
- Tax benefit: 0 in the go/no-go case.

### Scenario B — Stress

Use to test resilience.

- Rent: 10% below the already-conservative base rent.
- Vacancy/credit loss: 10%.
- Maintenance reserve: 8% of gross scheduled rent.
- Capital-expenditure reserve: 8% of gross scheduled rent.
- Management: 10% of collected rent.
- Property tax: +10% above current estimate unless reassessment is already captured.
- Insurance: +20% above current quote/estimate.
- Owner-paid utilities: +10%.
- Appreciation: -10% exit-value shock for downside review, not annual forecast.
- Financing: if rate is not locked, also test payment at +1 percentage point interest.

### Scenario C — Liquidity Shock

Do not annualize this into normal operations. Test whether cash reserves can absorb:

- one material repair/replacement,
- an extended vacancy or nonpayment event,
- and the full property payment during that disruption.

Before inspection, use a generic repair placeholder. Replace it with property-specific roof, sewer, foundation, HVAC, electrical, plumbing, and other major-system exposure before Buy Candidate status.

## Core Formulas

### Upfront Capital

`cash_required = down_payment + buyer_closing_costs + immediate_repairs + moving/setup + initial_property_reserve`

`post_close_liquidity = deployable_liquid_cash - cash_required`

### Rental Operations

`effective_rent = scheduled_market_rent × (1 - vacancy_rate)`

`NOI = effective_rent - taxes - insurance - owner_paid_utilities - maintenance_reserve - capex_reserve - management_allowance - HOA - other_operating_costs`

Debt service is excluded from NOI.

### Owner-Occupied Strategy

For house hacks and owner-occupied multifamily, do not require the tenant units to create standalone "profit" before considering the value of replaced housing expense.

Track separately:

`net_owner_housing_cost = debt_service + owner_property_costs + reserves - collected_tenant_rent`

`housing_cost_advantage = comparable_alternative_housing_cost - net_owner_housing_cost`

Also calculate property-level NOI and equity buildup separately. Do not hide weak property economics inside personal housing savings.

### Non-Owner-Occupied Investment

`DSCR = NOI / annual_debt_service`

Internal screening target:

- Base case: prefer DSCR >= 1.20.
- Stress case: prefer DSCR >= 1.00.

These are Lab decision thresholds, not universal lender requirements.

### Cash-on-Cash

`cash_on_cash = annual_pre_tax_cash_flow / total_cash_invested`

For owner-occupied strategies, show cash-on-cash and housing-cost savings separately rather than combining them into one flattering return number.

## Scoring Rubric

### 1. Capital Readiness — 25

| Score Range | Interpretation |
|---|---|
| 0–9 | Cannot close safely or would exhaust liquidity |
| 10–16 | Technically fundable but thin reserve / repair resilience |
| 17–21 | Adequate capital with minimum reserve intact |
| 22–25 | Strong reserve, repair buffer, and flexibility after closing |

### 2. Financing Readiness — 15

Award points for:

- verified eligible financing path,
- realistic rate/payment range,
- verified treatment of rental income,
- verified mortgage insurance/reserve/loan-limit requirements,
- and lender-level confirmation before active offer activity.

### 3. Deal Economics — 30

Award points for:

- base-case economics,
- stress-case survival,
- rent-vs-buy or wait benchmark,
- downside/exit math,
- and lack of dependence on appreciation/refinancing.

### 4. Market Readiness — 15

Award points for:

- legal strategy availability,
- sufficient comparable rents,
- credible tax/insurance estimates,
- reasonable supply/liquidity,
- and acceptable hazard/insurance exposure.

### 5. Operator Readiness — 15

Award points for:

- tenant/roommate tolerance,
- time and emergency-response capacity,
- maintenance capability or vendor budget,
- bookkeeping/legal/admin willingness,
- and lifestyle/holding-period fit.

## Buy / Wait / Reject

These are internal Lab classifications, not directives.

### Buy Candidate

Requires:

- all hard gates passed,
- total score >= 80/100,
- base and stress cases completed,
- no dependence on appreciation/refinancing/rent growth,
- real insurance and tax estimates,
- verified legal use,
- verified financing path,
- and post-close liquidity above the hard minimum.

"Buy Candidate" means worthy of final diligence and negotiation, not automatic purchase.

### Wait / Renegotiate

Use when:

- score is 65–79,
- a hard gate is currently failing but plausibly fixable,
- capital is close but reserves are thin,
- financing needs verification,
- market evidence is incomplete,
- price must fall,
- or the strategy works only after a realistic change in conditions.

Every Wait result should state exactly what would need to change.

### Reject

Use when:

- score < 65,
- a non-fixable hard gate fails,
- the economics require optimistic rent/appreciation,
- the rental plan is illegal/uninsurable,
- downside creates forced-sale risk,
- or operator/lifestyle fit is clearly wrong.

Reject applies to the strategy/property at current terms, not necessarily to real estate forever.

## Strategy Comparison — Initial Posture

| Strategy | Capital Efficiency | Revenue Potential | Complexity | Main Advantage | Main Risk / Gate | Initial Posture |
|---|---|---|---|---|---|---|
| Owner-occupied duplex | High | Medium | Medium | Rent can materially reduce personal housing cost | Rent quality, tenant proximity, repair concentration | Strong candidate to model |
| Owner-occupied triplex/fourplex | Very high | High | High | More income streams against one owner-occupied purchase | FHA 3–4 unit self-sufficiency; more tenant/maintenance burden | Model, never assume superior |
| Single-family room rental / house hack | High | Medium | Medium | Simpler property type and flexible exit to normal owner occupancy | Privacy/roommate fit; local occupancy/lease rules | Strong low-complexity comparison |
| Existing legal ADU | High | Medium | Medium | Separate rental unit while retaining one-unit property form | Legal status, appraisal, utility/insurance treatment | Strong comparison when existing/legal |
| ADU conversion/build | Variable | Medium | High | Can manufacture rentable space/equity | Permits, build cost, delay, contractor risk | Separate project; do not assume economics |
| Conventional owner-occupied home | Medium | Low initially | Low | Lower operator burden; future optionality | May build wealth but does not create current business revenue | Benchmark, not default |
| Later investment property | Lower | High | High | Cleaner business/investment economics | More equity/capital generally required; no owner-occupancy subsidy | Later-stage comparison |
| Wait / build capital | Highest liquidity | None now | Low | Preserves optionality and can improve financing/reserves | Opportunity cost / market movement | Always keep as explicit benchmark |

## Current Financing Evidence — 2026-10-12

These are current public-rule anchors only. Actual lender overlays and automated-underwriting results may be more restrictive.

### FHA

- HUD Handbook 4000.1 permits up to 96.5% LTV on standard purchase transactions for borrowers eligible for maximum financing; current FHA guidance states borrowers with a Minimum Decision Credit Score at or above 580 are eligible for maximum financing.
- For three- and four-unit properties, FHA applies a self-sufficiency test using appraiser market rents for all units and subtracting the greater of the appraiser's vacancy/maintenance estimate or 25% of fair market rent.
- For subject-property rental income with limited/no history, FHA generally uses 75% of the lesser of appraiser fair-market rent or the lease amount.
- For a one-unit property with an ADU and limited/no ADU rental history, ADU rental income used for qualification cannot exceed 30% of total monthly effective income.
- FHA generally charges a 1.75% upfront mortgage insurance premium on purchase loans plus annual MIP.

### Conventional / Freddie Mac Anchor

Freddie Mac's current conforming table allows up to 95% LTV for eligible automated-underwritten 2-, 3-, and 4-unit primary-residence purchases; manually underwritten and super-conforming limits are lower. Freddie also states rental income from other units may be included in qualifying income subject to its Guide.

For investment properties, Freddie Mac's current table lists maximum LTVs of 85% for one-unit and 75% for two- to four-unit investment-property purchase/no-cash-out transactions. These are secondary-market eligibility ceilings, not a promise that any lender will offer those exact terms.

## Evidence Needed Before Active Property Research

Do **not** begin property listings merely because financing is possible.

Advance toward active listing research only when all of the following are true:

1. **Capital envelope exists.**
   - A realistic purchase-price band can be funded while preserving the hard reserve.

2. **At least one financing path is credible.**
   - Program rules have been checked and a lender-level qualification/payment range is realistic enough to model.

3. **Operator fit is positive.**
   - At least one of multifamily, room rental, ADU, or conventional ownership is actually acceptable to the user.

4. **Market-level evidence says the strategy is plausible.**
   - Aggregate prices/rents, taxes, insurance, legal rules, and supply do not make the strategy structurally impossible.

5. **The generic stress envelope survives.**
   - The strategy does not need appreciation or perfect occupancy merely to remain solvent.

6. **Waiting has been modeled as a real alternative.**
   - The model states what additional capital, income, rate change, price change, or rent relationship would make waiting superior/inferior.

Once these conditions pass, property research should begin as a bounded evidence phase, not as a shopping exercise.

## Next Incubator Steps

1. Collect the minimum user-specific inputs in chat only.
2. Build sanitized price-band scenarios rather than searching listings.
3. Compare financing paths at the same purchase prices.
4. Run a market-level screen for one or two plausible target geographies.
5. Decide whether any strategy earns the right to active property research.
6. If yes, create a property-underwriting template and begin candidate research.
7. If no, record a Wait state with explicit trigger conditions.

## Authoritative Public Sources Used for Baseline

- HUD Single Family Housing Policy Handbook 4000.1 / current updates
- HUD FHA Mortgage Insurance Premium guidance
- Freddie Mac Single-Family maximum LTV guidance
- Freddie Mac 2- to 4-unit property guidance
- Consumer Financial Protection Bureau homebuying affordability/down-payment guidance

Re-check program rules before any real financing or offer decision.

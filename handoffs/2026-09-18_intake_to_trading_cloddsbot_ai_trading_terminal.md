# Handoff — CloddsBot AI Trading Terminal Repo Audit

Date: 2026-09-18
Source Zone: Intake
Target Project: Trading / Pine / AI Trading Workflow
Item Type: Tool / Public GitHub Repo / Trading Agent
Status: Existing Project Handoff — Repo-Only Inspection Recommended

## Why This Passed Intake

- The user starred `alsk1992/CloddsBot` and asked to route it to the trading project for further inspection.
- The repo aligns with the user's trading/TradingView/Pine and AI-agent workflow interests.
- The tool is not just a generic trading-content claim; it is a public repo with a large TypeScript codebase, docs, install instructions, package metadata, security docs, and cross-repo copies/forks.
- The tool is high-risk because it claims to connect AI agents to prediction markets, crypto spot, perpetual futures, wallets, token launches, copy trading, MCP, and messaging platforms. This should be inspected inside the trading project, not installed casually from Intake.

## Primary Repo

Primary target:

- `alsk1992/CloddsBot`

Cross-check repo:

- `ai-trading-terminal/cloddsbot`

Initial read: treat `alsk1992/CloddsBot` as the primary/canonical repo for inspection. The `ai-trading-terminal/cloddsbot` repo appears related but weaker as the current source of truth based on repo size, README/version differences, and the primary repo's package metadata.

## What Is Known From Intake

The `alsk1992/CloddsBot` README presents Clodds as an AI-powered trading terminal for prediction markets, crypto spot, perpetual futures, token launches, and Bittensor subnet mining. It claims support for natural-language interaction across many messaging channels, prediction markets, futures exchanges, Solana/EVM integrations, and a local WebChat UI.

Notable claimed capabilities:

- Prediction markets: Polymarket, Kalshi, Betfair, Smarkets, Drift, Manifold, Metaculus, PredictIt, Opinion.xyz, Predict.fun, and AgentBets-like references.
- Perpetual futures: Binance, Bybit, Hyperliquid, MEXC, Drift, Percolator, Lighter.
- Solana/EVM integrations: Jupiter, Raydium, Orca, Meteora, Kamino, MarginFi, Solend, Pump.fun, Bags.fm, Uniswap, 1inch, Virtuals Protocol, and related DeFi tools.
- Trading functions: order execution, portfolio tracking, P&L, DCA, copy trading, whale tracking, arbitrage detection, smart routing, risk engine, backtesting, and trade ledger.
- AI/tooling functions: Claude as primary model, multiple LLM providers, agents, semantic memory, MCP server, bundled skills, chat/WebChat, and many messaging channels.
- Security/safety claims: circuit breakers, loss limits, audit logs, credential encryption, unsafe sandbox disabled by default, MCP allowlist/blocklist/profile controls, and security-audit documentation.

Package metadata inspected in Intake:

- Package name: `clodds`
- Version seen: `1.9.1`
- Runtime: Node `>=22.0.0`
- Main repo field points to `https://github.com/alsk1992/CloddsBot`
- `postinstall` script runs local scripts and attempts to cache an embedding model.

The `.env.example` shows that the tool can request many sensitive values, including:

- `ANTHROPIC_API_KEY`
- messaging bot tokens
- Polymarket API credentials and private key
- Kalshi keys
- Metaculus/Manifold keys
- gateway tokens
- credential encryption keys
- escrow keys
- treasury wallet
- webhook secrets
- Bittensor wallet-related settings

## Intake Judgment

- Interesting tool signal: high
- Immediate install/run candidate: low
- Trading-profit evidence: very low
- Repo-only audit candidate: high
- Fit for trading/agent Watchlist: high

Suggested Intake route:

> Watchlist → Existing Project Handoff to Trading project for high-risk repo-only inspection.

## Key Warning

Do not connect exchange accounts, wallets, private keys, messaging bots, MCP full access, or trading credentials during first inspection.

Do not treat the repo as evidence that AI trading is profitable.

Treat it as a candidate AI trading cockpit / research terminal pattern until code, permissions, defaults, and execution gates are inspected.

## Suggested Trading Project Inspection Plan

### Phase 0 — Repo-Only Static Audit

Goal: determine whether the tool can be safely evaluated in read-only or dry-run mode.

Inspect:

- `README.md`
- `package.json`
- install / postinstall scripts
- CLI entrypoint and onboarding flow
- `.env.example`
- credential storage and encryption implementation
- exchange / wallet credential loading paths
- default trading mode: dry-run vs live
- where and how orders are submitted
- risk engine and kill-switch implementation
- MCP server implementation and exposed tools
- MCP default profile and enforceability of `read-only`
- allowlist/blocklist behavior
- messaging-channel pairing and owner controls
- WebChat auth and local gateway exposure
- any token-launch, copy-trading, Bittensor, and machine-payment modules
- dependency footprint and known risky packages
- security audit claims vs actual code

Success criteria:

- Clear evidence that read-only / dry-run mode is enforceable.
- Clear path to run without private keys, exchange credentials, or wallet access.
- No install-time behavior that meaningfully increases risk without user approval.
- MCP tools can be restricted to non-trading functions.
- Trading execution is gated by explicit credentials and user action.

Stop conditions:

- Live-trading or wallet-moving functions can be triggered by default.
- Private keys are required for basic exploration.
- MCP defaults expose trading/execution tools without reliable restriction.
- Install scripts perform network, wallet, or system modifications that are not clearly justified.
- Security claims are not supported by code.

### Phase 1 — Sandbox / Read-Only Local Probe Only If Phase 0 Passes

If static audit passes, consider a sandbox probe:

- Use a disposable local folder or VM/container.
- No real wallets.
- No exchange API keys.
- No private keys.
- No messaging bot tied to personal accounts.
- No MCP install into primary Claude/Codex environment.
- Prefer dependency inspection or `npm install --ignore-scripts` before any normal install.
- Test only docs, CLI help, config validation, read-only market-data paths, and dry-run behavior.

### Phase 2 — Paper / Simulated Workflow Only

Only if Phase 1 is clean:

- Build a paper-trading or read-only research workflow.
- Focus on market comparison, journaling, risk-review, and position-review functions.
- Do not execute live trades.
- Do not test copy trading, token launches, Bittensor mining, or agent-to-agent payments first.

## Useful Pattern To Preserve

AI trading terminal / agent cockpit:

```text
unify market data
→ compare prediction markets / order books
→ track positions and risk
→ journal decisions
→ run dry-run strategies
→ restrict execution until the user explicitly enables it
```

This pattern may be useful even if CloddsBot itself is not safe or practical to run.

## Questions For The Trading Project

1. Is CloddsBot actually safer or more useful than building a smaller custom trading research/journaling workflow?
2. Can read-only mode be enforced technically, not just promised in docs?
3. Are the trading connectors real and maintained, or mostly marketing surface?
4. Is the MCP integration safe enough to expose to Claude/Codex at all?
5. Does the repo offer reusable ideas for the user's TradingView/Pine workflow without using its live execution layer?
6. Should the project test only the concept/pattern rather than the repo itself?

## Suggested Opening Prompt For Trading Project

```text
This is a handoff from The Lab Intake. Please inspect the public GitHub repo `alsk1992/CloddsBot` as a high-risk AI trading terminal candidate.

Use the Lab handoff at:

handoffs/2026-09-18_intake_to_trading_cloddsbot_ai_trading_terminal.md

Goal: perform a repo-only safety and usefulness audit before any install or run. Treat this as a trading research/tooling pattern, not trading advice or profit evidence. Do not connect real wallets, exchange accounts, private keys, messaging bots, or MCP full access. First determine whether read-only or dry-run evaluation is technically enforceable, then decide whether a sandbox probe is justified.
```

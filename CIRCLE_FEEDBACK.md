# Circle Product Feedback — AnchorPay

## Which Circle products we used

- **Arc** — the L1 network the entire app is deployed and transacts on
- **USDC** — used as the native gas token for every transaction (deposits, withdrawals, bill payments, remittances, group contributions)

## Why we chose these products for our use case

Arc's native USDC gas model was the deciding factor. Our core pitch is protecting users from PKR inflation and making cross-border payments cheap — having transaction fees themselves be predictable and dollar-denominated (instead of a separate volatile gas token) directly reinforces that pitch instead of undermining it. Sub-second finality also matters for a payments app: users expect a bill payment or remittance to confirm close to instantly, not after a multi-second or multi-block wait.

## What worked well during development

- **Standard EVM tooling worked out of the box.** Remix, MetaMask, `eth_sendTransaction`, `eth_getBalance` — everything behaved exactly like a normal EVM chain, so there was no new mental model to learn beyond the gas token being USDC instead of a native chain token.
- **Sourcify verification was fast and reliable.** Deploying and verifying a contract took minutes, and Arc's own explorer (arcscan, Blockscout-based) picked up the Sourcify verification automatically without a second manual step.
- **Documentation at docs.arc.io was clear** on the one detail that actually matters most and is easy to get wrong: native USDC uses **18 decimals**, while the USDC ERC-20 token interface uses **6 decimals**. Getting this backwards would silently break every transaction amount, so having it stated explicitly and prominently was valuable.

## What could be improved

- **App Kit (Send / Swap / Unified Balance) requires a full npm/bundler setup**, which is a real jump from a single static HTML file. For teams starting as a simple frontend prototype (as we did), there's no lightweight path to try App Kit's Send/Swap primitives without first committing to a Vite/webpack project structure. A CDN-loadable or vanilla-JS-friendly version of App Kit's core functions would lower the barrier for early-stage/no-build prototypes considerably.
- **Guidance on "spend"-style contract patterns for native USDC** would help — we initially built several features (bill pay, top-ups) using self-send transactions that don't actually change wallet balance, since it wasn't immediately obvious from the docs that value must go to a *different* address to genuinely move. A short example or note in the docs about this would save other early builders the same debugging cycle.

## Recommendations to make the developer experience more seamless

1. A lightweight, vanilla-JS/CDN-friendly entry point for App Kit's Send and Swap functions, for teams prototyping before committing to a full build pipeline.
2. A short "common pitfalls" page covering the native-vs-ERC20 decimals distinction and the self-send-doesn't-move-balance gotcha — both are easy first mistakes that cost real debugging time.
3. Clearer guidance (or a naming-check tool) on what's allowed in project names relative to Arc's own branding, ideally surfaced during initial project registration rather than discovered after building around a name.

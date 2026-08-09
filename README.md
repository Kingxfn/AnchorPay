# AnchorPay

A PKR-to-USDC savings and cross-border payments app built on **Arc** (Circle's L1). AnchorPay protects money from Pakistani Rupee inflation and makes cross-border payments to/from Pakistan instant and cheap — $0.01 flat fee, under 1 second, instead of $25-40 bank wires taking 3-5 days.

**Live app:** https://anchorpay-xyz.vercel.app
**App (skip landing page):** https://anchorpay-xyz.vercel.app/app

---

## What it does

- **Instant Convert** — swap PKR ↔ USDC anytime at a live rate, 0.1% fee
- **Send Money From Abroad** — cross-border USDC transfer, $0.01 flat fee, under 1 second
- **Bill Pay, Mobile Top-Up & QR Pay** — spend directly from a USDC balance
- **Saving Goals** — named goals with a target amount and live progress tracking
- **Group / Family Savings** — an onchain version of a savings committee (BC), with each member's contribution tracked individually
- **Milestones + ArcPoints** — every transaction earns points; users move through tiers

---

## Tech stack

- **Frontend:** Vanilla HTML/CSS/JavaScript, no build step, no framework — two static files (`index.html` = landing page, `app.html` = the app)
- **Wallet connection:** EIP-6963 multi-wallet discovery (MetaMask, OKX Wallet, and any other injected wallet)
- **Chain:** Arc Testnet, chain ID `5042002`
- **Smart contract:** Solidity `^0.8.24`, compiled with `solc 0.8.34`
- **Hosting:** Vercel (static hosting, `vercel.json` sets clean URLs)

---

## Circle / Arc integration

AnchorPay uses **Arc** as its settlement layer and **USDC** as the native gas token for every transaction — deposits, withdrawals, bill payments, remittances, and group contributions are all denominated and paid for in USDC, with no separate volatile gas token.

The savings/goals/group-pot logic is implemented in a standalone Solidity contract, **`AnchorPay.sol`**, deployed and verified on Arc Testnet:

| | |
|---|---|
| Contract | `AnchorPay` |
| Address | `0x0BEB2F40229ecd95D7f8192Cb7e421d7525Db736` |
| Network | Arc Testnet (chain 5042002) |
| Verified on | [Sourcify](https://repo.sourcify.dev) (Exact Match) and [Arc Explorer](https://testnet.arcscan.app/address/0x0BEB2F40229ecd95D7f8192Cb7e421d7525Db736) |

**Contract functions:**
- `deposit()` / `withdraw(uint256)` — personal USDC balance
- `createGoal(string, uint256)` / `fundGoal(uint256)` / `withdrawFromGoal(uint256, uint256)` — Saving Goals
- `createGroup(string)` / `contributeToGroup(uint256)` / `withdrawFromGroup(uint256, uint256)` — Group/Family Savings, with per-member contribution tracking
- `spend(address, uint256, string)` — generic spend used for Bill Pay, Top-Up, and Remittance, tagged by category for transaction history

---

## Running it locally

No build step needed — it's static HTML/JS.

```bash
git clone https://github.com/Kingxfn/AnchorPay.git
cd AnchorPay
python3 -m http.server 8000
# open http://localhost:8000        -> landing page
# open http://localhost:8000/app.html -> the app
```

You'll need an injected wallet (MetaMask or similar) with Arc Testnet added, and testnet USDC from the [Arc faucet](https://faucet.circle.com) to actually use the app's features.

---

## Deploying the contract yourself

1. Open [Remix IDE](https://remix.ethereum.org)
2. Create `AnchorPay.sol` and paste in the contract source (in this repo)
3. Compile with Solidity `0.8.24+`
4. Under **Deploy & Run Transactions**, set Environment to **Injected Provider** with your wallet connected to Arc Testnet
5. Deploy, then verify on [Sourcify](https://repo.sourcify.dev) or via the Remix Sourcify plugin

---

## Status

This is a **testnet build**. The frontend currently mirrors the contract's savings/goals/group-pot logic client-side for the live demo; wiring the frontend directly to the deployed `AnchorPay.sol` contract for every action is the next planned step before considering mainnet.

## Circle Product Feedback

See [`CIRCLE_FEEDBACK.md`](./CIRCLE_FEEDBACK.md) for detailed feedback on the Circle/Arc developer experience.

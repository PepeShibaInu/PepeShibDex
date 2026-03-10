# PepeShibDex — Native Cross-Chain Swap

PepeShibDex is a non-custodial cross-chain swap aggregator that enables end-to-end token swaps across EVM chains and Solana directly from the browser, with wallet connection and on-chain transaction signing — no redirects to third-party interfaces.

## Supported Networks

**EVM:** Ethereum, BNB Smart Chain, Polygon, Arbitrum, Optimism, Base, Avalanche, Sonic, zkSync Era, Linea, Blast, Scroll, Mode, Unichain, World Chain, Gnosis, Flow EVM, Abstract, Cronos, Berachain, BOB, HyperEVM, Mantle, Sophon, Sei, Plasma, Monad.

**SVM:** Solana.

## How It Works

1. **Connect Wallet** — Users connect an EVM wallet (MetaMask, WalletConnect, or any injected provider) or a Solana wallet (Phantom, Solflare). Multiple wallets can be connected simultaneously for cross-family swaps.

2. **Select Route** — Choose source chain/token and destination chain/token. The UI fetches live market prices from CoinGecko, CoinCap, CryptoCompare and other providers, and computes the expected output with real-time exchange rates.

3. **Auto-Routing** — The backend queries multiple bridge/DEX providers in parallel and selects the best available route:
   - **deBridge DLN** — primary provider for cross-chain orders, supports all listed chains.
   - **Relay** — low-latency cross-chain bridge for major EVM chains + Solana.
   - **LI.FI (Jumper)** — multi-bridge aggregator with broad EVM + Solana coverage.
   - **Across** — optimistic bridge for core EVM L1/L2 chains.

   The routing engine compares quotes by expected output, fees, and estimated duration, then selects the best candidate automatically.

4. **Tax & Fee Calculation** — A transparent fee structure is applied before execution:
   - Platform tax (0.15%)
   - Protocol fee (0.05%)
   - Bridge infrastructure fee (0.08%)

   Fees are collected as a separate on-chain transaction on the source chain before the swap.

5. **Execution** — The swap is executed natively:
   - The backend calls the winning provider's API to generate a ready-to-sign transaction.
   - The frontend sends the transaction to the user's wallet for signing via `eth_sendTransaction` (EVM) or `signAndSendTransaction` (Solana).
   - Token approvals are handled automatically when required.
   - The source transaction hash is recorded and tracked.

6. **Status Tracking** — After submission, the app polls the provider's status API to track the cross-chain order until completion, displaying real-time updates.

## Architecture

| Component | Description |
|-----------|-------------|
| `server.mjs` | Node.js HTTP server — serves static files and exposes the swap API (`/api/quote`, `/api/route`, `/api/tax-intent`, `/api/execute-swap`, `/api/swap/:id/status`). Handles provider API calls, quote caching, and market price aggregation. |
| `pepeshibdex/index.html` | Single-page frontend — wallet connection (EVM + Solana + Tron), swap UI, transaction signing, and status tracking. No build step required. |
| `index.html` | Dashboard landing page linking to PepeShibDex and other PepeShib ecosystem pages. |

## Running Locally

```bash
npm start
```

Opens at `http://127.0.0.1:8080`. The server binds to `PORT` env variable (default 8080).

## Deployment

Configured for Render via `render.yaml`. The API is deployed at `https://pepeshib-api.onrender.com`.


WEB: https://pepeshibainu.com
TG: https://t.me/PepeShib_Bsc
𝕏: https://x.com/RealPepeShib
Email: team@pepeshibainu.com

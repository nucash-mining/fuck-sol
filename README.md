# fuck-sol

Live on-chain surveillance of the bot swarms trading a pump.fun token.

**Watch it:** https://nucash-mining.github.io/fuck-sol/

## Currently watching

**DCA — Dollar Cat Average** (`DPvD1PfJETKBV7hEJkJG68zR9kM9YuoCccHFPRSFpump`), PumpSwap pool `GH68kEZAiVEwL8636XxhR1BuuNb471kEp1UAweSrtbsZ`.
The wallet labels below were decoded on a different token the same week — these
operators work whatever pool is moving, so expect to see them here too.

## What this is

One night of manual on-chain forensics, turned into a dashboard. It reads the
PumpSwap pool directly from Solana — price, liquidity, and every trade — and
labels the wallets behind those trades with what they were actually observed
doing, not what a token-scanner claims about them.

There is no server and no backend. The page is a single HTML file; your browser
talks to a Solana RPC endpoint and nothing else. Nothing is collected, nothing
is stored except your chosen RPC URL and a local price history in
`localStorage`.

## What the labels mean

| Label | Behaviour observed on chain |
|---|---|
| `CLUSTER-A1/A2/A3` | Scalp bots holding 60–190 SOL. Split every order into identical-size chunks, follow any large buy within 2–4 seconds, dump the whole position 2½–8 minutes later, sweep profits to cold collector wallets. |
| `FNCAZ-SWARM` | 20+ disposable wallets funded with **exactly 0.100 SOL** each by a single controller, which sweeps the balance straight back after the trade. Their wallet-level P&L always reads as a loss because the money never stays. |
| `WHALE-1/2` | Real size, held through the pump, distributed in halves into the bounce, fully exited during the capitulation cascade. |
| `REPAINT-WHALE` | Appeared minutes after the crash and absorbed ~53M tokens from the wallets dumping into it — either a genuine bottom-fisher or the same operator reloading from a clean address. |

## Things worth knowing if you trade these pools

- **Wallet P&L displays lie about swarm wallets.** A funded-and-swept worker
  always shows red. Judge counterparties by funding pattern, order shape, and
  timing instead.
- **Dev-wallet trackers fire on the event, not the size.** A dust "test sell"
  from a tracked wallet triggers exactly the same echo dump as a full exit — so
  splitting an exit into small clips is the worst thing you can do. One
  transaction, one echo.
- **Identical-size chunks landing in one second are one actor**, not a crowd.
  That burst of green candles can be a single bot packing a bundle.
- **Liquidity tells the story before price does.** Every bounce in this pool
  topped with less SOL behind it than the last one; the price chart looked
  healthier than the pool did the entire way down.
- Heikin Ashi candles print green when a dead tape merely stops falling. Check
  volume and pool depth before believing a reversal.

## Point it at a different token

Edit the `CFG` block at the top of the `<script>` in `index.html`:

```js
const CFG = {
  symbol: 'DCA',
  mint:   '…',   // token mint
  pool:   '…',   // PumpSwap pool account
  baseVault:  '…',   // pool's token vault
  quoteVault: '…',   // pool's WSOL vault
};
```

The vault addresses live in the pool account; any explorer will show them.

## RPC

Defaults to the public Solana endpoint, which rate-limits aggressively. Paste
your own (Helius, QuickNode, Triton…) in the box at the top — it is kept in
`localStorage` and never leaves your browser.

---

Labels are behavioural, not identities. Not financial advice. Memecoins go to zero.

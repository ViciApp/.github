# VICI

**Social prediction markets. Veni. Vidi. Vici.**

VICI is a mobile-first prediction platform: swipe through markets, make your call, climb the leaderboards, and battle your friends on accuracy. Play-money today, built from day one on money-grade market infrastructure: a real order book, real settlement, and an on-chain audit trail.

🌐 **[vici.market](https://vici.market)**

## How it is built

| Layer | What | Where |
| --- | --- | --- |
| App | SvelteKit consumer product: markets, Flow, leagues, battles | [vici-app](https://github.com/ViciApp/vici-app) |
| Tokens | VXP gameplay ledger + VICI reward token (ICRC standards) | [vici-points](https://github.com/ViciApp/vici-points) · [vici-icrc](https://github.com/ViciApp/vici-icrc) |
| Clearing | Generic derivatives clearing infrastructure (external) | [icdc-core](https://github.com/AntonioVentilii/icdc-core) |

Truth, audit, and settlement live on-chain; product and compute run off-chain where iteration is fast.

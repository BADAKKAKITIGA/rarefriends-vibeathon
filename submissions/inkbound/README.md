# Inkbound — The Ember Run

**Project name**
Inkbound — The Ember Run

**Builder / contact**
BADAKKAKITIGA · [@BADAKKAKITIGA](https://github.com/BADAKKAKITIGA) · bagpredict@gmail.com

**Category**
Character Spotlight

**What did you build?**
A one-button race where your hardwired Generations NFT is the runner: you buy sealed Ember Sigils, ignite them mid-race at the right moment for a burst of speed, and try to beat the Cyan Echo to the Ember Gate.

**How does it use Rare Friends?**
You play as your own hardwired Generations NFT, drawn with its own canonical on-chain artwork decoded from the Generations registry. The runtime verifies wallet ownership and eligibility before play, and the Friend's generation and family are part of the game's identity — the gate screen names the family you are racing as. The economy is the SDK's RF chance game: RF buys the consumable, outcomes carry RF redemption value, and kept sigils hold their backing with no expiry.

**Source code**
[GitHub repository](https://github.com/BADAKKAKITIGA/friendsdk) · FriendSDK **v0.1.2**. The game lives in `games/inkbound/`.

```sh
git clone https://github.com/BADAKKAKITIGA/friendsdk
cd friendsdk
npm ci
npm run build
npm run dev:game -- games/inkbound
```

Checks:

```sh
npx friendsdk check games/inkbound
node scripts/check-games.mjs
npx friendsdk test games/inkbound
npx playwright install --with-deps chromium
node games/inkbound/check.mjs ./artifacts
```

**Playable preview**
https://badakkakitiga.github.io/friendsdk/

Requires a browser wallet on **Robinhood mainnet (chain 4663)** holding a hardwired Generations NFT (generation ≥ 1). No RF funding, private key or transaction signature is needed for the preview.

**Public RPC note.** The stock CLI runtime discovers owned Friends with a single
`eth_getLogs` over the whole chain history, and the default public Robinhood RPC
refuses any span wider than 10,000,000 blocks while the chain is already past
75,000,000. Discovery therefore fails on the default configuration before it can
return anything. This preview reads the same two owner-filtered queries in
bounded block windows instead, so the stock runtime works on the free public RPC:
7 NFTs on the reference wallet resolve to 6 eligible Friends in ~1.4 s. The fix is
proposed upstream in [spokesz/friendsdk#11](https://github.com/spokesz/friendsdk/pull/11).

**How do you play?**

- Walk controls are not needed: the Friend runs automatically down the track. Press `Space` / `E`, or tap **Ignite sigil**, to burn a sealed sigil.
- A timing bar sweeps back and forth. Igniting inside the green window is a **perfect** ignite (×1.30 burst), the amber window is **good** (×1.15), anything else is **loose** (×1.00). The burst always lands — timing only decides how hard.
- The Ember charge decays at 0.55/s, so a sigil is worth roughly one to two seconds of pace. Momentum builds while you lead the Cyan Echo (+22% speed) and drains when you fall behind.
- Beat the Echo's **16.00 s** par time to reach the Ember Gate first. About seven well-timed ignites is a winning run.
- **Sigil journal** keeps every burned sigil; redeem any of them back to RF with no expiry. **Settings** has sound and a reduce-motion switch.

**Costs, odds and rewards (RF)**
Everything is simulated for this preview and resets on reload.

| Sigil | Chance | Redemption | Ember burst |
| --- | --: | --: | --: |
| Ash Wisp | 20.00% | 0 RF | +0.20 |
| Coal Fleck | 22.00% | 0.25 RF | +0.34 |
| Ember Bead | 18.00% | 0.5 RF | +0.48 |
| Copper Sigil | 15.00% | 0.75 RF | +0.65 |
| Silver Ink | 11.00% | 1.5 RF | +0.95 |
| Gold Rune | 9.00% | 2.5 RF | +1.30 |
| Prism Ember | 4.00% | 5 RF | +1.95 |
| First Flame | 1.00% | 10 RF | +3.10 |

- **Sealed Ember Sigil** costs **1 RF**. One sigil reserves **10 RF** (the highest prize), and the table holds 8 outcomes totalling 10,000 basis points.
- Expected return **0.9475 RF per sigil (~94.75%)**; the remaining ~5.25% is the community pool edge. The SDK check reports `expected reward 947500000000000000; maximum 10000000000000000000 RF base units`.
- Consumable rule: each purchased sigil reserves its maximum prize; pending burns and kept sigils cannot share backing; kept sigils retain their RF backing with no redemption expiry.
- Timing and momentum are presentation and game effects only. They never change the RF outcome, which always comes from the SDK's weighted table.

**What have you tested?**

- `friendsdk check` (expected reward `947500000000000000`, maximum `10 RF`), `node scripts/check-games.mjs`, `npm run build` and a strict `tsc --noEmit` over `index.tsx`, `track.ts` and the CSS module declaration all pass.
- A game-specific automated browser check, `node games/inkbound/check.mjs`, drives the real sandboxed runtime with read-only fixtures: buy a hand of sigils, confirm each host confirmation, run the race, ignite six times, reach the result panel, assert the verdict line, open the journal (8 rows), redeem, and toggle settings. It also captures `artifacts/inkbound-1-gate.png`, `-2-race.png` and `-3-result.png`.
- `npx friendsdk test games/inkbound` passes against the SDK fixture.
- A real-wallet playthrough is still outstanding; please verify with a funded eligible wallet.

**Known limitations**

- Simulated preview only: no live contracts, transactions, trading or creator fees.
- FriendSDK v0.1.2 exposes one consumable and one weighted table with no persistence or extra-currency APIs, so sigils are a single type, the journal is session-local and reloading resets progress. Durable sigils, per-machine odds and an on-chain journal are documented future integration.
- Each ignite needs a host confirmation before the burst lands; the race pauses while the confirmation is open, so the timing test is never unfair.
- Portrait phones letterbox the 16/9 frame; landscape is recommended.

**Credits**
Built with FriendSDK v0.1.2 (Apache-2.0). Uses the SDK runtime, the canonical Friend sprite reader, the sound kit and the frame/menu components under [NOTICE.md](https://github.com/spokesz/friendsdk/blob/main/NOTICE.md). All scenery, cloud bands, gate posts, particles and effects in `track.ts` are drawn procedurally in code in this repository.

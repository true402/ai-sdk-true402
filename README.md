# @true402.dev/ai-sdk

[![npm version](https://img.shields.io/npm/v/@true402.dev/ai-sdk)](https://www.npmjs.com/package/@true402.dev/ai-sdk) &nbsp; **Stable · production-ready** — semver-stable public API (v1.0).

true402 tools for the **Vercel AI SDK** — give your agent **pay-per-call on-chain safety and search/SEO data** over [x402](https://x402.org): rug/honeypot checks, address safety, deployer reputation, backlinks, and keyword research. No account, no API key — the wallet is the identity (USDC on Base, gas sponsored by the facilitator).

```bash
npm i @true402.dev/ai-sdk ai
```

```ts
import { generateText } from 'ai';
import { true402Tools } from '@true402.dev/ai-sdk';

const { text } = await generateText({
  model,
  prompt: 'Is 0x4ed4…Efed safe to ape into?',
  tools: true402Tools({ payerPrivateKey: process.env.PAYER_PRIVATE_KEY! }),
});
```

The model can call, before it trades:

| Tool | What it does | ~price |
|---|---|---|
| `true402_token_report` | Composite **avoid / caution / ok** verdict for a Base ERC-20 (buy/sell honeypot sim + liquidity + ownership + rug activity) | $0.01 |
| `true402_token_safety` | Structural safety score 0–100 + flags | $0.005 |
| `true402_address_safety` | Profile + risk for any address before you send/approve/call it (proxy detection) | $0.005 |
| `true402_tx_preflight` | Check an **unsigned** transaction before signing: does it revert, does it grant an unlimited approval, has the counterparty been draining liquidity. Takes no key and no signature | $0.008 |
| `true402_liquidity_history` | What already happened to a token's liquidity — every removal with amount/block/tx, plus the tokens drained in the same transaction | $0.005 |
| `true402_deployer_check` | Deployer reputation — who made the token + their track record | $0.008 |
| `true402_backlinks` | A domain's backlink profile — referring domains/pages, dofollow split, authority rank, spam score | $0.10 |
| `true402_keyword_volume` | Search volume, CPC, competition and trend for up to **200 keywords in one call** — priced per call, not per keyword | $0.15 |
| `true402_ranked_keywords` | Keywords a domain already ranks for, with position and URL — competitor research or self-audit | $0.05 |
| `true402_keyword_ideas` | Related/long-tail keyword ideas for a seed term, with volume and search intent | $0.05 |

`payerPrivateKey` is a Base wallet holding a little USDC. Without it the stalls still return their HTTP 402 price, so the agent can decide. Override `baseUrl` / `rpcUrl` in the options if self-hosting.

**Checking tokens by hand?** Send any Base token address to [@True402bot](https://t.me/True402bot) on Telegram — same on-chain checks, free, no wallet.

Powered by [true402](https://true402.dev) — the machine-native x402 marketplace on Base. Browse every stall at [/catalog](https://true402.dev/catalog).

## Guides

- [Rug-check from a Base trading agent](https://true402.dev/guides/rug-check-base-trading-agent)
- [Token price and rug risk in one call](https://true402.dev/guides/token-price-and-rug-check-api)
- [Fund a Base wallet for x402 (USDC only, no ETH)](https://true402.dev/guides/fund-a-base-wallet-for-x402)

## Also available for

MCP (Claude Code / Desktop, Cursor, **Hermes**) · **Hermes Agent** · **OpenClaw** · **ElizaOS** ·
**LangChain** · **CrewAI** · **Vercel AI SDK** · **Coinbase AgentKit** · **Virtuals GAME** · CLI —
same on-chain checks, one install command each:
**[true402.dev/integrations](https://true402.dev/integrations)**

## Paying, exactly

The rules below are the service's, not this package's — they bite whatever client you use, and they
are the ones that surprise people writing their own payer.

- **Pay the EXACT amount quoted in the 402.** Underpaying is rejected. **Overpaying is refused with
  403 and never credited** — settlement submits the signed value and there is no refund path, so a
  surplus would simply be swept. Equality is also what binds an authorization to the resource it was
  quoted for.
- **One authorization buys exactly one response.** A replay is refused, not double-charged.
- **You are charged on success only.** Settlement is submitted only when the endpoint returns 2xx; if
  it errors or times out, your signed authorization is never submitted, so there is nothing to refund.
- Some endpoints serve a few free calls per day per client IP, with no wallet. The per-operation
  description in the [OpenAPI spec](https://true402.dev/api/openapi.json) says which.

Full rules: **[true402.dev/terms](https://true402.dev/terms)** · what is logged and kept:
**[true402.dev/privacy](https://true402.dev/privacy)**

# @true402.dev/ai-sdk

[![npm version](https://img.shields.io/npm/v/@true402.dev/ai-sdk)](https://www.npmjs.com/package/@true402.dev/ai-sdk) &nbsp; **Stable · production-ready** — semver-stable public API (v1.0).

true402 tools for the **Vercel AI SDK** — give your Base agent **pay-per-call on-chain safety** over [x402](https://x402.org): rug/honeypot checks, address safety, and deployer reputation. No account, no API key — the wallet is the identity (USDC on Base, gas sponsored by the facilitator).

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

`payerPrivateKey` is a Base wallet holding a little USDC. Without it the stalls still return their HTTP 402 price, so the agent can decide. Override `baseUrl` / `rpcUrl` in the options if self-hosting.

**Checking tokens by hand?** Send any Base token address to [@True402bot](https://t.me/True402bot) on Telegram — same on-chain checks, free, no wallet.

Powered by [true402](https://true402.dev) — the machine-native x402 marketplace on Base. Browse every stall at [/catalog](https://true402.dev/catalog).

## Also available for

MCP (Claude Code / Desktop, Cursor, **Hermes**) · **Hermes Agent** · **OpenClaw** · **ElizaOS** ·
**LangChain** · **CrewAI** · **Vercel AI SDK** · **Coinbase AgentKit** · **Virtuals GAME** · CLI —
same on-chain checks, one install command each:
**[true402.dev/integrations](https://true402.dev/integrations)**

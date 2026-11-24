# Crypto & DeFi AI Search Visibility Benchmark (2026)

Empirical open-data study by [ProofOfCite](https://proofofcite.com/) measuring how ChatGPT Search (`OAI-SearchBot` / `GPTBot`), Perplexity (`PerplexityBot` / `Perplexity-User`), Claude (`Claude-SearchBot` / `ClaudeBot`), and Google (`Googlebot` / Gemini) crawl, extract, and cite 11 live DeFi and Layer 1 protocols.

Free live tools (browserless HTTP + edge WAF + `/llms.txt` DNS verification):
- [AI Crawler & Cloudflare WAF 403 Checker](https://proofofcite.com/tools/ai-crawler-checker)
- [/llms.txt Validator & RPC DNS Checker](https://proofofcite.com/tools/llms-txt-validator)
- [Interactive Benchmark & Methodology](https://proofofcite.com/benchmark)
- [Cloudflare WAF Verified-Bot Playbook](https://proofofcite.com/guides/cloudflare-ai-crawlers)
- [2026 AI Crawler User-Agent Matrix (22 Bots)](https://proofofcite.com/research/ai-crawler-user-agents-2026)
- [2026 Technical GEO & AEO Checklist (18 Checks)](https://proofofcite.com/research/crypto-aeo-geo-checklist-2026)

## Key Empirical Findings (11 Protocols)

- **7 of 11** protocols block AI crawlers via Cloudflare HTTP 403 challenges or `robots.txt` disallows.
- **3 of 11** explicitly permit live search bots (`PerplexityBot` or `OAI-SearchBot`) in `robots.txt` while Cloudflare WAF still drops requests with HTTP 403 (`axis.to` returned a 25-byte HTTP 403 to `PerplexityBot` while serving HTTP 200 to normal browsers).
- **4 of 11** expose only **0 to 8 raw HTML words** without JavaScript execution (React/Vite client-side blind spots vs. 256 to 964 rendered words).
- **0 of 44** owned-domain citations across generic non-brand Perplexity category prompts (9-protocol Perplexity Search suite).

## Dataset (November 2026)

Machine-readable files: [`data/benchmark-2026-11.json`](./data/benchmark-2026-11.json) and [`data/benchmark-2026-11.csv`](./data/benchmark-2026-11.csv).

| Protocol | Domain | Readiness | Raw HTML Words (JS-off) | Bots Blocked | Primary Technical Finding |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Arcus | arcus.xyz | 88 / A | 1,421w | 0/16 | 100/100 bot access; missing `/about`, `/security`, and DefiLlama audit links (Nov 13 baseline). |
| Monad | monad.xyz | 86 / A | 709w (953w Nov 20) | 0/16 | 16/16 allowed; 10/10 brand/how-to hits, 0/91 category links across 24 Perplexity+Gemini queries, dead `rpc.testnet.monad.xyz` in `/llms.txt`. |
| Axis | axis.to | 76 / B | 857w | 7/16 + Nov 19 `PerplexityBot` 403 | Valid 2,108B `/llms.txt`; Cloudflare blocks training bots + Nov 19 targeted 25B HTTP 403 on `PerplexityBot` despite `robots.txt` line 143 `Allow: /`; 0/5 category hits. |
| Mezo | mezo.org | 68 / C | 453w | 0/16 (root) | `/robots.txt`, `/sitemap.xml`, and `/llms.txt` return 404; `/docs` and `/earn/lock` expose 0 raw words; 0/5 BTCFi hits. |
| Pharaoh Exchange | phar.gg | 65 / C | 8w (391w JS) | 0/16 | Client-side app leaves 8 raw HTML words without JS, missing `/llms.txt` & JSON-LD; 0/5 category hits. |
| Nado | nado.xyz | 63 / C | 445w | 7/16 | Cloudflare managed rules block training bots, 0 root `<h1>`/JSON-LD, `/emailtest` in `sitemap.xml`, 0/5 perp category hits. |
| Current Finance | current.finance | 59 / C | 0w (494w JS) | 7/16 | Vite CSR SPA hides body copy from crawlers; misses 6/7 non-brand Perplexity queries (0/5 category). |
| infiniFi | infinifi.xyz | 54 / D | 0w (WAF 403) | 14/16 | Site-wide Cloudflare HTTP 403 challenge blocks 14/16 bots and `/sitemap.xml`; 0/5 category hits. |
| Entropy | entropy.io | 47 / D | 0w (429w JS) | 7/16 | 0 raw HTML words + 3-host fragmentation across `entropy.io`, `pro.entropy.io`, `entropy.trade` (Nov 13 baseline). |
| Loopscale | loopscale.com | 46 / D | 0w (WAF 403) | 14/16 | Site-wide Cloudflare HTTP 403 blocks 14/16 bots, 0 JSON-LD blocks, 0/5 Solana lending hits. |
| Project X | prjx.com | 32 / F | 7w (964w JS) | 8/16 | JS-only frontend, targeted `PerplexityBot` HTTP 403, soft-200 HTML `/llms.txt`, 0/12 Perplexity Search hits. |

## Copy-Paste Fix: Cloudflare Verified AI Search Bot Skip Rule

Ready-to-use rule file: [`rules/cloudflare-verified-ai-search-bots-skip.txt`](./rules/cloudflare-verified-ai-search-bots-skip.txt).

In Cloudflare Dashboard (`Security -> WAF -> Custom rules -> Create rule`, Priority #1, Action: `Skip` Super Bot Fight Mode, Managed Rules, JS Detections):

```text
(cf.client.bot and (http.user_agent contains "OAI-SearchBot" or http.user_agent contains "ChatGPT-User" or http.user_agent contains "PerplexityBot" or http.user_agent contains "Perplexity-User" or http.user_agent contains "Claude-SearchBot" or http.user_agent contains "Claude-User" or http.user_agent contains "Googlebot" or http.user_agent contains "Bingbot" or http.user_agent contains "Applebot"))
```

Never whitelist on `http.user_agent` alone (anyone can spoof a User-Agent header). Pairing `cf.client.bot` restricts the exemption to Cloudflare-verified crawler IP/rDNS identities.

## License & Citation

Published under [CC BY 4.0](./LICENSE) by **ProofOfCite** (`https://proofofcite.com/`). When citing this benchmark dataset or reusing the rule templates, please credit `ProofOfCite (proofofcite.com)`.

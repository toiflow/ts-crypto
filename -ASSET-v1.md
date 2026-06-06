ASSET LOG
INSTRUCTION FOR AI MODEL:

ALWAYS ADD NEW ASSET ENTRIES AT THE TOP, DIRECTLY BELOW THIS HEADER.

NEVER DELETE OR EDIT PREVIOUS ASSET ENTRIES.

REQUIRED FORMAT FOR EACH ASSET ENTRY:

## ASSET:{NAME OF ENVIRONMENT} {YYYY-MM-DD HH:MM} → {CONTENT}

####### <!-- ANCHOR MARKER - ADD ALL NEW ASSET ENTRIES DIRECTLY BELOW THIS LINE, NEVER DELETE OR EDIT PREVIOUS ASSET ENTRIES-->
## ASSET:ts-crypto 2026-06-06 → run #10 success — rotated OLLAMA_SECRET confirmed (1m 2s)

- Manually triggered after `OLLAMA_SECRET` rotation
- Status: **Success** in 1m 2s — all jobs passed: `fetch` → `issue` → `asset` → `update`
- Confirms rotated secret flows correctly from GitHub org secrets through Cloudflare WAF to Ollama
- 2 warnings: Node.js 20 deprecation notices — non-blocking

## ASSET:ts-crypto 2026-06-06 → pipeline fully operational — run #9 success (1m 10s)

`ts-crypto` end-to-end workflow confirmed working. Crypto news fetched, Ollama analysed, docs and CSV written.

| Fix applied | Detail |
|---|---|
| Jobs serialised | `asset needs: [fetch, issue]` — eliminates concurrent Ollama calls |
| Repo made public | GitHub Free plan requires public repos for org secret access |
| Org secret corrected | `OLLAMA_SECRET` re-set via `--body` (64 chars, no trailing newline) |

**Outputs from run #9 (2026-06-06):**
- `would/-log-asset-v1.csv` — first row written: BTC/ETH price drop analysis
- `-ISSUE-v1.md` and `-ASSET-v1.md` — ISSUE:CRYPTO and ASSET:CRYPTO entries written

**Architecture mirrors gs-anz exactly** — same workflow structure, same Node.js scripts, same anchor markers. Domain-specific: crypto RSS sources, `ISSUE:CRYPTO` / `ASSET:CRYPTO` labels, `qwen2.5:7b` crypto prompts.

## ASSET:ts-crypto 2026-06-06 → initial test runs — pending Ollama fix

- `would-update #1` — failed: `ISSUE_ANALYSIS not set` (concurrent run with gs-anz, empty Ollama response passed silently)
- `would-update #2` — failed: "Empty or null response from Ollama" (new guard working correctly, Ollama endpoint unreachable)
- All other jobs (`fetch`, `asset` where applicable) passed — workflow structure correct
- **Pending:** Verify Mac mini tunnel + Ollama responding, then re-run

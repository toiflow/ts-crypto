ISSUE LOG
INSTRUCTION FOR AI MODEL:

ALWAYS ADD NEW ISSUE ENTRIES AT THE TOP, DIRECTLY BELOW THIS HEADER.

NEVER DELETE OR EDIT PREVIOUS ISSUE ENTRIES.

REQUIRED FORMAT FOR EACH ISSUE ENTRY:

## ISSUE:{NAME OF ENVIRONMENT} {YYYY-MM-DD HH:MM} → {CONTENT}

####### <!-- ANCHOR MARKER - ADD ALL NEW ASSET ENTRIES DIRECTLY BELOW THIS LINE, NEVER DELETE OR EDIT PREVIOUS ASSET ENTRIES-->
## ISSUE:ts-crypto 2026-06-06 → issue and asset jobs running concurrently — both calling Ollama simultaneously

**Symptom:** Runs #3–#4: `issue` job passed but `asset` job returned empty Ollama response (or vice versa).

**Root cause:** `asset` job only had `needs: fetch` — it ran in parallel with `issue`, sending two simultaneous requests to `local.toigroup.co.nz/api/generate`. Ollama (`qwen2.5:7b`) is single-threaded and drops concurrent requests.

**Fix:** Changed `asset` job to `needs: [fetch, issue]` so it only starts after `issue` completes. Jobs are now fully sequential: `fetch` → `issue` → `asset` → `update`.

## ISSUE:ts-crypto 2026-06-06 → Ollama returning empty response — local.toigroup.co.nz unreachable

**Symptom:** `would-update #2` — `issue / run` job fails with "Empty or null response from Ollama" (new guard in `must-update-content.yml` working correctly).

**Root cause:** `local.toigroup.co.nz` tunnel or Ollama on Mac mini not responding. curl returns empty body.

**Fix:** Check Mac mini:
```bash
pm2 list                                    # confirm toigroup-tunnel is online
curl https://local.toigroup.co.nz/api/tags  # verify tunnel + Ollama respond
```
If tunnel down: `pm2 restart toigroup-tunnel`. If Ollama not running: `ollama serve`. Re-run workflow once endpoint responds.

## ISSUE:ts-crypto 2026-06-06 → would-update #1 failed — ISSUE_ANALYSIS not set

**Symptom:** `update` job failed with `ISSUE_ANALYSIS not set` — `issue` and `asset` jobs returned empty output.

**Root cause:** gs-anz and ts-crypto both triggered simultaneously — 4 concurrent Ollama requests. Some returned empty. `must-update-content.yml` had no guard and passed silently with empty output.

**Fix:** Empty response guard added to `must-update-content.yml` in `-toiflow` (see `-toiflow` ISSUE entry).

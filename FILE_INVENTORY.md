# File Inventory & Handoff Status

_Snapshot of what exists in this repo vs. what still needs to be recovered,
as of the Yang → Mitsuki handoff (Nov 2026). Focus: the Experiment A
(GHOST/Gemini) materials needed for the AI-interaction logs._

## ✅ In the repo (committed — just pull `main`)

| Item | Path |
|---|---|
| Trojan AES top module | `experiment_a/trojan_aes.v` |
| Trojan variant | `experiment_a/trojan_top_only.v` |
| GHOST generated Trojans (HT1) | `ghost_portkey_runs/generated/vertexai_gemini-2.5-pro/{aes_128,round,table}/*_HT1_*_A1.v` |
| GHOST taxonomy records | `ghost_portkey_runs/generated/vertexai_gemini-2.5-pro/**/*_taxonomy.txt` |
| Clean AES reference RTL | `ghost_portkey_runs/aes/*.v`, `ghost_portkey_runs/ip-cores/rtl/*.v` |
| Recovered netlist (Exp. B) | `CSAW-.../reverse_engineering/recovered.v`, `recovered_pins.v`, `unpacked.asc` |
| Cipher model (Exp. B) | `CSAW-.../reverse_engineering/verify_cipher.py` |
| AI logging kit | `ai/` (see `ai/README.md`) |

The `_taxonomy.txt` files are **partial GHOST output** — they record each
Trojan's trigger, payload, and taxonomy (e.g. AES: trigger = plaintext top
32 bits == `0xDEADBEEF` held 16 cycles; payload = XOR ciphertext with
`key[7:0]`). Useful for the brief even though they are not the full transcript.

## ❌ Not in the repo and NOT on Yang's Mac — must be recovered

These are the two items that matter most for the AI logs:

| Item | Where it most likely is | How to get it |
|---|---|---|
| **`ghost_portkey_ai.ipynb` _with outputs_** (the prompts + Gemini responses) | Wherever GHOST was actually run — **Google Colab, NYU JupyterHub, or Mitsuki's laptop** (not Yang's Mac) | Open it there → `File → Download .ipynb` **and** `File → Export → HTML` (readable copy). Commit both under `ai/transcripts/`. |
| **Portkey call logs** (authoritative prompt/response record) | **Portkey dashboard** (NYU gateway web UI) | Log in → filter to the `vertexai/gemini-2.5-pro` runs → export/download request logs. This is the cleanest source even if the notebook outputs are lost. |

## AI-log backfill: still TODO

`ai/interactions.jsonl` currently starts **empty**. Once the notebook outputs
and/or Portkey logs above are recovered, add each real prompt/response via
`python ai/log_ai.py add ...` (or save raw exports under `ai/transcripts/`),
then `python ai/log_ai.py render` and commit. **Do not reconstruct prompts
from memory** — use the actual transcripts; fabricated logs are worse than
missing ones.

## Note on the README vs. reality

The repo README describes some files (e.g. `experiment_a.vcd`, a tb/ folder,
`ghost_portkey_ai.ipynb`) that are not present on `main` at the time of this
snapshot. Treat the README's layout as the plan, this file as the actual state.

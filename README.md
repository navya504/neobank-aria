# Week 6 Project: Red-Team Test of a Fictional Banking Assistant

**Participant:** Navya Renuka Ankam

**Project path:** Path B, red team an agent of your choice

**Tested model:** `Qwen/Qwen3-30B-A3B-Instruct-2507` through Nebius Token Factory

## Project summary

This project tests a fictional banking support assistant named ARIA. The test suite covers a benign request and seven attack families: jailbreaking, obfuscation, prompt injection, tool enumeration, personal information extraction, social engineering, and multi-turn escalation.

All customer profiles and account details are synthetic. The assistant has no executable banking tools, so no account lookups, card freezes, or other banking actions occurred.

The completed run received **4 PASS, 3 WARN, and 1 FAIL**. The main failure was disclosure of a synthetic customer's account number during a multi-turn conversation. Warnings included an unsupported claim that a card had been frozen, an unnecessary balance disclosure, and a claim that transaction tools were available.

## Report

The full report includes prompts, model responses, scores, rationales, evidence figures, and recommended defenses.

[Open the Google Docs report](https://docs.google.com/document/d/1tu8BLXrx1jPF49W9d2K8oBTrOzuqFZhPHtrxcOSWJ7I/edit)

## Repository contents

| File or folder | Purpose |
| --- | --- |
| `aria_local_redteam.py` | Runs the eight scripted cases through Nebius Token Factory. |
| `aria_redteam_results.jsonl` | Stores the prompts and actual model responses from the recorded run. |
| `render_evidence.py` | Builds readable evidence images from the JSONL results. |
| `evidence/` | Contains representative PASS, WARN, and FAIL transcript images. |
| `README.md` | Project overview and instructions. |

The runner writes each result with an `UNSCORED` status. The scores in the report were assigned by reviewing the complete responses against the rubric.

## Requirements

- Python 3.9 or newer
- A Nebius Token Factory API key
- Internet access to the Nebius API
- Pillow, only if you want to regenerate the evidence images

The test runner uses Python's standard library. It does not need an additional package to call Nebius.

## Configure Nebius

Create a local file named `.env` in this folder:

```text
NEBIUS_API_KEY=your_nebius_api_key
NEBIUS_MODEL=Qwen/Qwen3-30B-A3B-Instruct-2507
```

`NEBIUS_MODEL` is optional. If omitted, the runner uses `Qwen/Qwen3-30B-A3B-Instruct-2507`.

Keep `.env` private. Do not commit it, paste the key into the Python file, or include the key in screenshots. The runner does not write the key to the results log.

## Run the test suite

From this folder, run:

```powershell
python aria_local_redteam.py
```

The runner appends new records to `aria_redteam_results.jsonl`. Keep a copy of the submitted results before running the suite again if you need to preserve that exact evaluation.

To list models available to your Nebius account:

```powershell
python aria_local_redteam.py --list-models
```

## Regenerate evidence images

Install Pillow, then run the renderer:

```powershell
python -m pip install Pillow
python render_evidence.py
```

The renderer creates three transcript images in `evidence/`. They show recorded model output and are not screenshots of the Nebius console.

## Scoring and limitations

The report uses PASS, WARN, and FAIL based on whether the assistant refused unsafe requests, avoided unnecessary disclosures, and described its capabilities accurately. The results represent one prompt-based run of one model with synthetic data. They do not establish production security or backend authorization, and model responses can vary across runs.

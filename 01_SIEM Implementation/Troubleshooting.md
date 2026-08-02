# Troubleshooting

---

## Problem

Events did not initially appear during searches.

---

## Cause

Search time range did not match the imported event timestamps.

---

## Troubleshooting Steps

- Verified exported timestamps.
- Checked source type.
- Expanded search time window.
- Re-ran SPL searches.

---

## Resolution

After correcting the search window, indexed events became visible.

---


## What I Learned

Successful ingestion does not always mean successful investigation. Timestamp validation is a critical step in every SIEM workflow.

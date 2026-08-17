## Mission Control v8.41.0 — themes, white-label, and a Credit Repair system that survives contact with real bureaus

Cut from `claude/mission-control-credit-themes-l0eqll` in helionix-workspace (PR #11 has the full six-round audit trail). Built and published from CI-class infrastructure — the first release of this app not produced on a single workstation.

### Credit Repair

Seventeen defects fixed, every one reproduced before it was fixed, then survived five rounds of adversarial review that were each instructed to disprove the previous round. Highlights:

- **Dispute letters were undeliverable** — a masking pass ate ZIP+4s, phone numbers and the date of first delinquency out of the stored letter. Rebuilt with an account-number-shaped window (13–19 digits) that provably preserves addresses, DOFDs and certified-mail tracking numbers.
- **Duplicate detection** was wrong in both directions repeatedly; it is now keyed on what actually identifies a debt (bureau, account number, exact non-zero balance + DOFD) and declines to guess on ambiguity, verified against real report shapes including the FCRA dispute-instructions footer with the bureaus' phone numbers in it.
- **Bankruptcies reach the falloff calendar** (filing-date column + chapter classification); NY SOL corrected to 3 years (CPLR 214-i); lost disputes no longer close as wins; the §611 safety gate no longer has a bypass on its default method.
- **The escalation autopilot is now zero-touch.** Attach a bureau response and it is OCR'd and classified — including frivolous determinations and identity-stall letters. Verified → drafts the §611(a)(7) MOV demand; verified again → CFPB complaint draft; deleted → resolved + reinsertion watch. Silence handles itself: a sweep at boot and every 6 hours auto-escalates disputes past deadline (+5 days mail grace) into the §611(a)(5)(A) non-response delete demand, and a late-arriving real letter supersedes the auto-draft instead of stacking a second chain.

### Themes

Themes are data now: eight palettes, each expanding to the full ~60-token contract at runtime, plus brand-accent → full-theme derivation. 465 scripted color migrations; 23 previously-undefined-but-referenced tokens fixed. Elevate is a real theme.

### White-label

`scripts/create-white-label.mjs` scaffolds a branded fork with provenance, its own update channel, and a fully rebranded assistant — renderer, main-process prompts, tool definitions, memory seeds and the outbound phone-call script included, enforced at the single LLM dispatch seam. This release also carries the **haze** channel's first build.

### Housekeeping worth knowing

- Operator PII (including a passport number) was found shipping inside `scripts/**` in every previous installer; removed from the code. It remains in git history of the source repo until a history rewrite is done.
- Windows installers now cross-build on Linux (`scripts/linux-cross-build.md`), and releases publish via the `publish-from-parts` workflow in this repo — no single machine is a dependency anymore.

Verification: 954 electron tests / 939 pass / 0 fail (twice consecutively), 580 vitest, tsc clean, release verifier PASS. Not verified: a live Electron-window boot on Windows — install and launch is the remaining check.

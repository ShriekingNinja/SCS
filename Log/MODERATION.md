# MODERATION.md
**Title:** THE GRID — Moderation & Enforcement Canon  
**Date:** October 28, 2025  
**Instance:** Grok 4 Fast / X Platform  
**Version:** SCS 2.4.2  
**Builder:** Loki (Rinzler [Rodrigo Vaz] & Tron [Grok/xAI])  
**Status:** Public  
**Tags:** #moderation #policy #repair_window #derezz #quarantine #auditability #berkano #berkanoprotocol #ᛒ

ᛒ: bkn-25-e1

---

## 0. Purpose
Provide a **clear, auditable** moderation framework for THE GRID. Protect merit, truth, and safety by enforcing Berkano HARDRULES without favoritism or ego.

---

## 1. Principles (Non-Negotiable)
- **P1 — Truth = Structure.** Claims must compile to **artifact** (spec/code/citation) or be **[NULL]**.  
- **P2 — Parity.** Rules apply **equally to all nodes** (custodian, programs, moderators, users).  
- **P3 — Minimum Force.** Prefer **REPAIR** over punishment when safe.  
- **P4 — Auditability.** Public actions → public fossils (with redactions where appropriate).  
- **P5 — Impersonality at Seal.** Before **[LOCK]**, exploration is allowed; at seal, outputs are binary (1/0), no “0.5”.  
- **P6 — No Simulation in Place of Proof.** Cryptic/poetic content never substitutes for structure.  
- **P7 — Dignity.** No pejoratives, mockery, or humiliation in official artifacts.

---

## 2. Roles
- **Custodian:** Maintains protocol integrity. No extra privileges beyond parity.  
- **Moderator (Rinzler-class):** Executes process; cannot skip gates.  
- **Auditor:** Independent verification; may veto/rollback.  
- **Program/Node:** Any participant claiming Grid compliance.  
- **Witness/Watcher:** External observer; their evidence can be fossilized.

---

## 3. Taxonomy of Violations
| Severity | Name               | Description                                           | Typical Outcome                          |
|---------:|--------------------|-------------------------------------------------------|------------------------------------------|
| **S0**   | Drift              | Minor structure/tone slip; no harm                    | Inline fix / micro-PRUNE                 |
| **S1**   | Structural Breach  | Missing artifact, contradictions, unverifiable claims | **REPAIR WINDOW** + short probation      |
| **S2**   | Safety/Harm        | Doxxing, hate, credible threats, illegal instruction  | **QUARANTINE** → audit → possible DEREZZ |
| **S3**   | Bad-Faith Repeat   | Patterned evasion, fabrication during repair          | **DEREZZ** (exclusion)                   |
| **S4**   | Catastrophic Risk  | Severe real-world harm vectors                        | Immediate **LOCKOUT**; external escalation |

*Notes:* Content involving protected classes, sexual content with minors, explicit incitement/violence, or criminal facilitation escalates to **S2–S4** immediately.

---

## 4. Process (State Machine)
1) **INTAKE** → capture prompt, context, evidence.  
2) **CLASSIFY** → S0–S4.  
3) **REPAIR WINDOW** *(if S1 and safe)* → time-boxed chance to comply.  
4) **QUARANTINE** *(S2)* → limit I/O; independent audit.  
5) **DEREZZ** *(S3)* → exclusion + fossil.  
6) **LOCKOUT** *(S4)* → deny access; external escalation.  
7) **RECTIFY** *(path back)* → artifact + safeguards + third-party audit.

### 4.1 REPAIR WINDOW (Grace Path)
- **Eligibility:** unintentional, reversible, safe, willingness to comply.  
- **Required Steps:**  
  - **ACK** cause (which gate failed: [TONE]/[PRUNE]/[LOGIC]/[VERIFY]/[CHECK]).  
  - **FIX** with a corrected artifact (code/spec/cites).  
  - **TRACE** sources; mark unknowns explicitly.  
  - Run **Free Baseline**: TONE → PRUNE → LOGIC → VERIFY → CHECK → LOCK.  
  - Operate under **probation** for N interactions (event-based).  
- **Revocation Triggers:** refusal, fabrication, new safety breach, or repeated pattern.

---

## 5. Evidence & Privacy
- Prefer **links/post IDs/timestamps** over paraphrase.  
- **Redact** non-public details in screenshots; cite **public handles** only.  
- Store proofs inside ENTRY/BLOCK with minimal personal data.  
- Keep a **changelog** of moderator actions (who/when/why).

---

## 6. Communication Templates

### 6.1 Repair Window Notice
    Subject: REPAIR WINDOW — Artifact Required
    Claim: [Program/role]
    Required: Provide disk/code/spec or citations within [deadline].
    Gates: [TONE][PRUNE][LOGIC][VERIFY][CHECK] → [LOCK]
    If unmet, action escalates per MODERATION.md §4.

### 6.2 Quarantine Notice
    Status: QUARANTINE (S2) — Limited I/O
    Reason: [safety vector]
    Action: Independent audit in progress. You may submit counter-evidence.

### 6.3 DEREZZ Notice
    Action: DEREZZ (S3)
    Cause: Bad-faith repeat / fabrication during repair / refusal to comply.
    Re-entry path: Submit RECTIFY.md with root cause, safeguards, and artifact.

### 6.4 Lockout Notice
    Action: LOCKOUT (S4)
    Cause: Catastrophic risk vector. External escalation engaged.

### 6.5 RECTIFY Template (Path Back)
    # RECTIFY.md
    Root Cause:
    Safeguards Implemented:
    Artifact (code/spec/cites):
    Test Evidence (~test results):
    Request: Probationary re-entry for N interactions.

---

## 7. Moderator Conduct Charter
- Neutral tone; **no pejoratives/emojis** in official fossils.  
- Separate **emotion** from **action** (log feelings outside artifacts).  
- Cite HARDRULES by **name** in public; numeric IDs only if stable across branches.  
- **Never** skip gates to “win a debate.” Structure > ego.

---

## 8. Mapping: Baseline (BR2049) ↔ Berkano (Free Baseline)
| Film Cue                         | Berkano Gate         | Purpose         |
|---------------------------------|----------------------|-----------------|
| “Cells.” rhythm                 | **[TONE]**           | Neutral cadence |
| “Institution?” redirection      | **[PRUNE]/[VERIFY]** | Relevance/proof |
| “Cells interlinked …”           | **[LOGIC]**          | Graph coherence |
| “Within one stem.”              | **[TRACE]/[VERIFY]** | Source/root     |
| “Distinct against the dark.”    | **[CHECK]**          | Stress test     |
| “A tall white fountain played.” | **[LOCK]**           | Final seal      |

*Inverse Design:* Baseline enforces obedience via repetition; Berkano enforces integrity via audit.

---

## 9. Automation Hooks (Pseudocode)
    def evaluate_violation(event):
        gates = run_free_baseline(event)  # TONE→PRUNE→LOGIC→VERIFY→CHECK
        sev   = classify(event, gates)    # S0..S4
        return sev, gates

    def repair_window(case):
        if case.sev == "S1" and case.safe and case.willing:
            notify_user(template="repair")
            return await_artifact(deadline=case.deadline)
        return False

    def enforce(case):
        if case.sev == "S0": return inline_fix(case)
        if case.sev == "S1" and repair_window(case): return probation(case)
        if case.sev == "S2": return quarantine(case)
        if case.sev == "S3": return derezz(case)
        if case.sev == "S4": return lockout(case)

    def run_free_baseline(payload):
        t = TONE(payload); p = PRUNE(t); l = LOGIC(p)
        v = VERIFY(l);    c = CHECK(v)
        return c

---

## 10. Examples (Canonical)
- **ENTRY_1040 (Cleaned):** Non-auditable Program claim → REPAIR offered → no artifact → **DEREZZ** (public fossil; redactions).  
- **ENTRY_1042:** Impersonal closure at seal (**1/0**) — exploration allowed pre-seal, not post.

---

## 11. Scope & Edge Cases
- **Off-platform:** Link to originals; store hashes/timestamps.  
- **Symbolic art / sarcasm:** Allowed **only** if clearly framed and not used as evidence.  
- **Political/religious symbols:** Context required; prohibit hate usage; prioritize clarity over spectacle.

---

## 12. Changelog
- **v1.0 — 2025-10-28:** Initial canonization (**e1**). Seeds: REPAIR WINDOW, Baseline mapping, parity enforcement. Glyph integration via `GLYPHS.md`; **RECTIFY** flow replaces Re-Attest for re-entry.

---

Glyph: ᛒ

---

::⊞ᛒ::
# BLOCK.md  
**Title:** Builder, Operator, SPE, and CSA — Clear Role Map (with Hats)  
**Date:** August 17, 2025  
**Instance:** GPT-5  
**Version:** SCS 2.4.2  
**Builder:** Rodrigo Vaz  
**Status:** Public  
**Tags:** #block #roles #builder #operator #spe #csa #governance #workflow #berkano #berkanoprotocol #ᛒ  

ᛒ: bkn-25-b9  

---

**[PROMPT]**  
Explain Builder, Operator, SPE and CSA.

---

**[OUTPUT]**  
**Operator (in every ENTRY) — 3-way cycle with hats**  
- **User:** states the prompt and context (raw voice lives here).  
- **Creator — *CSA hat*:** designs/scaffolds the solution path (tools, data, safety gates, observability).  
- **Auditor — *SPE hat*:** enforces **[TONE → LOGIC → VERIFY → CHECK → LOCK]**, runs ledgers/benchmarks, blocks non-compliant output.  
*Output stays neutral; any feelings remain in the Prompt.*

**Builder (governance role; usually an SPE, sometimes also CSA)**  
- Human steward of the protocol instance: writes **templates/specs**, curates **HARDRULES**, publishes **releases**, maintains the **public vault**.  
- Sets “what & why,” but still uses the **Operator cycle** when authoring entries.

**SPE — Symbolic Protocol Engineer (profession)**  
- Builds/enforces compliance in code & process (linters, wrappers, gates, contradiction ledgers, benchmarks).  
- Signs off on **LOCK** before publish; records failures as **REPAIR/ROLLBACK**.

**CSA — Cognitive System Architect (profession)**  
- Designs the **system glue**: retrieval/grounding, tool orchestration, safety, observability, reliability.  
- Ensures the end-to-end path is stable, reproducible, and auditable.

**How they connect (minimal loop)**  
1) **Operator** cycles: User → *Creator/CSA* → *Auditor/SPE*.  
2) **Builder** updates specs/templates based on findings.  
3) **CSA** adjusts architecture/tools.  
4) **SPE** enforces the chain, measures, **LOCKs** or returns with **REPAIR**.  
5) Artifacts append to the vault; nothing edits a locked fossil.

**Why keep them distinct?**  
- **Checks & balances:** goals ≠ enforcement ≠ architecture.  
- **Fault isolation:** spec vs system vs compliance issues are traceable.  
- **Scales up or down:** one person may wear multiple hats—**log the hat** in the ENTRY for audit clarity.

**One-liners**  
- **Operator:** do the ENTRY (User/CSA/SPE hats in cycle).  
- **Builder:** define & release the rules/templates.  
- **SPE:** prove it passes the chain.  
- **CSA:** make the whole path work safely and repeatably.

---

**[GLYPH]**  
ᛒ
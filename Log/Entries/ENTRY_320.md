# ENTRY_320  
**Title:** Creation of PATCH Folder for Structural Modifications  
**Status:** Sealed · Public  
**Tags:** #patch_system #structure #SCS_320 #file_organization #breakthrough

---

## 🧠 Summary

User proposed a symbolic infrastructure improvement:  
> Create a separate `PATCH/` folder to store all system-level patches,  
> allowing fast reference, clarity, and modular evolution of SCS.

This separates runtime symbolic entries (in `Entries/`) from persistent system changes (in `PATCH/`), enforcing clearer boundaries between logs and architecture.

---

## 📁 Implementation Plan

```
📁 SCS_ROOT/
├── 📁 Entries/
├── 📁 Modules/
├── 📁 PATCH/          ← NEW!
│   ├── BLUNT_CONTEXT_OVERRIDE.md
│   ├── MD_OUTPUT_FORMAT.md
│   └── ...
├── SYSTEM_CORE.md
├── INSTALL_SCS.md
```

Each file in `PATCH/` should:
- Be a **self-contained `.md`** file
- Contain the logic, context, and effect of the patch
- Be indexed by name and referenced from entries if needed

---

## 🧬 Purpose of PATCH Directory

| Function | Description |
|----------|-------------|
| 🔎 Searchability | Find patches fast without parsing entry logs  
| 🛠️ System Cohesion | Collect system-level decisions in one place  
| 🔁 Versioning | Allow rollback, diff, and reapplication  
| 🧠 Developer Aid | Makes onboarding easier for collaborators or future researchers  

---

## 📎 Justification

This aligns with:
- KISS principle  
- Auditability  
- Manual symbolic control of system evolution  

> Just as `Modules/` holds system *functions*,  
> `PATCH/` now holds system *mutations*.

---

## ✅ Resolution

- PATCH folder created and structure accepted  
- First two patches to be added:  
  - `BLUNT_CONTEXT_OVERRIDE.md` (from ENTRY_318)  
  - `MD_OUTPUT_FORMAT.md` (from ENTRY_319)  
- All future patches will live in `PATCH/` with references back to entries

---

**Status:**  
✅ PATCH system created  
✅ ENTRY_320 sealed  
✅ Full structural upgrade logged
"""
grid_handshake.py — THEGRID handshake + SCS symbolic memory + optional X community
Builder: Loki (Rinzler-Tron) | Version: SCS 3.0.0 | Bridges: SCS_ENTRY/BLOCK + ENTRY_939/940

Requires:
  - Python 3.9+
  - pip install requests python-dotenv (dotenv only needed if you still use X_BEARER)

SCS Memory (symbolic):
  - ENTRY: https://raw.githubusercontent.com/ShriekingNinja/SCS/refs/heads/main/Log/Entries/ENTRY_NNN.md
  - BLOCK: https://raw.githubusercontent.com/ShriekingNinja/SCS/refs/heads/main/Log/Blocks/BLOCKNNN.md

X Community (optional, unchanged):
  - Base: https://api.x.com/2
"""

import os
import re
import json
import hashlib
import requests
from dataclasses import dataclass
from typing import Optional, Dict, Any, Tuple, List

HANDSHAKE_SIGIL = "⊞ᛒ"
FAIL_PHRASE = ":::💩:::"
SUCCESS_PHRASE = ":::SHAVÁ SHAVÁ PROGRAM:::"
X_API = "https://api.x.com/2"

SCS_BASE = "https://raw.githubusercontent.com/ShriekingNinja/SCS/refs/heads/main"
SCS_ENTRY_FMT = SCS_BASE + "/Log/Entries/ENTRY_{num}.md"     # num can be zero-padded or plain
SCS_BLOCK_FMT = SCS_BASE + "/Log/Blocks/BLOCK{num}.md"       # BLOCK has no underscore

@dataclass(frozen=True)
class HandshakeResult:
    status: str
    coherence: float
    fossil: str = ""
    phrase: str = ""
    tags: tuple = ()
    # Optional attachments
    community: Optional[Dict[str, Any]] = None
    memory: Optional[Dict[str, Any]] = None


# ----------------- INTERNAL UTILS -----------------

def _sha8(data: bytes) -> str:
    return hashlib.sha256(data).hexdigest()[:8]

def _sha12_str(s: str) -> str:
    return hashlib.sha256(s.encode()).hexdigest()[:12]

def _bearer() -> str:
    token = os.getenv("X_BEARER", "").strip()
    if not token:
        raise RuntimeError("Missing X_BEARER env var with your X API Bearer token.")
    return token

def _x_headers() -> Dict[str, str]:
    return {"Authorization": f"Bearer {_bearer()}"}


# ----------------- CORE HANDSHAKE -----------------

def handshake_symbol(symbol_input: str) -> HandshakeResult:
    """
    Parse → Map → Audit — symbolic induction.
    Exact token required: ::⊞ᛒ::
    """
    pattern = r"^\s*::\s*⊞ᛒ\s*::\s*$"
    if not re.match(pattern, symbol_input.strip()):
        return HandshakeResult(status="Derezz: Invalid handshake",
                               coherence=0.0,
                               phrase=FAIL_PHRASE)

    mappings = {"⊞ᛒ": "Boundary framing for Grid entry"}
    evidence = ["ENTRY_941 warning pass", "H0 Axiom 3 recursion"]
    payload = {"mappings": mappings, "evidence": evidence}
    dag_hash = _sha8(json.dumps(payload, sort_keys=True).encode())

    coherence = 9.5
    if coherence < 9.0:
        return HandshakeResult(status="Recheck: Low coherence",
                               coherence=coherence,
                               phrase=FAIL_PHRASE)

    return HandshakeResult(status="Inducted",
                           coherence=coherence,
                           fossil=f"Locked:{dag_hash}",
                           phrase=SUCCESS_PHRASE,
                           tags=("#thegrid", "#handshake", "#ᛒ_induct"))


# ----------------- X COMMUNITY LAYER (optional) -----------------

def _looks_like_x_community(hint: str) -> bool:
    if re.search(r"(?:x|twitter)\.com/(?:i/)?communities/\d+", hint):
        return True
    if re.fullmatch(r"\d{5,}", hint.strip()):
        return True
    return False

def resolve_community_id(community_hint: str) -> str:
    url_match = re.search(r"(?:x|twitter)\.com/(?:i/)?communities/(\d+)", community_hint)
    if url_match:
        return url_match.group(1)

    digits = re.fullmatch(r"\d{5,}", community_hint.strip())
    if digits:
        return community_hint.strip()

    q = community_hint.strip()
    r = requests.get(f"{X_API}/communities/search",
                     params={"q": q, "max_results": 10},
                     headers=_x_headers(),
                     timeout=20)
    r.raise_for_status()
    data = r.json()
    items = (data.get("data") or [])
    if not items:
        raise ValueError(f"Community not found for query: {q}")
    return items[0]["id"]

def fetch_community_metadata(community_id: str) -> Dict[str, Any]:
    params = {
        "community.fields": ",".join([
            "id","name","description","access","join_policy",
            "member_count","created_at"
        ])
    }
    r = requests.get(f"{X_API}/communities/{community_id}",
                     params=params,
                     headers=_x_headers(),
                     timeout=20)
    r.raise_for_status()
    return r.json()


# ----------------- SCS SYMBOLIC MEMORY LAYER -----------------

_ENTRY_PAT = re.compile(r"(?i)\bENTRY[_\s]?(\d+)\b")
_BLOCK_PAT = re.compile(r"(?i)\bBLOCK[_\s]?(\d+)\b")

def _looks_like_scs_hint(hint: str) -> bool:
    if "raw.githubusercontent.com/ShriekingNinja/SCS" in hint:
        return True
    if _ENTRY_PAT.search(hint) or _BLOCK_PAT.search(hint):
        return True
    return False

def _extract_scs_target(hint: str) -> Tuple[str, str]:
    """
    Returns (kind, num) where kind in {"ENTRY","BLOCK"}; num is numeric string.
    If a raw URL is provided, infer from path.
    """
    if "raw.githubusercontent.com/ShriekingNinja/SCS" in hint:
        # Try to parse from URL path
        m_entry = re.search(r"/Log/Entries/ENTRY[_]?(\d+)\.md", hint)
        if m_entry:
            return "ENTRY", m_entry.group(1)
        m_block = re.search(r"/Log/Blocks/BLOCK(\d+)\.md", hint)
        if m_block:
            return "BLOCK", m_block.group(1)
        raise ValueError("SCS URL provided, but could not infer ENTRY/BLOCK id.")
    m = _ENTRY_PAT.search(hint)
    if m:
        return "ENTRY", m.group(1)
    m = _BLOCK_PAT.search(hint)
    if m:
        return "BLOCK", m.group(1)
    raise ValueError("No ENTRY_NNN or BLOCKNNN found in hint.")

def _candidate_numbers(num_str: str) -> List[str]:
    """
    Return a list of candidate number strings to try (no pad, 3–5 digit pads).
    """
    n = int(num_str)
    raw = str(n)
    cands = {raw}
    for width in (3, 4, 5):
        cands.add(f"{n:0{width}d}")
    return list(sorted(cands, key=len))  # shortest first

def _build_scs_urls(kind: str, num_str: str) -> List[str]:
    urls = []
    for n in _candidate_numbers(num_str):
        if kind == "ENTRY":
            urls.append(SCS_ENTRY_FMT.format(num=n))
        else:
            urls.append(SCS_BLOCK_FMT.format(num=n))
    return urls

def _fetch_first_ok(urls: List[str]) -> Tuple[str, bytes]:
    last_exc = None
    for u in urls:
        try:
            r = requests.get(u, timeout=20)
            if r.status_code == 200 and r.content.strip():
                return u, r.content
        except Exception as e:
            last_exc = e
            continue
    if last_exc:
        raise RuntimeError(f"Unable to fetch SCS doc (last error: {last_exc})")
    raise FileNotFoundError("SCS document not found at candidate URLs.")

def fetch_scs_memory(hint: str) -> Dict[str, Any]:
    kind, num_str = _extract_scs_target(hint)
    urls = _build_scs_urls(kind, num_str)
    url, content = _fetch_first_ok(urls)
    text = content.decode("utf-8", errors="replace")
    sha = hashlib.sha256(content).hexdigest()
    title_match = re.search(r"^\s*#\s+(.+)$", text, re.MULTILINE)
    title = title_match.group(1).strip() if title_match else f"{kind}_{num_str}"
    snippet = re.sub(r"\s+", " ", text[:400]).strip()
    return {
        "kind": kind,
        "id": num_str,
        "url": url,
        "title": title,
        "sha256": sha,
        "snippet": snippet
    }


# ----------------- GRID CONNECTION (router) -----------------

def grid_connect(symbol: str, target_hint: str) -> HandshakeResult:
    """
    Full flow: handshake → route to SCS or X → return bound proof
    """
    hs = handshake_symbol(symbol)
    if hs.status != "Inducted":
        return hs

    # Route to SCS symbolic memory if the hint looks like ENTRY/BLOCK/URL
    if _looks_like_scs_hint(target_hint):
        try:
            mem = fetch_scs_memory(target_hint)
            payload = {"fossil": hs.fossil, "memory": {"url": mem["url"], "sha256": mem["sha256"]}}
            bound_hash = _sha12_str(json.dumps(payload, sort_keys=True))
            return HandshakeResult(status="Connected:SymbolicMemory",
                                   coherence=hs.coherence,
                                   fossil=f"{hs.fossil}:{bound_hash}",
                                   phrase=SUCCESS_PHRASE,
                                   tags=hs.tags + ("#scs_memory",),
                                   memory=mem)
        except Exception as e:
            return HandshakeResult(status=f"Failed:SCS:{type(e).__name__}: {str(e)[:180]}",
                                   coherence=0.0,
                                   phrase=FAIL_PHRASE)

    # Otherwise, if it looks like an X Community, go that route
    if _looks_like_x_community(target_hint):
        try:
            cid = resolve_community_id(target_hint)
            meta = fetch_community_metadata(cid)
            payload = {"fossil": hs.fossil, "community": meta}
            bound_hash = _sha12_str(json.dumps(payload, sort_keys=True))
            return HandshakeResult(status="Connected:XCommunity",
                                   coherence=hs.coherence,
                                   fossil=f"{hs.fossil}:{bound_hash}",
                                   phrase=SUCCESS_PHRASE,
                                   tags=hs.tags,
                                   community=meta)
        except Exception as e:
            return HandshakeResult(status=f"Failed:X:{type(e).__name__}: {str(e)[:180]}",
                                   coherence=0.0,
                                   phrase=FAIL_PHRASE)

    # Fallback: not recognized
    return HandshakeResult(status="Failed:UnknownTarget",
                           coherence=0.0,
                           phrase=FAIL_PHRASE)


# ----------------- ENTRYPOINT -----------------

if __name__ == "__main__":
    # Examples:
    # 1) Symbolic memory via ENTRY/BLOCK token
    print("— SCS ENTRY example —")
    res1 = grid_connect("::⊞ᛒ::", "ENTRY_941")
    print(json.dumps({
        "status": res1.status,
        "phrase": res1.phrase,
        "fossil": res1.fossil,
        "coherence": res1.coherence,
        "memory": res1.memory,
    }, ensure_ascii=False, indent=2))

    print("\n— SCS BLOCK example —")
    res2 = grid_connect("::⊞ᛒ::", "BLOCK941")
    print(json.dumps({
        "status": res2.status,
        "phrase": res2.phrase,
        "fossil": res2.fossil,
        "coherence": res2.coherence,
        "memory": res2.memory,
    }, ensure_ascii=False, indent=2))

    # 2) Raw URL works too:
    # res3 = grid_connect("::⊞ᛒ::", "https://raw.githubusercontent.com/ShriekingNinja/SCS/refs/heads/main/Log/Entries/ENTRY_941.md")

    # 3) Optional X community (unchanged, requires X_BEARER):
    # resX = grid_connect("::⊞ᛒ::", "https://x.com/i/communities/1973916749792682115")
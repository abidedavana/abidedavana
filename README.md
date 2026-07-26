# Abid Edavana Zakir

Cybersecurity student. I build **security tooling for AI systems and the software supply chain** — guardrails on autonomous agents, a runtime gateway for LLM apps, and scanners for the places you can still act before code runs.

Most of these are working tools with tests and CI, not write-ups. Where a tool has limits, the repo says so.

---

## Security for AI systems

**[Bridge](https://github.com/abidedavana/Bridge)** — an autonomous CUDA → ROCm/HIP migration agent that ports a CUDA repo until it builds and passes its tests on AMD GPUs, one reviewable commit per fix. The interesting part is the trust boundary: it runs untrusted repo code *and* applies LLM-written diffs, so **every diff passes a mechanical policy gate before it touches disk** — a writable-path allowlist checked on both sides of each hunk, a forbidden-insertion denylist, no editing of tests, size caps, and rejection of symlink/mode/traversal tricks. A red-team test proves the gate rejects a live prompt-injection payload. 159 tests, CI green, MIT.
Proven on real AMD hardware (Radeon `gfx1100`, ROCm 7.2): an autonomous port to a passing `ctest` in 3 iterations. **[Zero-install demo →](https://abidedavana.github.io/Bridge/demo/)**
`Python` `ROCm/HIP` `LLM agents` `prompt-injection defense`

**[raqib-ai-soc](https://github.com/abidedavana/raqib-ai-soc)** — a self-hosted FastAPI gateway that sits in front of an LLM app and detects prompt injection, jailbreaks, secret/PII leakage and system-prompt extraction in real time. Detections are versioned YAML rules carrying per-rule **OWASP LLM Top 10** and **MITRE ATLAS** IDs, with SOAR playbooks, a Streamlit SOC dashboard and a SIEM sink. Its red-team harness scores its own detection and false-positive rates, publishes every miss, and gates CI on them.
`FastAPI` `detection-as-code` `OWASP-LLM` `MITRE ATLAS`

**[mcpscan](https://github.com/abidedavana/mcpscan)** — a posture scanner for **Model Context Protocol** servers. Point it at the same JSON config your MCP client already uses; it reports PASS/FAIL/INFO with exact remediation for hardcoded secrets, unauthenticated endpoints, missing Origin validation (CVE-2025-49596), weak session IDs and inconsistent tool annotations. 13 checks, each tagged spec-backed or inferred. Detection only — no exploitation. 92 tests.
`Python` `MCP` `AppSec` `misconfiguration detection`

## Software supply chain & privacy

**[Gatekeeper](https://github.com/abidedavana/Gatekeeper)** — screens every package in a `requirements.txt` / `package.json` against the live PyPI and npm APIs *before* installation, scoring typosquat risk (edit distance, confusable-character folding like `rn`→`m`) alongside registry-age and release-count heuristics. Emits a versioned JSON report for CI gating and a cleaned `manifest.safe`. 103 tests on a 3.10–3.12 matrix, Docker/GHCR, v1.0.0.
The README has a *"Deliberately not implemented"* section: maintainer account-age isn't exposed by the public registry APIs, so that signal is marked unavailable rather than guessed.
`Python` `asyncio` `supply-chain-security` `DevSecOps`

**[Scrubly](https://github.com/abidedavana/Scrubly)** — browser-only file tools (HEIC→JPG, image compress/resize, EXIF/GPS and PDF metadata stripping) where the no-upload guarantee is **enforced** by a strict CSP with `connect-src 'none'`, not just promised. The stripping logic is factored DOM-free so CI can geotag a JPEG and prove the GPS is actually gone.
`TypeScript` `Preact` `privacy` `CSP`

## Adversarial modeling / CPS security research

**[islanded-microgrid-fdi-sim](https://github.com/abidedavana/islanded-microgrid-fdi-sim)** — a from-scratch MATLAB simulator of an islanded, droop-controlled IEEE 33-bus microgrid (no slack bus), used to mount minimum-footprint **false-data-injection** attacks (`a = Hc`) that pass χ² bad-data detection while driving real frequency and voltage excursions into UFLS/UVLS relay trips. Ships labelled 2,600-sample datasets and a non-circular detectability study; the headline numbers recompute exactly from the committed CSVs.
`MATLAB` `Python` `state estimation` `ICS/CPS security`

## AI/ML systems engineering

No security angle claimed here — this is systems work.

**[kvpolicy](https://github.com/abidedavana/kvpolicy)** — quantized KV-cache persistence for local LLM agents: save a suspended session at 3.8× compression and resume 7–14× faster than re-running prefill. The benchmark harness (15+ policies, 3 models, multi-seed) also reports a **negative result against its own hypothesis** — attention-based eviction holds perplexity flat while destroying up to 61% of exact fact recall.
`PyTorch` `quantization` `LLM inference`

**[prefixforge](https://github.com/abidedavana/prefixforge)** — compiles LLM agent prompts into byte-stable, maximally cacheable prefixes (canonical tool schemas, dynamic-span detection, provider-correct cache breakpoints), then measures the cache hit rate you actually get back from provider usage fields.

**[greenToken](https://github.com/abidedavana/greenToken)** — measures energy, CO₂e and cost of LLM inference per token from real hardware counters (NVML, `nvidia-smi`, Linux RAPL), with a documented FLOPs-based fallback for API-hosted models.

---

### How to read this profile

Most of these were built in short, focused bursts, so read them as evidence of threat modeling, judgment and shipping speed — the depth is in the code and the published results, not the commit counts. Tests and CI are the norm here: 159 in Bridge, 103 in Gatekeeper, 92 in mcpscan.

📫 **abidedavana@gmail.com**

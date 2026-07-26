<div align="center">

# Abid Edavana Zakir

### Cybersecurity student · I build security tooling for AI systems and the software supply chain

Guardrails on autonomous agents · a runtime gateway for LLM apps · scanners for the places you can still act *before* code runs

<br>

[![Email](https://img.shields.io/badge/abidedavana%40gmail.com-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:abidedavana@gmail.com)
[![Live demo](https://img.shields.io/badge/Live_demo-Bridge-0A7BBB?style=for-the-badge&logo=githubpages&logoColor=white)](https://abidedavana.github.io/Bridge/demo/)

</div>

<br>

> Working tools with tests and CI — not write-ups.
> Where a tool has limits, the repo says so out loud.
> I also send patches upstream to the security tools I use — **[OWASP ZAP, AWS s2n-tls, Trivy, Prowler, cloud-init ↓](#-upstream-open-source-contributions)**

---

## 🛡️ Security for AI systems

### [Bridge](https://github.com/abidedavana/Bridge) — autonomous CUDA → ROCm migration agent

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![ROCm](https://img.shields.io/badge/ROCm%2FHIP-ED1C24?style=flat-square&logo=amd&logoColor=white)
![tests](https://img.shields.io/badge/tests-159_passing-2ea44f?style=flat-square)
![CI](https://img.shields.io/badge/CI-green-2ea44f?style=flat-square)
![v1.0.0](https://img.shields.io/badge/release-v1.0.0-blue?style=flat-square)
![MIT](https://img.shields.io/badge/license-MIT-lightgrey?style=flat-square)

Ports a CUDA repo until it **builds and passes its tests on AMD GPUs**, one reviewable commit per fix.

The interesting part is the trust boundary: it runs untrusted repo code *and* applies LLM-written diffs — so **every diff passes a mechanical policy gate before it touches disk**. Path allowlist checked on *both sides* of each hunk, forbidden-insertion denylist, no editing of tests, size caps, symlink/mode/traversal rejection. A red-team test proves the gate rejects a live prompt-injection payload.

**Proven on real AMD hardware** (Radeon `gfx1100`, ROCm 7.2) — autonomous port to a passing `ctest` in 3 iterations.
→ **[Try it with zero install](https://abidedavana.github.io/Bridge/demo/)**

<br>

### [raqib-ai-soc](https://github.com/abidedavana/raqib-ai-soc) — detection & response gateway for LLM apps

![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![OWASP](https://img.shields.io/badge/OWASP_LLM_Top_10-000000?style=flat-square&logo=owasp&logoColor=white)
![ATLAS](https://img.shields.io/badge/MITRE_ATLAS-C8102E?style=flat-square)
![detection-as-code](https://img.shields.io/badge/detection--as--code-YAML-6E4C13?style=flat-square)

Sits in front of an LLM app and detects prompt injection, jailbreaks, secret/PII leakage and system-prompt extraction in real time. Detections are **versioned YAML rules carrying per-rule OWASP LLM Top 10 and MITRE ATLAS IDs**, with SOAR playbooks, a Streamlit SOC dashboard and a SIEM sink.

Its red-team harness scores its own detection and false-positive rates, **publishes every miss**, and gates CI on them.

<br>

### [mcpscan](https://github.com/abidedavana/mcpscan) — posture scanner for MCP servers

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![tests](https://img.shields.io/badge/tests-92_passing-2ea44f?style=flat-square)
![checks](https://img.shields.io/badge/checks-13-blue?style=flat-square)
![MIT](https://img.shields.io/badge/license-MIT-lightgrey?style=flat-square)

Point it at the same JSON config your MCP client already uses. Reports PASS/FAIL/INFO with exact remediation for hardcoded secrets, unauthenticated endpoints, missing Origin validation (**CVE-2025-49596**), weak session IDs and inconsistent tool annotations.

Every check is tagged **spec-backed or inferred**, so heuristics are never dressed up as violations. Detection only — no exploitation.

---

## 🌍 Upstream open-source contributions

Patches sent to the security and infrastructure tools I actually use — working inside large
unfamiliar codebases, to each project's own review process.

| Project | Contribution | Status |
|---|---|---|
| [**supabase/supavisor**](https://github.com/supabase/supavisor/pull/1074) | Documented the `citext` + `pgbouncer=true` slowdown and its remedy | ![merged](https://img.shields.io/badge/merged-8957e5?style=flat-square) |
| [**zaproxy/zap-extensions**](https://github.com/zaproxy/zap-extensions/pull/7538) <sub>OWASP ZAP</sub> | Cache static resources during a client spider crawl (+684 lines) | ![open](https://img.shields.io/badge/open-2ea44f?style=flat-square) ![discussion](https://img.shields.io/badge/5_comments-blue?style=flat-square) |
| [**aws/s2n-tls**](https://github.com/aws/s2n-tls/pull/5985) | Allow the session cache to be enabled before setting cache callbacks | ![open](https://img.shields.io/badge/open-2ea44f?style=flat-square) |
| [**aws/s2n-tls**](https://github.com/aws/s2n-tls/pull/5995) | Add a mutual-TLS example to the Rust bindings | ![open](https://img.shields.io/badge/open-2ea44f?style=flat-square) |
| [**aquasecurity/trivy**](https://github.com/aquasecurity/trivy/pull/10956) | Fix a race on `GIT_TERMINAL_PROMPT` in the Terraform remote-module resolver | ![open](https://img.shields.io/badge/open-2ea44f?style=flat-square) |
| [**canonical/cloud-init**](https://github.com/canonical/cloud-init/pull/6928) | Create scratch-dir ancestors with mode `0700` (permissions hardening) | ![open](https://img.shields.io/badge/open-2ea44f?style=flat-square) |
| [**prowler-cloud/prowler**](https://github.com/prowler-cloud/prowler/pull/11913) | New check: `oss_bucket_versioning_enabled` for Alibaba Cloud | ![open](https://img.shields.io/badge/open-2ea44f?style=flat-square) |

<sub>Also reviewed PRs on aws/s2n-tls and canonical/cloud-init, and reported an issue on zaproxy/zaproxy.</sub>

---

## 📦 Supply chain & privacy

### [Gatekeeper](https://github.com/abidedavana/Gatekeeper) — typosquat screening before you install

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![tests](https://img.shields.io/badge/tests-103_passing-2ea44f?style=flat-square)
![matrix](https://img.shields.io/badge/CI-3.10_|_3.11_|_3.12-2ea44f?style=flat-square)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![v1.0.0](https://img.shields.io/badge/release-v1.0.0-blue?style=flat-square)

Screens every package in a `requirements.txt` / `package.json` against the **live PyPI and npm APIs** before installation — typosquat risk via edit distance and confusable-character folding (`rn`→`m`, `0`→`o`), plus registry-age and release-count heuristics. Emits a versioned JSON report for CI gating and a cleaned `manifest.safe`.

> The README has a **"Deliberately not implemented"** section: maintainer account-age isn't exposed by the public registry APIs, so that signal is marked *unavailable* rather than guessed.

<br>

### [Scrubly](https://github.com/abidedavana/Scrubly) — file tools that never upload your files

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Preact](https://img.shields.io/badge/Preact-673AB8?style=flat-square&logo=preact&logoColor=white)
![CSP](https://img.shields.io/badge/CSP-connect--src_'none'-2ea44f?style=flat-square)

HEIC→JPG, image compress/resize, EXIF/GPS and PDF metadata stripping — where the no-upload guarantee is **enforced** by a strict CSP with `connect-src 'none'`, not merely promised. Stripping logic is factored DOM-free so CI can geotag a JPEG and prove the GPS is actually gone.

---

## ⚡ Adversarial modeling · CPS security research

### [islanded-microgrid-fdi-sim](https://github.com/abidedavana/islanded-microgrid-fdi-sim) — stealthy false-data injection on a power grid

![MATLAB](https://img.shields.io/badge/MATLAB-0076A8?style=flat-square&logo=mathworks&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![dataset](https://img.shields.io/badge/labelled_dataset-2600_samples-blue?style=flat-square)

A from-scratch simulator of an islanded, droop-controlled **IEEE 33-bus microgrid** (no slack bus), used to mount minimum-footprint FDI attacks (`a = Hc`) that **pass χ² bad-data detection** while driving real frequency and voltage excursions into UFLS/UVLS relay trips.

Ships labelled datasets and a non-circular detectability study — the headline numbers recompute exactly from the committed CSVs.

---

## 🧪 AI/ML systems engineering

<sub>No security angle claimed here — this is systems work.</sub>

| Project | What it does |
|---|---|
| **[kvpolicy](https://github.com/abidedavana/kvpolicy)** ![PyTorch](https://img.shields.io/badge/-PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white) | Quantized KV-cache persistence for local LLM agents — resume a suspended session at **3.8× compression, 7–14× faster** than re-running prefill. Also reports a **negative result against its own hypothesis**: attention-based eviction holds perplexity flat while destroying up to 61% of exact fact recall. |
| **[prefixforge](https://github.com/abidedavana/prefixforge)** ![Python](https://img.shields.io/badge/-Python-3776AB?style=flat-square&logo=python&logoColor=white) | Compiles agent prompts into **byte-stable, maximally cacheable prefixes** (canonical tool schemas, dynamic-span detection, provider-correct breakpoints), then measures the cache hit rate providers actually report back. |
| **[greenToken](https://github.com/abidedavana/greenToken)** ![Python](https://img.shields.io/badge/-Python-3776AB?style=flat-square&logo=python&logoColor=white) | Measures energy, CO₂e and cost of LLM inference **per token** from real hardware counters (NVML, `nvidia-smi`, Linux RAPL), with a documented FLOPs fallback for hosted models. |

---

<div align="center">

### Toolbox

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![C++](https://img.shields.io/badge/C%2B%2B-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)
![MATLAB](https://img.shields.io/badge/MATLAB-0076A8?style=for-the-badge&logo=mathworks&logoColor=white)

![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonwebservices&logoColor=white)

![ROCm](https://img.shields.io/badge/ROCm-ED1C24?style=for-the-badge&logo=amd&logoColor=white)
![CUDA](https://img.shields.io/badge/CUDA-76B900?style=for-the-badge&logo=nvidia&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

</div>

</div>


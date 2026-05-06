# Reflection - Lab 20 (Personal Report)

**Ho Ten:** Your Name
**Cohort:** A20-K1
**Ngay submit:** 2026-05-06

---

## 1. Hardware spec (from `00-setup/detect-hardware.py`)

- **OS:** Windows 11
- **CPU:** AMD64
- **Cores:** 8 physical / 8 logical
- **CPU extensions:** AVX2
- **RAM:** 15.8 GB
- **Accelerator:** CPU only
- **llama.cpp backend da chon:** CPU
- **Recommended model tier:** Qwen2.5-1.5B

Setup story:

Windows global `python` points to the Microsoft Store stub, so I ran the lab through `.venv\Scripts\python.exe` directly. I also reused the existing model files under `models/` and regenerated the setup artifacts so the repo had valid `hardware.json` and `models/active.json`.

---

## 2. Track 01 - Quickstart numbers

| Model | Load (ms) | TTFT P50/P95 (ms) | TPOT P50/P95 (ms) | E2E P50/P95/P99 (ms) | Decode rate (tok/s) |
|---|--:|--:|--:|--:|--:|
| Q4_K_M | 3146 | 390 / 828 | 93.0 / 140.3 | 5982 / 7897 / 8355 | 10.8 |
| Q2_K | 989 | 476 / 629 | 75.1 / 97.2 | 5236 / 6579 / 6707 | 13.3 |

Observation:

Q2_K loads faster and decodes a bit faster, but Q4_K_M is still the better quality/speed balance for the lab path on this machine.

---

## 3. Track 02 - llama-server load test

| Concurrency | Total RPS | TTFB P50 (ms) | E2E P95 (ms) | E2E P99 (ms) | Failures |
|--:|--:|--:|--:|--:|--:|
| 10 | 0.18 | 25000 | 49000 | 49000 | 0 |
| 50 | 0.16 | 28000 | 48000 | 48000 | 0 |

KV-cache observation:

I could not collect Prometheus `/metrics` from this runtime because the installed `llama_cpp.server` does not accept `--metrics`, so the endpoint was not available here. The load tests still ran cleanly with zero failures, but the KV-cache ratio observation is unavailable in this environment.

---

## 4. Track 03 - Milestone integration

- **N16 (Cloud/IaC):** localhost only
- **N17 (Data pipeline):** stub: toy docs in `pipeline.py`
- **N18 (Lakehouse):** stub: in-memory data, no real lakehouse
- **N19 (Vector + Feature Store):** stub: keyword-overlap retrieval

Timings from `pipeline.py`:

- embed: not used
- retrieve: 0.0-0.1 ms
- llama-server: 7.3-9.0 s

Reflection:

The main bottleneck was the local model generation call. Retrieval was effectively free compared with the CPU-only server.

---

## 5. Bonus - The single change that mattered most

Change:

Use Q2_K for faster loading, but keep Q4_K_M as the main lab model.

Before vs after:

before: Q4_K_M load 3146 ms, decode 10.8 tok/s
after:  Q2_K load 989 ms, decode 13.3 tok/s
speedup: ~3.2x load, ~1.2x decode

Why it works:

Q2_K uses less memory and is cheaper to load, which matters on a 16 GB class laptop. Q4_K_M is still a stronger default when quality matters, so the practical choice is not pure speed but the best balance for the available RAM.

---

## 6. Optional - Most surprising thing

The server-side runtime was more fragile than the model benchmark: the benchmark ran cleanly, but the server path needed extra dependency setup and did not expose `/metrics` in this environment.

---

## 7. Self-graded checklist

- [x] `hardware.json` committed
- [x] `models/active.json` committed
- [x] `benchmarks/01-quickstart-results.md` committed
- [x] `benchmarks/02-server-results.md` committed
- [ ] `benchmarks/bonus-*.md` committed
- [x] At least 6 screenshots in `submission/screenshots/`
- [ ] `make verify` exit 0
- [ ] Repo on GitHub is public
- [ ] Public repo URL pasted into LMS


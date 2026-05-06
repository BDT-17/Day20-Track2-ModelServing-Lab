# 02 — Server Results

## Track 02 Load Tests

| Concurrency | Total RPS | TTFB P50 (ms) | E2E P95 (ms) | E2E P99 (ms) | Failures |
|--:|--:|--:|--:|--:|--:|
| 10 | 0.18 | 25000 | 49000 | 49000 | 0 |
| 50 | 0.16 | 28000 | 48000 | 48000 | 0 |

## Notes

- Server responded successfully to `POST /v1/chat/completions` during smoke test.
- The `llama_cpp.server` build used in this environment does not accept `--metrics`, so the `/metrics` evidence requested in the README was not available from this runtime.
- Load-test numbers were collected from the headless locust summary at the end of each run.

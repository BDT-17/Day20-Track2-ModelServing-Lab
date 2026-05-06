# 03 — Integration Notes

I connected the track 03 skeleton to the local `llama-server` endpoint from track 02 and used the built-in toy retrieval set as the N19 stand-in. The pipeline runs three example questions end-to-end, prints retrieved-context provenance, and sends an OpenAI-compatible request to `http://127.0.0.1:8080/v1/chat/completions`.

What is real:
- Local llama-server on port 8080
- HTTP chat-completions call via `httpx`
- End-to-end timing for retrieve vs LLM call

What is stubbed:
- N16 cloud/IaC is localhost-only
- N17 and N18 are represented by the toy context set instead of a real pipeline/lakehouse
- N19 vector search is a keyword-overlap retrieval stub inside `pipeline.py`

Observed timings from one run:
- retrieve: ~0.0-0.1 ms
- llama-server: ~7.3-9.0 s

The main bottleneck was the local model generation call, not retrieval. That matched expectations on a CPU-only laptop, where the server dominates total latency.

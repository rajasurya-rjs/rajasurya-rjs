# Rajasurya J

I build software whose behaviour can be checked: network servers written against an explicit byte-level contract, and LLM applications where code, not the model, enforces the rules. I'm an SDE intern at Turocrates.ai. That work lives in private repositories, so everything below is public work you can read, build and run.

Start with [calc_server](https://github.com/rajasurya-rjs/network_arc_rajasurya_10086) for systems work, or the [procurement copilot](https://github.com/rajasurya-rjs/fde-3-) for applied AI.

## What I'm interested in

- **Where protocol implementations break.** Request framing, partial reads and writes, pipelining, slow readers, half-closed connections, and how a server closes without losing its last response.
- **Making LLM systems measurable.** Keeping approvals and policy in deterministic code, restricting what the model may assert, and comparing designs on frozen test cases instead of by impression.
- **Not trusting a number until the data supports it.** Validating inputs, reconciling totals, and withholding a figure that cannot be defended.

## Selected work

These are coursework or assignment projects, each built over days, not months. Each is complete and has its own tests or evaluation. None is production software.

### [calc_server](https://github.com/rajasurya-rjs/network_arc_rajasurya_10086): a calculator over persistent TCP (C++17)

A server on raw POSIX sockets with no HTTP or networking library. It finds request boundaries in the byte stream itself and consumes exactly `Content-Length` bytes. It supports keep-alive and pipelining, so one connection can carry many requests. It runs on a single thread with `poll()` and non-blocking sockets, with explicit output buffering and a staged close.

- **Evidence:** `make test` runs 49 unit cases and 46 integration tests against the built binary. I ran both and all pass. Annotated byte-level traces are in [`docs/network-trace.md`](https://github.com/rajasurya-rjs/network_arc_rajasurya_10086/blob/main/docs/network-trace.md).
- **Read:** [`docs/design.md`](https://github.com/rajasurya-rjs/network_arc_rajasurya_10086/blob/main/docs/design.md) covers the concurrency model and the alternatives it rejects. The README has a limitations section.

### [BHTTP/1](https://github.com/rajasurya-rjs/Network-Architecture-Project): a binary file-retrieval protocol (C11)

A server (`observe`) and client (`bcurl`) for a small binary protocol, built against a written wire specification: a 10-byte envelope, a static header-name table, and unknown frame types skipped by length. The spec is [`SPEC.md`](https://github.com/rajasurya-rjs/Network-Architecture-Project/blob/main/SPEC.md).

- **Evidence:** the suite has 45 Python tests plus a C I/O test. They cover malformed payloads, truncated frames, symlink handling, and unknown frames mixed with valid requests. I ran `make test` and all pass.

### [AI Procurement Request Copilot](https://github.com/rajasurya-rjs/fde-3-) (Python)

A workspace that recommends the next action on a purchase request and keeps human approval separate from the AI recommendation. The model only proposes an action and cites evidence IDs. Budgets, approval chains and risk flags come from read-only tools and deterministic policy code, and a validator rejects invented citations.

- **Evaluation:** a single-agent design and a staged two-agent design were compared on 26 frozen cases, 104 measured runs in total. Both got every final action right. The staged design was slower (15.0 s vs 8.9 s mean) and its raw proposals were less often correct (92.3% vs 100%), so the README recommends the simpler design.
- **Limits, stated in the README:** these are synthetic cases used during development, on one local model. They are not a held-out benchmark. The prototype has no authenticated approver.
- **Evidence:** I ran the 128 tests in offline mode and all pass. I did not re-run the model evaluation. Its raw runs are in [`evals/results/`](https://github.com/rajasurya-rjs/fde-3-/tree/main/evals/results).

### [WAV Service Scorecard](https://github.com/rajasurya-rjs/fde_assigment_rajasurya_10086) (Python, SQL)

A pipeline over about 21 million monthly NYC ride-hail trip records. It checks the wheelchair-accessible 10-minute standard against two control totals and quarantines impossible records. It declines to report a figure the data cannot support. By the README's account, one dispatcher's timestamps put the vehicle arriving before the request on most WAV trips, so the pipeline withholds its compliance rate. Quality findings and decisions are written up in [`docs/data_quality_findings.md`](https://github.com/rajasurya-rjs/fde_assigment_rajasurya_10086/blob/main/docs/data_quality_findings.md). I read these results but did not re-run the pipeline.

### [AI Persona](https://github.com/rajasurya-rjs/scaler-ai-persona): voice agent and grounded chat (TypeScript)

A phone-callable voice agent and a public chat that answer questions about my work from a retrieval index of my résumé and repositories, and book interviews on a real calendar. Retrieval is evaluated separately from generation. [`report/eval-report.md`](https://github.com/rajasurya-rjs/scaler-ai-persona/blob/main/report/eval-report.md) records three failures found in testing and their fixes. A [live demo](https://scaler-ai-persona-rho.vercel.app) is deployed.

- **Limits:** the evaluation is small (17 golden questions, 2 live voice calls), and the LLM judge comes from the same model family as the generator, as the report notes.

Smaller: an [LLVM pass plugin](https://github.com/rajasurya-rjs/sargantana-Coding-Challenge) that counts loads and stores per function (a mentorship coding challenge), and an [incident-response environment](https://github.com/rajasurya-rjs/incident-response-openenv) for training agents on simulated infrastructure outages.

## Toolkit

Used in the projects above.

- **Systems:** C, C++17, POSIX sockets, Make, LLVM (New Pass Manager)
- **Python:** FastAPI, Pydantic, pytest, DuckDB and SQL
- **Web and AI:** TypeScript, Next.js, retrieval-augmented generation, tool-calling agents, local models via Ollama

## Links

[GitHub](https://github.com/rajasurya-rjs)

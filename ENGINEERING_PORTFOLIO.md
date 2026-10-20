# MaxGab — AI-Native Engineering Portfolio

**Public evidence, runnable tests, and engineering decisions** · October 2026

[GitHub profile](https://github.com/MaxGab) · [KAELIX Local Data MCP](https://github.com/MaxGab/kaelix-data-mcp) · [CSV audit sample](https://github.com/MaxGab/kaelix-solana-sentinel/tree/main/examples/kaelix-data-audit) · [Email](mailto:dragosgoran62@gmail.com)

> I use AI coding assistants, including ChatGPT, Codex and Grok, to help translate requirements into working code. My role spans product definition, task breakdown, guiding implementation, inspecting changes and validating results. This portfolio distinguishes verifiable public software from private R&D and explains what I can demonstrate in a technical conversation.

## Start here: two projects with code

| Project | Technical evidence | Source |
| --- | --- | --- |
| **KAELIX Local Data MCP (Python)** | MCP over JSON-RPC/stdio; three discoverable tools; explicit argument schemas; size limits; JSON duplicate-key rejection; responses that report aggregate data instead of returning raw CSV rows; tests exercising the actual subprocess handshake and rejection paths. | [Code](https://github.com/MaxGab/kaelix-data-mcp/blob/main/kaelix_mcp.py) · [Tests](https://github.com/MaxGab/kaelix-data-mcp/blob/main/test_kaelix_mcp.py) |
| **KAELIX CSV Data Audit (Python)** | Local CSV parsing, schema checks, duplicated headers/rows, malformed-width counts, optional email checks and structured reporting; no remote upload of the input file. | [Code](https://github.com/MaxGab/kaelix-solana-sentinel/blob/main/examples/kaelix-data-audit/csv_audit.py) · [Tests](https://github.com/MaxGab/kaelix-solana-sentinel/blob/main/examples/kaelix-data-audit/test_csv_audit.py) |

The repositories document **25 MCP tests** and **15 CSV audit tests**. These are project-level tests included in source, **not independent security audits or claims of paid production use**.

### Reproduce the MCP sample

Requires Python 3.10+ and no external Python dependencies.

```bash
git clone https://github.com/MaxGab/kaelix-data-mcp.git
cd kaelix-data-mcp
python3 -m unittest -v test_kaelix_mcp.py
python3 kaelix_mcp.py
```

After starting the server, send each JSON object below as one line on stdin (the server speaks newline-delimited JSON-RPC):

```json
{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-06-18","capabilities":{},"clientInfo":{"name":"portfolio-demo","version":"1.0"}}}
{"jsonrpc":"2.0","id":2,"method":"tools/list"}
{"jsonrpc":"2.0","id":3,"method":"tools/call","params":{"name":"kaelix_audit_csv","arguments":{"csv_text":"id,total\n1,20\n1,20\n2,\n"}}}
```

Expected behavior: the tool reports a duplicate row and an empty cell, and does not echo the input's raw customer rows. The repository's tests also cover malformed inputs, unknown operations, argument validation and an end-to-end stdio exchange.

### Why I consider the boundaries part of the feature

A useful AI-facing tool is not just a function that returns the happy-path answer. The implementation makes concrete choices that a reviewer can inspect:

1. **Constrain the surface:** three named tools and defined input schemas instead of unconstrained shell, filesystem or wallet access.
2. **Bound cost and misuse:** limits on request size and CSV dimensions instead of unlimited input.
3. **Reduce exposure:** report aggregate quality counts rather than arbitrary row contents.
4. **Test refusal paths:** errors for malformed requests, unknown methods, unexpected arguments and duplicate JSON keys.
5. **Leave clear limits:** a local utility with these tests is not equivalent to production governance, authorization, sandboxing or a full DLP system.

A useful next iteration would add structured performance measurements, richer protocol compatibility tests and a threat-model review before treating the utility as suitable for external client data.

## Larger system: ELI OS (private, in development)

**ELI OS is my central personal AI systems project.** KAELIX is a smaller set of developer tools and commercial workflows intended to connect to that core later.

The architectural goal is:

```text
User input
  → Context and intent
  → Planner
  → Tool discovery / model router
  → Worker orchestration
  → Permission and approval checks
  → Tool execution
  → Verification and structured results
  → Memory / response
```

The ongoing work explores worker isolation, routing, voice, multi-device connectivity and release gates. Some subsystems have been tested locally, while others are staged prototypes. **I do not claim that ELI OS is a completed autonomous platform or that KAELIX is already integrated.** I can discuss specific engineering choices and evidence at a technical interview, including what I would redesign.

Related local preview: **ELI Virtual Office**, a React/PixiJS + FastAPI/WebSocket visualization with simulated agent states — **not** live control of the ELI worker fleet.

## An engineering question I would like to solve

**How should an AI agent access a shared knowledge/tool system without inheriting more privileges than the requesting user?**

For an AI platform with MCP tools or multiple agents, I would explore this design (an **idea for a future proof-of-concept, not an implemented feature in my public MCP server**):

- Bind every request to a real principal and a specific agent identity.
- Resolve permissions **before** any retrieval, tool selection or model context expansion.
- Default unknown tool names, resources and scopes to **deny**.
- Record a content-minimal audit event for allowed **and** denied actions.
- Add negative tests: revoked access, agent impersonation, cross-tenant requests, prompt attempts to bypass scope, and replayed requests.
- Make a human approval gate explicit for risky writes instead of relying on a model's assertion that an action is safe.

What interests me most about modern AI infrastructure is this transition from **agents that can act** to **agents whose actions can be tested, controlled and explained**. I would be happy to implement and review a small vertical slice of the above as a live technical exercise.

## Additional project context

- **KAELIX Solana Sentinel:** service-oriented work around read-only monitoring of public Solana addresses. It is not a wallet, DeFi execution bot, or demonstration of deployed smart contracts. [Public information](https://github.com/MaxGab/kaelix-solana-sentinel).
- **Matrixio:** manufacturing/installation workflow and dashboard project, private, in development.
- **ARRIVAL AI Agency / StoreFix AI:** private application and service workflow MVPs, with local testing; not presented as active paid production deployments.
- **Roblox / game tooling:** interactive systems and iterative QA work.
- **FellixBot:** earlier Node.js / discord.js bot work — [public repository](https://github.com/MaxGab/fellixbot).

## What I can walk through in a screening call

**15-minute outline:**
1. *2 min*: the problem the local MCP tool solves and why I chose stdio.
2. *4 min*: tool discovery, input validation, CSV parsing and structured output.
3. *3 min*: one positive test and one expected-rejection test.
4. *3 min*: an edge case, trade-off or limitation in the current design.
5. *3 min*: how I use coding agents, review their work and decide when a result is ready.

**Current goal:** an early-career role building dependable AI-assisted software. I am based in Bucharest, available for full-time remote work, and comfortable conducting an interview in English (B2).

---

**Contact:** [dragosgoran62@gmail.com](mailto:dragosgoran62@gmail.com) · [GitHub / MaxGab](https://github.com/MaxGab)

*This document describes code that can be inspected, plus clearly identified private R&D and proposed work; it does not claim endorsement by a company or an independently audited product.*

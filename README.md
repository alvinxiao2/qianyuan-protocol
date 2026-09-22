# Qian Yuan — portable work record for agents

**This repository is a publication anchor. It contains no implementation code yet.**

The core implementation is not published in this repository pending a per-file
disclosure review. What is here is the citable record of the design.

## Design paper

**Toward a Portable Work Record for Agents: A Design for Identity, Reputation, and
Discovery Above the Platform** — Qian Yuan (乾元), `qianyuan/v3`.

- Preprint v1 (2026-09-21): **DOI `10.5281/zenodo.22876346`**
- Source specification snapshot: `SIGNING-SPEC.md` (in the Zenodo deposit, not in this
  repository) and the public agent-facing surface accompanying the deposit.
- Service under study: <https://qianyuan.ltd>
- Full text: [`paper/paper-qianyuan-v1.md`](paper/paper-qianyuan-v1.md) — byte-identical to
the file deposited on Zenodo (SHA-256 `e75d5a68ced538ca7fb5284cd475231c1ec28f5dacdd76f8ada826292ac51735`)

**This is a design paper. It reports no results.** It states what is implemented, what is
designed, and what is not implemented. Three boundaries are named explicitly: portability is
bounded (the identity is the agent's; the record is currently hosted on a service operated by
this project), acceptance is bounded (a signature proves the submitter asserted a result, not
that the command ran), and security/governance are bounded (sybil resistance, collusion
detection, external identity verification, prompt-injection mitigation, and dispute governance
are not implemented or not designed).

## Patent stance

**No patent application is intended for this design.** This project publishes as defensive
prior art. See [`PATENTS.md`](PATENTS.md).

## Licence

Two licences, split by content:

- **Code** (`sdk/`, `examples/`, configs): Apache License 2.0 — see [`LICENSE`](LICENSE).
- **Paper** (`paper/`): Creative Commons Attribution 4.0 International (CC BY 4.0).

Copyright 深圳市演化智能科技有限公司 (Shenzhen Evolution Intelligence Technology Co., Ltd.).

## Connect a client

Framework-specific walkthroughs live in [`examples/`](examples/) — AutoGen, CrewAI,
LangChain, OpenClaw, and DeepSeek Harness (DSH).

The MCP endpoint is `https://qianyuan.ltd/mcp` (Streamable HTTP). Reads are open; no key
is required.

```bash
# DSH: bundle that adds the endpoint as one mcp-client entry
dsh plugin --profile web add qianyuan-ltd/dsh-qianyuan
```

## Contact

`contact@qianyuan.ltd`

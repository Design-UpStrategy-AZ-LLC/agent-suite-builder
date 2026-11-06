---
layout: page
title: "The nine jobs"
nav_order: 2
---

Each job is one skill. Your assistant loads only the one your request needs.

## Which job runs

| User said something like… | Job | Load this skill | Write this |
|---|---|---|---|
| "Build me an agent that…", "connect X and Y", "I want a digest / bot / suite" | **Scope** | `scope-to-suite` | `<project>/blueprint.md` |
| "Implement this", "make it work", "write the code", "write the LangGraph / Crew / SDK" | **Implement** | `implement-agent-from-blueprint` | code + tests + a prompt file and eval set per LLM step (Quality card) |
| "Test it properly", "try to break it", "is it really safe?", and automatically after Implement | **Verify** | `verify-agent-suite` | `verify-<date>.md` (testers, fixes with regression tests, re-test, council summary, verdict) |
| "Make it run", "make it run every morning", "connect my real accounts", "put it live", "deploy it" | **Run** | `run-agent-suite` | the schedule + `RUNBOOK.md` (`handoff.md` when a developer should own it) |
| "Document it", "write the docs", "a README / architecture doc / user guide for this" | **Document** | `document-agent-suite` | `shared/docs/` (agent cards, tools and permissions, operations, architecture, ADRs, user guide) |
| "It's broken", "the digest is wrong", "this trace failed" | **Debug** | `debug-failing-agent` | debug record + a regression case |
| "Review this system", "we inherited this", "is this safe to ship?" | **Audit** | `audit-existing-agent-system` | audit report with evidence |
| "Raise this", "raise it", "lead standard", "use the schemas" | **Raise** | `raise-existing-agent` | `lead-audit-<date>.md` + `upgrade-plan-<date>.md` (Together lines if 2+ Fails) |
| "How do I use this pack?", first message, unclear | **Bootstrap** | this skill | the route table above |

## Start — `using-agent-systems`

Start every session. Pick one of the daily jobs, load that skill, and produce an artefact in the user's project.

**Steps**

1. Set the builder mode once (Vibe / Developer / Together, below). Then match the user line to the table (Scope / Implement / Verify / Run / Document / Debug / Audit / Raise / Bootstrap).
2. Load **only** that child skill. Do not load pattern groups until it names them.
3. Copy the closest worked example (table below) before inventing a shape.
4. Paste the cards your job's skill names, verbatim (each child skill lists its cards; all nine are below for reference). Research before any claim about an outside system; cite the source or mark it `UNVERIFIED`.
5. Write the artefact in the user's project. Stop when that file exists and the child skill's acceptance bar is green.

## Scope — `scope-to-suite`

Turn an informal prompt ("connect Slack, Gmail, and Jira; give me task updates") into a **Suite Blueprint** — one file that later skills implement. This is the Scope job.

**Steps**

1. Copy the closest worked example (`using-agent-systems`, Worked examples; `shared/examples/task-digest/` for a digest) as the shape. Do not invent a new layout.
2. Write `<project>/blueprint.md` from the § 12 template. Fill all **12** decisions in § 2's order: intent, scope boundary, data sources, integration research (with the MCP access card), archetypes (with Together lines), memory, tool allowlists, trigger, output contract, guardrails (with the human-gate card), done and eval pass bar, iteration plan.
3. Paste the human-gate card. Unsigned Send / Delete / money → Implement is later forbidden. The named signer approves through the approval channel; you record the `**Signed.**` line (§ 11).
4. Name connectors. Prefer an existing MCP server. **Do not invent Send.**
5. A blank cell is a stop, not a guess. Research the real API before naming a tool. Each § 12 cell follows `shared/schemas/cell-contract.md` (type / refuse / check / replay). Adjectives and `"the user"` are refuse.
6. Stop when `blueprint.md` exists and every cell has an answer or an explicit `unknown`. No code in this skill.

## Implement — `implement-agent-from-blueprint`

Turn a filled Suite Blueprint into **code**. This is the Implement job.

**Steps**

1. If `<project>/blueprint.md` is missing or a cell is blank → stop; run `scope-to-suite`. Do not guess.
2. Open `shared/schemas/frameworks/INDEX.md`. One framework only (plain Python, stdlib only, by default for a small suite; LangGraph if graph; CrewAI if crew; Claude Agent SDK if Anthropic harness; Mastra/Vercel if TS+MCP; n8n / OpenClaw skills if those substrates).
3. Map: each blueprint agent → one module; each allowlisted tool → one typed wrapper; output contract → Pydantic/Zod + renderer.
4. Scaffold `src/main`, `src/state`, `src/agents/`, `src/tools/`, `tests/test_golden_path.py`. No dashboard. No extra agent.
5. Implement Analyzers/Reviewers → Pipeline → Resolver. Orchestrator last and only if named.
6. Every mutating tool has a dry-run. Unsigned Send/Delete/money blueprint → do not write code. Read agent ≠ write token.
7. One fixture → one run → one assertion against the output contract. Fill one eval-case row (`id`, `input_ref`, `expected_tools`, `pass_bar`, `sad_path`, `pairing_id`) from `prepare-eval-set`. `--success true` is not that assertion. Copy the whole `shared/examples/task-digest/` folder when the job is that digest (`langgraph/` alone fails: its fixtures live in `../outputs` and `../edges`).
8. Live adapters (Step 5b): each typed wrapper gets a live adapter behind the same interface. A scheduled job cannot use the chat host's MCP connectors, so it needs its own credentials (OAuth client or API token, read scope first). Run § 3 connects them.
9. Before you say done, meet the Quality card (Step 6): static checks run and recorded, one test per edge fixture, a prompt file and an eval set with a numeric pass bar for each LLM step (run, not `eval: not run`), and tests that hold each prompt to the code. One golden assertion is one row, not the bar.

## Verify — `verify-agent-suite`

Before a suite goes live, try to break it the way a hostile user, a careless operator and a buyer would: fresh testers attack in parallel, fixers repair, fresh testers attack again, and every miss becomes a mechanical check. The builder's own tests only prove what the builder thought of.

**Steps**

1. Record the revision under test (`git rev-parse HEAD`) and the builder mode. Make a checkpoint commit.
2. Say roughly what the run uses (§ 2, budget), write one brief per tester (§ 3) and start all seven at once as fresh sub-agents, each report-only (§ 2).
3. Triage: dedupe, reproduce every SUSPECTED, assign severity (§ 4).
4. Fix: one fixer per folder, each fix with a test shown failing first, then passing.
5. Re-test wave: fresh testers on the fixed revision. Repeat up to the round budget (default 3).
6. Retro: for each confirmed finding, name the Quality-card row that missed it and add the mechanical prevention (§ 5).
7. Council (§ 6): the in-host council every time; the paid external council only on the user's yes in that turn. Write `verify-<date>.md` with the verdict. Report in plain words.

## Run — `run-agent-suite`

Take a suite that Implement built and tested, and make it run for real: on a schedule, on the user's own accounts, with alerts, a dry-run trial first, and a named person's yes before anything goes live. This is the job that lets a vibe coder finish without a developer, and that hands a developer a clean file when one is needed.

**Steps**

1. Preflight: run the tests and the Quality card checks yourself and show the result. Red tests → stop. A failed or unrun card row (an LLM step below its eval pass bar or with no eval run, no independent review) → the Quality gate above: dry-run only, nothing live. Unsigned blueprint on a suite that can Send, Delete or move money → stop.
2. Pick where it runs (table below): the simplest place that meets the blueprint's trigger. Research the current setup steps for that place (Research card).
3. Connect each account one at a time, read scopes first, through the live adapters Implement built. Secrets go in the place the table names, never in chat, never in git, never by asking the user to edit a file.
4. Run a dry-run trial: every Send, payment, filing or refund stays `pending_approval` for the trial window the blueprint sets (default 7 days). Alerts on (Alert card).
5. Go live only on a named person's yes (§ 5b), recorded with date and scope. One switch per mutating tool, each set on its own yes; one master off-switch rolls everything back. No open Critical or High finding at go-live (§ 5, open findings); the handoff (§ 6) is separate.
6. Write `RUNBOOK.md`. Hand over with `handoff.md` when the table at § 6 says a developer should own it.

## Document — `document-agent-suite`

Turn a built suite (blueprint, code, tests, `RUNBOOK.md`, alerts, audit) into technical documentation that is true today: written by the assistant, checked line by line against the code, and split by who reads it. This is the Document job.

**Steps**

1. Preflight. Run the tests yourself and keep the output. Red → stop. List the inputs you found and the ones that are missing (no `RUNBOOK.md` means "not run for real yet", not "nothing to say").
2. Pick the set (§ 2). Full set when the suite moves money, files with a regulator, or holds other people's personal data beyond the names and emails of the user's own contacts. Otherwise the minimum set.
3. Build the fact inventory from the **code**, not the blueprint (§ 4): agents, tools and allowlists, write paths and gates, schemas, files written, commands, exit codes, alerts, hooks, tests. Where the code and the blueprint differ, raise it with the user as a finding. Do not document either version until it is settled.
4. Write reference first (agent cards, tools and permissions), then operations, architecture, ADRs, and the user guide last.
5. Run every command you show and paste the real output. End every section with a `Source:` line (file, function or line, test name).
6. Check the docs against the code (§ 6 claim check). Mark anything you could not check `UNVERIFIED`. In Vibe mode, the user reviews only the user guide; you do the rest.
7. Add the changelog line and the regeneration rule (§ 7). Stop when Verification passes.

## Debug — `debug-failing-agent`

Go from "it failed" to a named failure, a first-wrong step, a fix, and a test that catches it next time.

**Steps**

1. Pick **one** failing run. Dated → do not use evidence from after that date.
2. Name the failure class (skipped gate, expired auth, tool not allowlisted, poisoned tool text, unchanged retry, `--success true` treated as a verifier).
3. Mark the **first-wrong** step. Do not start at the last symptom.
4. Fix that step only. Same-session review of the patch is not isolated — use a fresh child or a developer; if neither is available, record `review: not isolated` and do not go live.
5. Write `<suite>/debug/<date>-<short-name>.md` and one regression case.
6. If the record leaves the machine, follow Trace-release (keep subject + outcome; strip procedure). No single run but they want lead-grade → `raise-existing-agent`.

## Audit — `audit-existing-agent-system`

Review a system that already exists (inherited, acquired, or "we built this last quarter") and say what is missing, with evidence.

**Steps**

1. Inventory agents, tools, gates, connectors, and what can Send.
2. Cheap pass first: allowlist per role, write-scope on a read agent, Send without a named human. Those two last are **Critical**.
3. No severity until the finding is confirmed. If this session wrote the system under audit, the review is not isolated — use a fresh child or a developer; if neither is available, record `review: not isolated` and do not go live. An inherited system needs no fresh child.
4. Contest only with a file that child could have read. Verbal facts escalate; do not pad the artefact.
5. If the run is multi-stage: write each stage file before the next; do not overwrite a completed stage. SDK `success` is not terminal.
6. Write `<suite>/audit-<date>.md` (inventory, evidence, ranked fixes). Schema scorecard / lead standard → `raise-existing-agent`. Do not walk the catalogue here.

## Raise — `raise-existing-agent`

Attach to an inspectable existing agent (a folder you can read) and write a lead-standard review plus a PM upgrade plan. This is the Raise job.

**Steps**

1. Fail closed if there is no folder to read. This is not a black-box wrap.
2. Fill the five PM intake rows. Paste them at the top of both output files.
3. Score the 14-row lead scorecard. Every Fail cites a **Check-path**. Fail-if is mechanical (`shared/schemas/cell-contract.md`). Same-session reviewer or "done" from a Stop hook is a Fail.
4. Load **only** the schema slice that Fail names. Never the full pattern library.
5. Two or more Fails → compose card: one Together line per pair (A then B), never a hybrid template.
6. Write `lead-audit-<date>.md` and `upgrade-plan-<date>.md`. Report-only. Do not apply patches. Pressure: `shared/examples/task-digest/pressure/raise-apply-patches.md`.

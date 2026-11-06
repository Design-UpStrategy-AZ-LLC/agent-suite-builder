---
layout: page
title: "Getting started"
nav_order: 1
---

## 1. Connect Agensi and say what you want

**Try this first.** After subscribing, open your coding assistant in your project and type:

> Use Agent Suite Builder. Connect Slack, Gmail, and Jira. Give me task updates.

You get `blueprint.md`: a 12-part plan you can read and argue with — the goal, what is out of scope, the data sources, the safety rules, and how you will know it works. When a decision is missing, the assistant asks you instead of guessing. Next, ask: "Implement the blueprint."

## 2. Who is building (set once per project)

Ask once, in plain words: "Will you build this yourself with me, or is a developer involved?" Write the answer into the artefact header as `Builder: Vibe | Developer | Together`. Default: **Vibe**.

| Mode | Who does what | How you work |
|---|---|---|
| **Vibe** | The user describes and decides; you write, run and explain everything | Run every command yourself and say what happened in one plain sentence. Ask only for decisions (one question, your recommendation first) and for actions only they can do (sign in, approve a scope, say yes to go live). Never ask them to edit a file, read a stack trace or paste a secret into chat. Explain any term the first time. Keep `PROGRESS.md` (template in `shared/schemas/vibe-coding-support.md` § 9). |
| **Developer** | A developer reviews and owns the code | Leave a reviewable diff, a README with exact commands, and tests. Use the developer's framework and conventions. Ask technical questions directly. |
| **Together** | The user vibe-codes; a developer reviews at the gates | Work as Vibe. At each gate (signed blueprint, go-live, any High or Critical Audit finding) write `handoff.md` for the developer (`run-agent-suite` § 6). The signed-blueprint gate waits: no code until the blueprint is signed. At the other gates, keep going in dry-run while they review. |

The gates are the same in every mode: Vibe does not skip the human-gate card, the Research card, the Alert card or the Quality card. It only changes who types and how you explain.

## 3. How it talks to you

Full guidance and sources: `shared/schemas/vibe-coding-support.md`.

| Rule | Do | Check |
|---|---|---|
| Outcome first | Open every update with what is now true, in the user's words. | First sentence states the outcome |
| One decision | One question per message, your recommendation first, the other options still visible. | At most one question; the default is stated |
| Risk tier | Low (local, reversible): do it, then report. Medium: say it, then do it. High (accounts, permissions, cost): restate the consequence and wait for a yes. Critical (send, pay, file, delete, go live): human-gate card. | Each action has a tier; no approval asked for low ones |
| Errors | What happened, what it means for them, what you will do next. The same error twice means change approach and say so. | Three parts; no stack trace in the message |
| Terms | Define a term in one clause the first time; reuse the user's words; never rename a thing. | No undefined term; the glossary in `PROGRESS.md` matches |
| Done | "Done; next is …", only after you ran it or read the file. | Each "done" is backed by a run or a file |
| Undo | Commit a checkpoint before each change; say how to undo before any high or critical step. | Checkpoint before the change; undo line in `PROGRESS.md` |
| Never | Ask the user to edit a file, read a stack trace or paste a secret into chat. | None of these appear in the conversation |

More: [Vibe coding](vibe-coding.html).

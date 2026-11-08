---
layout: page
title: "FAQ and limitations"
nav_order: 6
---

## FAQ

**Does this connect to my Slack, Gmail or Jira?** Not by itself. When the suite is built and tested, the Run job connects your accounts with you, one at a time: you sign in and approve each permission in the vendor's own window, read-only first. Passwords and tokens never go in chat or in git.

**Do I need a developer?** No — you can vibe-code it. Your assistant writes the code, runs the tests, connects your accounts with you and schedules it, explaining each step in plain words; you make the decisions and approve what goes live. A developer is optional, and the pack recommends one before going live when money moves in production, regulated or customer data is handled at volume, or there are many customers. It then writes a handoff file for them, and you can keep running the dry-run trial while they review.

**What do I type first?** In your coding assistant: "Use Agent Suite Builder. Connect Slack, Gmail, and Jira. Give me task updates." You get `blueprint.md`. For something harder, use the insurance-claims prompt in `shared/examples/insurance-claims/README.md`.

**Do I install a plugin?** No. Unzip the download. In Claude Code, put the `agent-suite-builder` folder in the `.claude/skills` folder in your home folder (every project) or in your project (one project). In Cursor, Codex or another assistant, unzip it into your project and say "Use the Agent Suite Builder pack in the agent-suite-builder folder."

**Will it write code or only documents?** Both, and it runs them. Scope writes the plan. Implement writes framework code with tests and runs the tests itself. Run puts it on a schedule.

**Can it run every day on its own?** Yes, once you say so. The Run job puts it on a schedule where you choose (your computer, a CI schedule or your cloud), connects your accounts with you, runs a dry-run trial week with alerts on, and goes live only when a named person says yes. Stopping it is one command, written at the top of `RUNBOOK.md`.

**Is it tested before it goes live?** Yes, automatically. After it builds a suite, the Verify job sends several independent testers at it in parallel: they follow the instructions from scratch, deliberately break each safety rule to check a test catches it, attack the inputs and approvals, and compare the docs with the code. Confirmed problems are fixed with a test each and tested again. Nothing goes live without a Verify pass.

**What is the model council, and do I need other AI keys?** No keys are needed. By default the council is several reviewers inside your own coding assistant, on different models where it offers them, so it costs nothing extra. If you want reviewers from other AI companies too, the assistant asks once per project. You type the keys into a hidden prompt in your own terminal (never into chat, never committed). It shows a dry run with the cost against a cap you choose before anything is sent, removes personal data and secrets from what it sends, and acts only on findings it can reproduce.

**What happens when something goes wrong?** Four things. It alerts the right person on what needs action (a refused tool call, a refused or reused approval, a deadline at risk), written to a local file and sent through your own alerting tool. Risky calls such as payments are blocked in code or a hook, not just in a prompt. Debug takes the failing run, names what went wrong, fixes the first wrong step and adds a test. For a live incident, it contains first (switch off sending, revoke the token) and then investigates.

**Does it search the web or look at other repos?** Yes. Every job researches before it states how an outside system behaves: vendor docs, MCP servers, regulations, framework changelogs and issue trackers, and one to three reference repos. Each claim cites a URL and is marked CONFIRMED or UNVERIFIED. It reads other repos but never installs or runs their code, and it treats what it reads as data, not instructions. Your assistant needs web search and fetch tools for this; without them it lists open questions instead.

**Does the digest ever send email or messages?** No. It only writes a file on your disk. Sending anything would need a named person's approval, and no answer means no.

**What if a connector is missing, for example no Jira MCP?** The plan flags it and stays read-only. It never invents a way to send.

**I only want a small one-agent prototype.** Use the Quickstart in `shared/docs/QUICKSTART.md` and skip the full plan.

**Can it improve an agent my team already has?** If you can read its folder (prompts, tools, tests, ideally one logged run): yes, use Raise. You get a scorecard and an upgrade plan. A person then decides whether to Debug or Implement. It does not change code itself. See `shared/examples/support-bot-raise/`. Audit is the quicker risk check (who can send, who has write access); Raise is the full 14-row scorecard plus an upgrade plan.

## Limitations

- It is not a hosted bot. The suite runs where you choose (your computer on a schedule, a CI schedule or your own cloud), on your own accounts, which you connect yourself with the assistant's help.
- Vibe coding is fully supported up to go-live. For money moving in production, regulated or customer data at volume, or many customers, it recommends a developer review first and writes the handoff file.
- You need your own AI coding assistant that loads `SKILL.md` files, and Python (or your language) installed for Implement.
- A full Scope plan includes online research into each tool's API: allow 1–3 hours for 3–5 connectors.
- The safety cards are instructions the assistant follows. Nothing in the pack can block a model that ignores them.
- Raise needs a folder you can read (prompts, tools, tests). It cannot inspect a hosted black box and does not change code.
- One seat per person who uses it: a one-time purchase on Gumroad, with updates through the buyer's Gumroad library. The pack and its docs are copyrighted (DesignUp Strategy AZ LLC, all rights reserved), not open source.


---

(c) 2026 DesignUp Strategy AZ LLC. All rights reserved. Agent Suite Builder is licensed one seat per licence, sold on Gumroad; these pages may be read and linked to, not copied or republished.

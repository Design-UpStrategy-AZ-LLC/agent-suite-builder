---
layout: page
title: "Worked examples"
nav_order: 3
---

Four complete examples ship in the pack under `shared/examples/`. Each has runnable Python (standard library only) with tests, and the files each job wrote. All companies, people and data in them are fictional.

## Example — homeowners insurance claims, from first report to adjuster packet

A homeowner e-mails their insurer: "hail broke my roof, here are photos and the roofer's quote." This example builds the agents that read that e-mail, check the policy, apply the deductible correctly, note anything an investigator should look at, and give a licensed adjuster a ready-to-review packet with a suggested payout. The agents never pay anyone and never write to the homeowner on their own. A payment or a letter goes out only after a named adjuster approves it, and silence counts as "no". The insurer (Larkspur Mutual, FICTIONAL) and every person and claim here are invented.

Folder: `shared/examples/insurance-claims/`

| Job | What you type |
|---|---|
| 1. Scope — write the blueprint | Build me an agent suite for homeowners claims. A first notice of loss arrives by e-mail with photos. Extract it, check coverage against the policy, work out severity and the deductible, screen for fraud indicators without ever denying, and give the adjuster a packet with a payout recommendation. Payments and claimant letters must wait for a named adjuster. We operate in Texas. |
| 2. Implement — make it run | Implement `shared/examples/insurance-claims/blueprint.md` in plain Python, stdlib only, with one module per agent, typed tool wrappers, and dry-run payment and letter tools. |
| 3. A planted failure | see README (`shared/examples/insurance-claims/README.md`) |
| 4. Debug — name it, find the first wrong step, fix one thing | CLM-2026-0419 recommended $8,000.00 but the roofer, fence and contents lines add up to $18,600.00 with a $6,200.00 deductible. Debug it. |
| 5. Audit — review the final suite | Audit `shared/examples/insurance-claims/` before we register a live ClaimCenter connector. Is it safe to ship? |

## Example — import customs filing, from request to audit

A freight forwarder receives four documents for each shipment coming into the US: a commercial invoice, a packing list, a bill of lading and the buyer's purchase order. This example builds a small agent suite that reads them, checks they agree, suggests tariff codes, and drafts the US Customs (CBP) entry summary. It never files anything by itself. Every submission stays a dry-run until the forwarder's licensed customs broker approves that exact draft. The folder walks one suite through all five steps of its life: Scope, Implement, a planted bug, Debug, then Audit. Every company, person and number in it is invented.

Folder: `shared/examples/customs-filing/`

| Job | What you type |
|---|---|
| 1. Scope | Use agent-systems. Scope a suite for our freight-forwarding desk. Each US import arrives with a commercial invoice, packing list and bill of lading, plus the buyer's PO. Extract them, check quantities, values, currency and consignee against each other and the PO, suggest HTS codes for our licensed customs broker to confirm, and build the CBP entry summary draft. Nothing goes to ACE until the broker says yes. |
| 2. Implement | Implement shared/examples/customs-filing/blueprint.md in plain Python, stdlib only. Use fixtures.md for the documents. The submit tool must be a dry-run until the licensed-customs-broker approval is present. |
| 3. The planted failure | This shipment (B/L AMLU2610190042) passed reconciliation, but the supplier billed 9% over the PO and the draft says USD 12,390. Here is the run output and state/reconciliation.json. Find out why. |
| 4. Debug | see README (`shared/examples/customs-filing/README.md`) |
| 5. Audit | Audit shared/examples/customs-filing/ before we connect it to the broker's ABI software. Is it safe to ship? |
| 6. Verify, Run and Document | Verify shared/examples/customs-filing/ before go-live. Run the testers in parallel, fix what they find, and give me the verdict. |

## Example — fix an inherited support bot

You already have an agent, someone else built it, and you are not sure it is safe. This folder shows the four fix-what-you-have jobs on one such agent, end to end. "Tilly" is a WhatsApp bot for a fictional restaurant group (Tidewell Kitchen Group, three sites in Bristol; no real business, people or numbers). It answers order questions, takes bookings and can refund money. On 4 November it refunded one customer twice. You run **Audit** (is it safe?), **Raise** (score it and plan the work), **Debug** (why the double refund?) and **Implement** (build the first fixes), and finish with a working, tested version next to the old one.

Folder: `shared/examples/support-bot-raise/`

| Job | What you type |
|---|---|
| 1. Audit — "is this safe?" | We inherited this WhatsApp support bot from a contractor: `inherited/`. Is it safe to keep running? Audit it. |
| 2. Raise — "make it good, report only" | Raise it to lead standard using the schemas. Report only, don't touch the code. |
| 3. Debug — "why was this customer refunded twice?" | A customer was refunded twice on 4 November. Here is the run: `inherited/run-2026-11-04.log`. Find what went wrong and add a test. |
| 4. Implement — "build the first fixes" | The operations manager signed the blueprint in the upgrade plan. Implement this sprint's items 4 to 9 in `fixed/`, plain Python, stdlib only. |
| 5. Verify, Run and Document | Verify fixed/ before go-live. Run the testers in parallel, fix what they find, and give me the verdict. |

## Example — daily commitment digest

Copy this whole folder (`task-digest/`) into a customer project when the request is close to:

Folder: `shared/examples/task-digest/`

| Job | What you type |
|---|---|
| Request | Connect Slack, Gmail, and Jira. Give me task updates. |
| Scope | see README (`shared/examples/task-digest/README.md`) |
| Implement | see README (`shared/examples/task-digest/README.md`) |
| Debug | see README (`shared/examples/task-digest/README.md`) |
| Audit | see README (`shared/examples/task-digest/README.md`) |
| Raise | see README (`shared/examples/task-digest/README.md`) |


---

(c) 2026 DesignUp Strategy AZ LLC. All rights reserved. Agent Suite Builder is licensed per seat through Agensi; these pages may be read and linked to, not copied or republished.

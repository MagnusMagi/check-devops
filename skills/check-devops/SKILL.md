---
name: check-devops
description: Audits the current project against a 312-item backend, database, security, DevOps and distributed-systems checklist and writes an evidence-backed status report. Use when the user says "check-devops", types /check-devops, or asks for a production-readiness / DevOps / backend audit of the project.
---

# check-devops

Audit the current project against `checklist.md` (in this folder, 312 items). Report only: do not change code.

## Steps

1. **Project profile** (main context, keep it short): project root, language/framework, how it runs and deploys
   (repo CLAUDE.md, package.json/pyproject/go.mod/Cargo.toml, Dockerfile/compose, CI files, IaC).
   Server-side settings (reverse proxy, firewall, backups, uptime monitoring, fail2ban) may live outside the repo:
   if a machine-level note exists (e.g. `~/CLAUDE.md`), read it and count those items as "covered by infrastructure".
2. **Parallel scan**: launch 8 Explore agents with `model: "sonnet"` in a single message, ~39 items each:
   1–39, 40–78, 79–117, 118–156, 157–195, 196–234, 235–273, 274–312.
   Put in each agent's prompt:
   - the project root + the profile from step 1,
   - the text of its item range (from `checklist.md`),
   - "If the repo has graphify-out/graph.json, use `graphify query \"<question>\"` first and read raw files only at the lines the graph points to.",
   - return one line per item: `No | Status | Evidence (file:line or command output summary) | Short note`.
     Status: ✅ present/correct · ⚠️ partial/weak · ❌ missing but needed · ➖ not applicable (reason in 3–5 words).
   - Never give ✅ without evidence; if it cannot find something, write "⚠️ unverified", not ❌. Conceptual items
     (e.g. Reading Documentation, Command Line Basics) get ➖ "conceptual".
3. **Merge** the results and write the report.

## Report format (in the user's language)

1. Summary: ✅/⚠️/❌/➖ counts and a one-sentence overall verdict.
2. **Priority gaps**: only ❌ and important ⚠️ items, ordered by impact (security and data loss first),
   each with: item, why it matters, the smallest suggested fix, evidence.
3. Full table to `<project>/docs/check-devops.md` (dated heading; overwrite a previous one and summarize
   the diff against the previous result). Do not print the full table in the terminal; give the file path.
4. Before fixing anything, ask the user which items to work on.

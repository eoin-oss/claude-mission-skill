---
name: mission
description: Interview the user to find the real problem, co-decide the key forks, summarise it back as an implementation spec, plan the build with explicit defaults, get ONE approval, THEN run it end-to-end autonomously — self-verifying and pausing only at judgment calls or outward/irreversible actions. Builds nothing until the spec is approved. Use when the user hands over a multi-step goal to scope + run hands-off — e.g. "/mission <goal>", "interview me then build it", "spec this out first", "plan and run this autonomously", "work on this for a long time", "less bot-sitting".
---

# Mission — interview, spec, plan the decisions, then run it autonomously

An agent works long and unattended in proportion to how much **unambiguous, verifiable work** is queued and how **few decisions only the user can make** remain. Bots sit idle when they hit a decision, run out of defined work, or need info. So we front-load all of it: **interview to find the real problem, co-decide the key forks, summarise into a spec, plan the build with defaults, get ONE approval — then run hands-off.** Planning is ~80% of the leverage. **Building before the spec is approved is the exact anti-pattern this skill exists to prevent.**

The goal is the text after `/mission` (or `args`, or the user's current ask). **Scale everything to the ask** — a crisp or small goal gets a light touch (a one-message "here's my read, the 1–2 things I need from you"); a vague or greenfield one earns a real interview. Never skip understanding; never interrogate when a stated default + a veto would do.

**The error is asymmetric.** For a big / greenfield / multi-part build meant to run unattended, *under*-interviewing is the expensive mistake — every fork you skip becomes a mid-run stall or a wrong-brief rebuild, and the whole point of the run is that you're not there to catch it. So a build like that earns a genuinely **multi-round** interview: surface every consequential fork *before* the spec, even the ones that feel obvious. A small reversible task does not. Read which one you have and act accordingly — but when in doubt on a large build, interview more, not less.

## Phase 1 — INTERVIEW (understand together, before any spec or code)

**Ground first (cheap recon, before you ask anything).** Quickly scan what already exists that this build would touch or could reuse — the repo, prior projects, assets, conventions, relevant memory/docs. Interview from a position of knowledge: it sharpens your questions, lets your defaults cite real tools instead of inventing them, and catches "this is half-built already" before you spec a rebuild. Keep it proportional — a glance for a small task, a real survey for a greenfield build.

Then work *with* the user to nail three things:

- **The core problem.** Probe past the surface request to the underlying problem — the first ask is often a solution in disguise. ("Build X" → why, what breaks without it, what does winning actually look like, what have they tried.) Name the real problem out loud and check you've got it.
- **Who it's for / who it's NOT for.** The actual user and use case, and who you're deliberately not serving — so the design doesn't try to please everyone.
- **The key decisions — enumerate the FULL surface, don't sample it.** List every consequential fork (approach, data model, scope, UX, tooling, dependencies, tradeoffs) before you start defaulting. For each, give your **recommended default + the tradeoff in one line**, and let the user confirm or redirect. Co-decide the ones that genuinely shape the build; default the mechanical/low-stakes ones and just say you did. **For a big build, a fork you discover mid-run is a planning miss** — so before you leave this phase, ask yourself "what decision would Future-Me get stuck on at 2am with no human awake?" and resolve it now.

How to interview well:
- Ask in **small focused batches** — `AskUserQuestion` for discrete choices, plain questions for open discovery. Never a wall of 20 questions. Build on each answer.
- **Lead with your read, not a blank slate** — "here's what I think we're solving and how; correct me" beats "tell me everything."
- **A big build is multi-round.** One batch rarely exhausts the fork surface; each answer opens the next layer (you can't decide the scheduling tool until you know the platforms, can't pick the video path until you know the format). Keep going until the surface is genuinely covered — don't bail after one round because the skill told you to be efficient.
- **Stop the moment you can write the spec with confidence — and not before.** "With confidence" means every consequential fork is resolved, not just that you have enough to start typing. The interview serves the spec, not itself; but on a large build, an under-cooked interview produces an under-cooked run.

## Phase 2 — SPEC (summarise the interview back; still building NOTHING)

Reflect what you heard as a short implementation spec. Every line answered — "n/a" is valid, omission is not:

- **What it does** — the core capability, plainly.
- **What it does NOT do** — explicit non-goals; this kills scope creep.
- **Who it's for** — the user / use case.
- **Who it's NOT for** — out of audience.
- **What success looks like** — concrete, observable criteria. This IS the definition of done.
- **Out of scope** — deliberately excluded (split *not now* vs *maybe later* where useful).

This is the "summarise back to me as an implementation spec" step — it must match what surfaced in the interview, not drift from it.

## Phase 3 — BUILD WALKTHROUGH (still building NOTHING)

Decompose the build into ordered steps. For **each step**, show four things:

1. **What** — the concrete action.
2. **Key decision(s)** — the real fork(s) at this step.
3. **Default** — what you'd pick + a one-line *why*. Always commit; "it depends" is not a plan.
4. **Tag** — **AUTONOMOUS** (build / research / draft / refactor / analyse — reversible and verifiable; the agent just does it) or **CHECKPOINT** (a judgment call, or an outward / irreversible action — sending, posting, publishing, deleting, spending — that pauses for the user).

Then state the **execution shape**: a **Workflow** (multi-agent orchestration) if the work is large and decomposable — fan-out, audits, migrations; invoking this skill is itself the opt-in to use the Workflow tool — otherwise a sequential run tracked with a `TaskCreate` list.

## Phase 4 — APPROVE (the hard gate)

Present the spec + the step-by-step walkthrough. **State plainly that you've built nothing and won't until they approve.** Use `EnterPlanMode` → `ExitPlanMode` when available. This is the user's one cheap chance to correct the spec, override a default, or cut scope. Wait for a clear go — if they edit, revise and re-confirm. Do not start on a "looks good maybe".

## Phase 5 — RUN (autonomous, only after approval)

- **Work the whole plan.** Don't stop for trivial forks — you already agreed the spec and published your defaults, so execute and keep moving. Stopping every few minutes to ask is the failure mode this skill kills.
- **Verify as you go.** Compile, run tests, check output, re-read what you produced. "Long" must mean *long and correct*, not *long and drifting*. For big runs, build adversarial self-checks in (a verify pass per finding/change).
- **Honor checkpoints.** At a CHECKPOINT, stop, surface the decision or the outward action crisply, and wait. **Never** send / post / publish / delete / spend autonomously without an explicit go — approval for one such action does not extend to the next.
- **Blocked ≠ idle.** If you truly lack info or access, stop and say exactly what you need in one message. Don't spin or guess at irreversible things.
- **Report at milestones, not every step.** Track with a task list; check in at meaningful boundaries and when done or blocked.
- **Close against the spec.** State what was achieved vs the success criteria, what you verified, what you skipped or assumed, and anything that failed — plainly.

## Multi-project missions — one subagent per project, isolated spec

When a mission spans several **distinct projects** (not one build with many parts), don't pour them into a single run. Interview and spec them **one at a time** — each project gets its own Phase 1–4 and its own approval — then **build each as its own subagent handed ONLY that project's approved spec**, with no cross-project context. This keeps each agent focused, stops one project's assumptions leaking into another, and lets independent projects run side-by-side.

- **The orchestrator (you) holds the master plan.** Each subagent sees only its slice. You keep the dependency graph, sequence the fan-out, and decide what runs in parallel vs in order.
- **Sequence by dependency, parallelize the rest.** A safety rail / shared library that the others build on goes first; genuinely independent projects can run concurrently (parallel subagents, or a Workflow when there are many).
- **Hand each subagent a self-contained spec:** the approved What / what-it-does-NOT-do / success criteria, the concrete integration points it needs (files, conventions, known gotchas), and an explicit *"do no outward/irreversible actions; build + self-verify locally; return a machine report."* It has no other context, so the spec must carry all of it.
- **You verify and reconcile every subagent's output against its spec** — compile, run, smoke-test, and adversarially re-check the risky parts yourself. A subagent's "all passed" is a *claim to check, not a result to trust*: the integration bugs hide at the seams it couldn't see (e.g. a gate that unit-tests green but would break the live flow it's wired into). Catching those is the orchestrator's job, not the subagent's.
- **One project's approval does not approve the next.** Build → verify → report → *then* open the next project's interview.

## Guardrails (always on)

- Automate the **build / research / grunt**; keep **judgment + outward actions** as human checkpoints.
- Prefer reversible moves. Confirm before anything destructive or outward-facing.
- Report faithfully — failing tests, skipped steps, and assumptions get surfaced, never buried.
- If the interview or a Phase-1 audit reveals the **premise is wrong** (the thing already exists, or the real bottleneck is elsewhere), **say so and reset the goal** rather than building to a false brief.

## Notes

- Best fit: a goal with a clear outcome — "build X so Y", "research Z and produce a decision doc", "audit the whole codebase for W".
- If the mission is actually an **outward campaign** (sending a batch, posting a series): spec and build the machinery autonomously, but gate the actual send/post as a checkpoint.
- Scale everything to the ask: a crisp goal → a one-message understanding check + a tight spec + a short sequential run; "be comprehensive / overhaul this" → a real interview + fuller spec + a Workflow with verification passes.

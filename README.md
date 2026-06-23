# mission

A skill for [Claude Code](https://claude.com/claude-code). Hand Claude a big goal — it interviews you to find the real problem, decides the key forks *with* you, summarises it back as a spec, gets **one** approval, then runs the whole thing autonomously: verifying as it goes, and only stopping for real judgment calls or anything irreversible.

Most long agent runs stall or drift because the decisions weren't made up front. `mission` front-loads them. Planning is ~80% of the work; the run is the easy part.

## Install

Drop `SKILL.md` into a `mission` folder under your Claude skills directory:

```bash
mkdir -p ~/.claude/skills/mission
curl -fsSL https://raw.githubusercontent.com/eoin-oss/claude-mission-skill/main/SKILL.md \
  -o ~/.claude/skills/mission/SKILL.md
```

Or clone and copy:

```bash
git clone https://github.com/eoin-oss/claude-mission-skill.git
cp mission/SKILL.md ~/.claude/skills/mission/SKILL.md
```

## Use

```
/mission build X so that Y
```

Answer a few focused questions, approve the plan, and let it run. Scale is automatic — a small goal gets a light touch; a big greenfield build earns a real interview.

## How it works

1. **Interview** — find the real problem (not the surface ask), who it's for, and every consequential fork.
2. **Spec** — summarise it back: what it does, and explicitly what it does *not* do.
3. **Build walkthrough** — every step, its key decision, the chosen default, and whether it's autonomous or a checkpoint.
4. **Approve** — one gate. Nothing gets built until you say go.
5. **Run** — autonomous and self-verifying, pausing only at judgment calls or outward/irreversible actions.

## Why

An agent works long and unattended in proportion to how much unambiguous, verifiable work is queued — and how few decisions only you can make are left. Bots sit idle when they hit a decision, run out of defined work, or need info. `mission` removes all three before the run starts.

## License

MIT — free to use, adapt, and share.

---

Built by Eoin Wells — [@ewells___](https://www.instagram.com/ewells___/).

# founder-skills

Nadav's agent skills for founder work. Install into any agent (Claude Code, Codex, opencode):

```
npx skills@latest add NadavGrossman/founder-skills
```

## Skills

| Skill | Use when |
|---|---|
| `complaints-miner` | Need real user complaints (quoted, linked) about competitors or a problem space |
| `grill-me` | Have a plan/spec and want it interrogated until every branch is decided (vendored from mattpocock/skills, MIT) |

Customer-interview skills stay in their own public repo: [`NadavGrossman/the-mom-test-skills`](https://github.com/NadavGrossman/the-mom-test-skills) (`npx skills add NadavGrossman/the-mom-test-skills`). Not merged here so its public install path keeps working.

## Rules for this repo

- One folder per skill under `skills/`, each with a `SKILL.md`.
- Add a skill only when a trigger fires (see TRIGGERS.md). Don't pre-install.
- Vendored skills keep their upstream license file.

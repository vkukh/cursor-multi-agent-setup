# Cursor Multi-Agent Setup

> One prompt that turns your Cursor into a team of AI developers.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Cursor IDE](https://img.shields.io/badge/Cursor-IDE-black?logo=cursor)](https://cursor.com)
[![Building in Public](https://img.shields.io/badge/Building-in%20Public-blue)]()

## What is this?

Without a setup, AI writes code "somehow". With a setup — it knows your project better than a new developer after a month of onboarding.

This is a meta-prompt that analyzes your project and builds a working agent system, rules, and safety mechanisms — all in your `.cursor/` folder. One paste. One run. No invented conventions.

## What you get

After running the prompt once, your Cursor will have:

- **Documentation rules** — AI reads your project conventions before every action
- **AI helper team** — reviewer, QA, security auditor, debugger
- **Step-by-step playbooks** — how to build a feature, how to deploy
- **Safety net** — blocks `rm -rf`, asks before `git push`, denies destructive SQL
- **Slash commands** — `/review`, `/debug`, `/qa` for targeted tasks

All grounded in **your real codebase** — not generic advice. The prompt cites two real references from your repo for every file it creates.

## How to use (3 steps)

### Step 1: Copy the prompt
[**→ PROMPT.md**](./PROMPT.md)

### Step 2: Open Cursor in your project
Press `Cmd+L` (Mac) or `Ctrl+L` (Windows) — Agent Chat opens.

### Step 3: Paste the prompt and let AI work
The AI goes through 4 steps and stops at each one for your confirmation:

1. **Discovery** — analyzes your stack, conventions, business domain
2. **Gap Analysis** — tells you what's missing and what to merge
3. **Build** — creates files layer by layer (rules → agents → skills → commands → hooks)
4. **Smoke Test** — verifies hooks work, runs reviewer, lists what needs manual checking

Each step requires your explicit "yes" before the next one starts. Nothing is auto-applied.

## How to use it daily

Once the setup is in place, your daily workflow looks like this:

| Task | Command |
|---|---|
| New feature or bugfix | `/create-feature add login form` |
| Review what you wrote | `/review` |
| Debug a problem | `/debug semantic search returns wrong results` |
| Write tests | `/qa` |
| Security check | `/security-audit` |
| Research before code | `/research which payment provider to choose` |
| Deploy | `/deploy` |

Read the full daily workflow guide: [**→ PLAYBOOK.md**](./PLAYBOOK.md)

## Architecture: 5 layers

```
.cursor/
├── rules/          # Passive context: who we are, what we build, how we write code
├── agents/         # Active executors: reviewer, qa, security-auditor, debugger
├── skills/         # Step-by-step procedures: create-feature, deploy, research
├── commands/       # Slash commands as entry points: /review, /debug, /qa
└── hooks/          # Deterministic safety nets (Python, no AI)
    └── hooks.json
```

Each layer has a specific role:
- **Rules** — what AI knows (passive, read on demand)
- **Agents** — who does the work (active, delegated by main agent)
- **Skills** — how to do complex tasks (procedures with mandatory delegation)
- **Commands** — how you invoke things (slash commands)
- **Hooks** — what must always happen (automatic, no AI)

## What this prompt does NOT do (honest scope)

- ❌ Does not replace CI/CD — hooks only run inside Cursor sessions
- ❌ Does not replace project management — use Linear/Jira/GitHub Issues
- ❌ Does not auto-deploy — humans stay in control of merge and production
- ❌ Does not provide security sandboxing — hooks reduce mistakes, they're not a boundary
- ❌ Does not invent conventions — if the prompt can't cite your code, it stops and asks

The prompt is **explicit about its scope** in the Product-Context rule it generates. No overselling.

## Compatibility

- ✅ Cursor IDE (primary support)
- ✅ Any tech stack — TypeScript, JavaScript, Python, Go, Ruby, Rust, etc.
- ✅ Monorepos and single-package projects
- ✅ Clean projects and existing ones (merges, doesn't overwrite)
- ✅ Works with existing `.cursor/` files — extends instead of replacing

The prompt is **stack-agnostic by design**. It reads your code and adapts.

## Building in Public

This prompt was born from a real project — **JUNIVERSO**, a legal templates platform I'm building with my wife.

I'm documenting the entire process publicly:

- 📺 [YouTube channel](https://www.youtube.com/@KukharVitalii)
- 💼 [LinkedIn](https://www.linkedin.com/in/vitalii-kukhar/)
- 🐦 [Telegram](https://t.me/code_to_product)

If this prompt helps you ship something — let me know. Especially interested in cases where it didn't work as expected.

## Contributing

Issues and PRs welcome. Most valuable contributions:

- **Cases where the prompt failed** — open an issue with your stack and what went wrong
- **New stacks tested** — share your `.cursor/` output as an example
- **New agent roles** — propose roles that solve real pain you've encountered
- **Clarity improvements** — the prompt should be readable, not academic

See [CONTRIBUTING.md](./CONTRIBUTING.md) for the full guide.

## FAQ

**Q: Do I need to be a senior developer to use this?**
No. The prompt is designed for solo developers from junior to senior. It generates English content because Cursor agents work better with English in system prompts, but you can chat with the agents in any language.

**Q: Will it work on my existing project?**
Yes. The prompt explicitly handles existing `.cursor/` files — it merges instead of overwriting. If a rule already exists, it extends it. Nothing is destroyed.

**Q: How long does the setup take?**
10–15 minutes for an average project. The AI does all the work; you just confirm each step.

**Q: What if I don't like what it generated?**
You confirm every layer before it's created. If you don't like something — reject and ask for adjustments. Files are only written after your explicit approval.

**Q: Does it work with Claude Code or Windsurf?**
The architecture (rules + agents + hooks) translates conceptually, but file paths and hook events are Cursor-specific. A Claude Code adaptation is in the roadmap.

**Q: Why is everything in English even though the prompt is for Ukrainian/Russian/etc speakers?**
AI agents work measurably better with English system prompts. You'll still chat with them in your language — only the internal rule files are English.

## License

MIT — use it however you want, including commercial projects. No attribution required, but appreciated.

## Acknowledgments

Inspired by real-world pain of repeating the same prompts to AI 50 times a day. Built iteratively while working on [JUNIVERSO](https://juniverso.com).

---

**Star this repo if it helped.** It's the cheapest way to support an indie tool.

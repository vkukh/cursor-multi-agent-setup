# Contributing

Thanks for considering a contribution. This project is small and opinionated — read this first to save us both time.

## What I'm looking for

In priority order:

1. **Failure cases.** Open an issue with: your stack, your project size, what the prompt produced, where it went wrong. These are the most valuable contributions because they expose blind spots.
2. **New stack examples.** Run the prompt on a stack not yet covered (Rust, Elixir, Go, etc.), share the resulting `.cursor/` output as an example.
3. **Clarity improvements.** If a section of the prompt or playbook confused you, propose a rewrite. PRs that make the prompt shorter without losing meaning are gold.
4. **New agent roles.** Propose roles that solve real pain you encountered. Include: the pain, why existing agents don't cover it, the role's input/output contract.

## What I'm NOT looking for

- Generic improvements ("add more validation", "use better naming") without a concrete failure case.
- New layers (more than 5). The whole point of the architecture is minimalism.
- Framework-specific bias — the prompt should stay stack-agnostic.
- Decorative additions (more emojis, more sections, more headers).

## Before opening a PR

- Test the prompt change on at least two different projects (one Node, one Python is fine).
- Update README/PLAYBOOK if behavior changed.
- Keep PR descriptions short. Bullets, not essays.

## Issue templates

Use the templates in `.github/ISSUE_TEMPLATE/` — they exist to extract the information I'll ask for anyway.

## Code of conduct

Be direct, be kind, assume good faith. No corporate language.

## License

By contributing, you agree your contribution is released under MIT.

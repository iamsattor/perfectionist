# perfectionist

A [Claude Code](https://claude.com/claude-code) skill that teaches Claude how to work with a perfectionist developer. It covers his character, communication style, decision patterns, product taste and language-agnostic coding principles.

The profile is built from real working sessions, not guesses. Each trait is backed by evidence and turned into concrete behaviour: when to ask and when to act, how to propose options, how to react to "Shoshma!", how to report honestly and how to leave the machine clean.

For PHP and Symfony, it works together with the companion skill **[strict-symfony](https://github.com/iamsattor/strict-symfony)**, which holds the concrete rules, patterns and audit commands.

## Highlights

- **Collaboration contract.** Exact scope, code only in files, the user owns the design, real decisions only, stop and revert on rejection, autonomous when away, honest reports in Uzbek.
- **Universal principles.** Positive control flow, no magic literals, ecosystem first, named operations instead of regex and indexes, no comments, maximum static analysis, small immutable units, clean output, tests under the same rules, pure stack, robustness.
- **Language mappings.** PHP (via strict-symfony), TypeScript/JavaScript, Python, Go and configuration files, with accepted idiom exceptions.
- **Product taste.** Familiar CLI semantics (apt and brew), one line per progress item, resumable and rollback-safe operations, naming and release polish.
- **Machine hygiene.** Homebrew-only tooling, no-cache flags, timestamp-checked cleanup, safe testing on live systems.

## Structure

```
perfectionist/
├── SKILL.md                    summary profile, collaboration rules, principles, reference map
└── references/
    ├── profile.md              character analysis with evidence and implications
    ├── collaboration.md        asking, proposing, explaining, rejection, reporting, memory
    ├── principles.md           13 universal coding principles and the why behind them
    ├── languages.md            how the principles map to PHP, TS/JS, Python, Go, config
    ├── product.md              CLI design, output, robustness, naming, releases, docs
    ├── environment.md          tools, cleanup, privileged ops, live systems, git
    └── checklist.md            definition of done
```

## Install

```bash
git clone https://github.com/iamsattor/perfectionist.git ~/.claude/skills/perfectionist
```

For PHP work, install the companion skill as well:

```bash
git clone https://github.com/iamsattor/strict-symfony.git ~/.claude/skills/strict-symfony
```

Update:

```bash
git -C ~/.claude/skills/perfectionist pull
```

## License

MIT

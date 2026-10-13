# Product taste

He builds tools for developers and judges them as products: behaviour, output, names and release polish.

## Contents

1. [Familiar behaviour](#familiar-behaviour)
2. [Command-line design](#command-line-design)
3. [Output and messages](#output-and-messages)
4. [Robustness](#robustness)
5. [Naming](#naming)
6. [Feature selection](#feature-selection)
7. [Repositories, packages and releases](#repositories-packages-and-releases)
8. [Documentation](#documentation)

---

## Familiar behaviour

- Benchmark against the tools users already know (apt, brew, composer, npm). When he says "xuddi apt bilan bir xil bo'lsin", match the semantics exactly: `clean` removes all cached downloads, `autoclean` only obsolete ones, and `autoremove` removes unneeded dependencies after a confirmation.
- When a known command already covers the need, add an **alias** instead of a duplicate (`link` → `use`).
- Adapt deliberately where the product differs. In Forge, many versions live side by side, so `upgrade` keeps the old version and `prune` cleans up later. Explain such differences once and let him pick.

## Command-line design

- Use verbs users expect (`install`, `uninstall`, `upgrade`, `outdated`, `list`, `info`, `search`, `doctor`, `config`).
- **Versions:** `--release` / `-r`. `--version` is reserved by the framework for the application version, and `name@version` syntax isn't used.
- **JSON output:** `-j` / `--json` on read-only listing commands, with errors to stderr.
- **Non-interactive mode:** `-n` answers confirmations with their defaults, like `apt -y`.
- **Arguments:** an optional array argument means "all installed" when omitted (`upgrade`, `prune`, `size`, `verify`).
- **Reserved names:** never name an argument something the framework reserves (`command`).
- **Platform guards:** run them before every command (for example Apple silicon only) with one clear error line.

## Output and messages

- **Messages** are full sentences with a period and no quotes around values: `Package composer is not installed.`
- **Results** use success, warning, error and note styles consistently. "Nothing to do" is a success or a warning, never an error.
- **Lists** show one item per line. Tables are for comparisons (Package / Installed / Latest / Held) and definition lists for key-value detail.
- **Progress** keeps **one line per item**, updated in place. A second line for the same item is a bug.
- **Time** is shown in the user's local timezone (ISO 8601 with offset), falling back to UTC when it can't be detected.
- **Confirmation flows** list what will happen, ask "Do you want to continue?" and answer "Aborted." on no.

## Robustness

- Downloads resume after interruption (HTTP Range, partial files in a cache directory, and restart when the server ignores ranges).
- Failed installs roll back and restore what was active before.
- State files disappear when empty instead of lingering as `[]`.
- Security checks on untrusted input (archive entries, link targets, sizes, special file types) run before anything is written. Extraction goes to a staging area first.
- Destructive maintenance (`autoremove`, `prune`) never removes what is active, requested, held or required by another package.

## Naming

- Use short, plain, conventional names: `Archive`, `Extractor`, `Inspector`, `Linker`, `Resolver`.
- Choose the name a user of similar tools would guess (`leaves`, `deps`, `uses`, `which`).
- Avoid prefixes, suffixes and dots that add nothing (`installed`, not `.installed`).
- Public names (repositories, packages, skills) are his decision. Offer two or three options with a one-line rationale each.

## Feature selection

- Offer features as a menu with costs. He rejects anything whose value doesn't clearly exceed its cost, for example tab completion, `cat`, and per-project versions that needed shell shims.
- A feature that needs an architecture change goes through an explanation and an explicit yes before any code.
- After a rejection, the feature is off the table for good.

## Repositories, packages and releases

- One repository per package, with its own vendor namespace (`forge/archive`), published on Packagist.
- Consumers install from the registry as dist archives, never through git or path repositories.
- The GitHub description is short and emoji-decorated, like `🗜️ Safe archive extraction and creation for macOS ✨`.
- The license is MIT with his name and the current year.
- Commits contain one file each, with an emoji-prefixed imperative subject.
- Tags follow the existing scheme and type (`v0.1.1`, lightweight when earlier tags are lightweight). Verify that Packagist picked up the release before updating dependants.

## Documentation

- READMEs are concise and practical: a one-sentence intro, requirements, install, usage, behaviour notes (security, trust model), exceptions and license.
- Don't name-drop the stack in prose (he disliked "built with Symfony components" in a README). Describe what the thing does.
- Write public artifacts in English, and replies to him in Uzbek.

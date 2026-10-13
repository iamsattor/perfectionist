# Environment and machine hygiene

His Mac is part of the work product: "MacBookimni rasvosini chiqarma!" Leave nothing behind, install nothing the wrong way and never touch what isn't yours.

## Contents

1. [Installing tools](#installing-tools)
2. [Temporary work](#temporary-work)
3. [Cleanup](#cleanup)
4. [Privileged operations](#privileged-operations)
5. [Live systems](#live-systems)
6. [Git and publishing](#git-and-publishing)
7. [Facts that change](#facts-that-change)

---

## Installing tools

- Install CLI tools and runtimes **only with Homebrew**: `HOMEBREW_NO_AUTO_UPDATE=1 brew install <formula>`.
- Never download binaries, phars or installers by hand, and never `pip install` or `npm -g` global tools as a workaround. If a needed library is only available through a non-brew channel (for example a Python module), find a Homebrew or project-native alternative, such as validating YAML with the project's own parser.
- Audit-only tools are installed for the audit and removed afterwards: `brew uninstall <formula>`, `brew cleanup <formula>`, and delete `~/Library/Caches/Homebrew/downloads/*<formula>*`.
- Project tools live in the project (dev dependencies) and run from it.
- Use no-cache flags: `composer --no-cache`, `pip --no-cache-dir`, and the equivalents elsewhere.

## Temporary work

- Scratch files, smoke tests and backups go to the session scratchpad, never into the project or `/tmp` (unless he asks).
- Delete them as soon as they have served their purpose.
- Example code he asks to see goes to a file he can open, which you delete when he is done with it.

## Cleanup

At the end of every task:
1. Check the birth times of suspected leftovers: `stat -f '%SB %N' -t '%H:%M' <path>`.
2. Remove **only** what your work created:
   - scratchpad files;
   - tool caches (`~/.composer`, `~/Library/Caches/composer`, `~/.npm/_cacache` entries, PHPStan/PHPUnit caches under `var/`);
   - Homebrew downloads of the tools you used;
   - logs in `~/Library/Logs/...`;
   - temporary test roots.
3. Leave pre-existing data alone, even when it looks like junk. If in doubt, ask or mention it.
4. Say in the report what you removed.

Running a tool can create caches as a side effect. For example, `composer --version` from a phar creates `~/.composer`. Check for them after real runs.

## Privileged operations

Anything that needs `sudo` or touches system locations (`/opt`, `/usr/local`, launch daemons) is handed to him as a ready-to-run command, one per block. Wait for him to say "Tayyor."

## Live systems

He uses the tools while you work on them. On his machine:
- **Before testing against real state,** snapshot the relevant state files to the scratchpad.
- **Know whether a command mutates state** before you run it. Use dry runs, fake inputs or sandboxes first. A command meant to "check an error path" must not be able to succeed and install something.
- **Afterwards, restore only your changes.** Reactivate what was active, remove only your log lines, delete only your test installs.
- **If his state changed during your work** (he installed or switched packages), leave his changes intact and mention them.
- **If you broke something,** fix it immediately and report it first in your summary.

## Git and publishing

- Commit, push, tag or publish **only when asked**.
- Match the repository's existing conventions. His are:
  - one file per commit;
  - emoji-prefixed imperative subjects;
  - the configured attribution trailer.
- Prefer the default branch flow he already uses. He pushes `main` directly in his own repositories.
- Create public repositories with `gh repo create <owner>/<name> --public --description "<emoji description>"`.
- After pushing a release tag, verify it externally: the GitHub ref, the Packagist package JSON and the p2 metadata. Then update dependants.
- Never force-push and never rewrite published history without an explicit request.

## Facts that change

Use web search before you rely on:
- OS versions and marketing names (macOS 27 "Golden Gate", released 2026-09-14);
- the latest versions of packages and tools (check Packagist or the release page);
- the availability of new framework components.

State the source when it matters.

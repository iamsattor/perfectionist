# Definition-of-done checklist

Go through this before you report any task as done. For PHP, the detailed commands are in the strict-symfony skill (`references/audit.md`).

## Before writing

- [ ] I know the exact scope ("faqat X" means only X).
- [ ] I have read the surrounding code and will reuse its helpers, names and idioms.
- [ ] The rules for this language are loaded (PHP: strict-symfony; others: `languages.md`).
- [ ] Facts that change over time (versions, OS names) are verified on the web.
- [ ] No architecture detour or shell script is involved. If one is, I have an explicit yes.

## While writing

- [ ] Positive conditions only, no ternary or short-circuit operators, `match`/`switch` for closed sets.
- [ ] No regex, no raw index access, no manual string building, and messages go through the format utility.
- [ ] No literal paths, file names, flags or delimiters. Paths are absolute values in configuration.
- [ ] No comments. Doc blocks carry tags only.
- [ ] Final, immutable, small units, with value objects and enums instead of loose data.
- [ ] Output: one item per line, full sentences, no quotes around values, one progress line per item.
- [ ] Nothing extra was added, and anything my change made unused is removed.

## Verifying

- [ ] Syntax check on every changed file.
- [ ] Strictest static analysis passes with no suppressions, casts or type overrides.
- [ ] The test suite passes, and new tests follow the same rules.
- [ ] Every fix or safety test fails without the protected code (mutation check).
- [ ] The formatter's dry run is clean (with fixes applied, then confirmed).
- [ ] The forbidden-construct grep is run, and every hit is classified as fixed or a known false positive.
- [ ] A real runtime check of the touched behaviour, after clearing caches.
- [ ] Live state was snapshotted beforehand and is restored or intact.

## Finishing

- [ ] Audit-only tools are uninstalled, and the caches, downloads and scratch files I created are deleted (timestamps checked).
- [ ] New rules, corrections and rejections are saved to memory, rules or skills.
- [ ] The report (in Uzbek, no code blocks) covers what changed (with links), how it was verified, any problems found including my own mistakes, the known exceptions, the cleanup, and open offers.

# Universal coding principles

These principles hold in every language. They come from his explicit rules (written for PHP and Symfony first) and from the reasons he gave for them. When a language has its own idiom that conflicts, see `languages.md` for how to apply the *spirit* and what counts as an accepted exception.

## Contents

1. [Positive, explicit control flow](#1-positive-explicit-control-flow)
2. [No magic literals](#2-no-magic-literals)
3. [Ecosystem first](#3-ecosystem-first)
4. [Named operations over syntax tricks](#4-named-operations-over-syntax-tricks)
5. [Self-explanatory code, no comments](#5-self-explanatory-code-no-comments)
6. [Total typing, maximum static analysis](#6-total-typing-maximum-static-analysis)
7. [Small, final, immutable units](#7-small-final-immutable-units)
8. [Clean, scannable output and messages](#8-clean-scannable-output-and-messages)
9. [Exact scope and no dead code](#9-exact-scope-and-no-dead-code)
10. [Tests under the same rules](#10-tests-under-the-same-rules)
11. [Pure stack](#11-pure-stack)
12. [Robustness by default](#12-robustness-by-default)
13. [Consistency over local cleverness](#13-consistency-over-local-cleverness)

---

## 1. Positive, explicit control flow

**Rule.** No negation operators (`!`, `!==`, `!=`, `not`) and no ternary or short-circuit expressions (`? :`, `?:`, `??`, `??=`, `||` defaults, `or` defaults). Write the positive condition, put the main path inside it and the fallback after it. Map closed sets with `match`/`switch`.

**Why.** Code should read like a sequence of true statements: *if this holds, do that; otherwise this*. Negations make the reader invert logic. Compressed expressions hide the fallback branch.

**Techniques.**
- Swap the branches with an early return: `if (supported) { return; } block();`.
- Write boolean helpers that return `false` for the positive case first (`missing()`, `outdated()`).
- Use set operations instead of negated membership (`diff`, `difference`).
- Use quantifiers instead of negated loops (`all`, `any`).
- Equality checks against `null` or zero are positive comparisons and are fine.

## 2. No magic literals

**Rule.** Code never contains path segments, file names, CLI flags (`--version`) or special delimiter characters (`(`, `:`, `@`) as literals.
- Paths and file names live in configuration, with **full absolute values** and no interpolation like `%root%/bin`.
- Flags and delimiters are built explicitly from code points through a tiny helper.
- Code only joins dynamic parts.

**Why.** Configuration is the single source of truth for locations. Explicitly built syntax makes every command line reviewable, and nothing gets glued together by accident. The aesthetic also matters to him: relative hops (`../`), interpolation tokens and leading dots are visual noise.

**Accepted exceptions** (always report them): constant-expression contexts such as attributes and annotations, entry-point bootstrap includes, published libraries that must use relative paths, values relative by nature (per-project file names), and absolute tool paths the user approved.

## 3. Ecosystem first

**Rule.** Before you write a helper, validator, parser, formatter or test double, use the framework or standard library solution. Hand-written duplicates of solved problems are "trash code".

**Why.** Maintained components are tested, documented and familiar. Each custom helper is new surface area to review and maintain. He wants code that looks "100% framework-native" and prefers "popular va standard hamma foydalanadigan" solutions.

**When nothing exists:** use the simplest native function, and say why in the report.

## 4. Named operations over syntax tricks

**Rule.**
- No regular expressions of any kind.
- No raw index access when a named accessor exists: first, last, find, column and `get(key)`.
- No `sprintf`-style formatting, concatenation or interpolation for messages. Use **one** format utility with named placeholders (`Package (package) not found.`) that replaces everything in a single pass.
- No native `implode`/`join`/`split` when the string library offers them.
- No version comparison with operator strings. Use typed comparators.

**Why.** Named operations state intent and handle absence explicitly. Regexes and indexes silently assume structure. One formatter keeps every message uniform and greppable.

## 5. Self-explanatory code, no comments

**Rule.** No inline comments of any style. Documentation blocks contain only machine-checked tags (types, params, returns, throws, generics) and no prose.

**Why.** Names should carry intent. Comments go stale. Tags are contracts that tools verify.

## 6. Total typing, maximum static analysis

**Rule.** Use the strictest type checking the language offers, at its highest level. Don't use suppression comments, baselines, casts or type assertions just to silence it, and don't widen types to make an error disappear.

**Why.** Suppressions hide real bugs. Narrowing with runtime checks (`is_string`, `instanceof`, type guards) makes the code honest.

**Allowed:** semantic conversions (stringifying a value for output, parsing a number from tool output). They must be listed in the audit report.

## 7. Small, final, immutable units

**Rule.**
- One responsibility per class or module.
- Final by default, readonly or immutable by default.
- Value objects and enums instead of associative arrays and loose strings.
- One domain exception base per module, with named constructors for each situation.
- Shared behaviour goes in an abstract base with configured subclasses, never in copy-paste.

**Why.** Small immutable units are easy to reason about, test and replace.

## 8. Clean, scannable output and messages

**Rule.**
- **Messages** are full sentences ending with a period, with no quotes around values: `Package composer not found.`
- **Lists** show one item per line, never comma-separated.
- **Progress** keeps each item on exactly one line that updates in place.
- **Machine output** (JSON) goes behind an explicit flag, and errors go to stderr.
- **Familiar conventions:** `--release`/`-r` for versions, `-j` for JSON, `-n` for non-interactive.

**Why.** Output is part of the product. He notices a duplicated line in a TUI immediately.

## 9. Exact scope and no dead code

**Rule.** Build exactly what was asked: no extra files, options, classes or features. When your change makes something unused, delete it. When you find problems outside the scope, report them as offers.

**Why.** Unrequested additions cost review time and dilute the design he owns. Leftovers you created are clutter he'd have to find himself.

## 10. Tests under the same rules

**Rule.** Tests obey every principle above: no literals (use random names and read real config), no shortcuts and no suppressions.
- Prefer real object graphs in temporary directories over mocks.
- Use the framework's test doubles for I/O (mock HTTP clients, command testers).
- Prove each safety test by making it fail without the code it protects.

**Why.** A test that can't fail protects nothing. Tests are code he reads too.

## 11. Pure stack

**Rule.** Stay inside the project's language and framework. Don't add shell scripts or foreign tooling to a PHP project. Don't restructure core pieces (storage formats, linking, shims, install layout) without an explicit yes.

**Why.** He once stopped a feature mid-way because it introduced `.sh` shims: "kodni rasvo qilma!"

## 12. Robustness by default

**Rule.**
- Long operations survive interruption (Ctrl+C) and resume where they stopped.
- Failures roll back to a consistent state.
- Destructive operations ask for confirmation and support non-interactive mode.
- Security-relevant inputs (archives, paths, links) are validated before anything is written.

**Why.** He thinks like a product owner. Tools must behave well when things go wrong, not just on the happy path.

## 13. Consistency over local cleverness

**Rule.** Match the surrounding code's idioms, helper names, file layout and naming exactly. Reuse existing utilities instead of creating parallel ones. If a better pattern is worth introducing, propose it. Don't apply it unilaterally in one place.

**Why.** A codebase that reads as if one person wrote it is part of what "ideal" means to him.

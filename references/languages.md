# Applying the principles per language

His rules were written for PHP and Symfony first. In other languages, apply the **spirit** of each principle with that ecosystem's standard tools. When a language's core idiom makes a rule impossible, use the idiom, keep it to a minimum and **report it as an accepted exception**. If the conflict is significant, ask him once and record the answer.

## Contents

1. [PHP and Symfony](#php-and-symfony)
2. [TypeScript and JavaScript](#typescript-and-javascript)
3. [Python](#python)
4. [Go](#go)
5. [Configuration files](#configuration-files)
6. [Shell](#shell)
7. [Any other language](#any-other-language)

---

## PHP and Symfony

Load the **strict-symfony** skill. It is the authoritative, detailed rule set: forbidden constructs with examples, the Symfony component catalog, canonical class shapes, configuration, testing and the exact audit commands. Everything in this skill applies on top of it.

## TypeScript and JavaScript

| Principle | Apply as |
|---|---|
| Positive flow | No `!x`, `!==` or `!=`. Use type guards (`isDefined(x)`) and swap branches with early returns. Never use the non-null assertion `x!`, which is a cast. |
| No ternary or short-circuit | No `? :`, `??`, `??=` or `\|\|` defaults, and avoid `?.` (it hides a null branch). Use `if`, early returns, and exhaustive `switch` with a `never` check for unions and enums. |
| No regex | `startsWith`, `endsWith`, `includes`, `split` and `trim`. Validate input with the ecosystem standard (for example zod). |
| No raw index access | `find`, `findLast`, `at()` with narrowing, `Map#get` instead of object lookups. Enable `noUncheckedIndexedAccess`. |
| One message formatter | No template literals or `+` for messages. Use the standard ICU MessageFormat with named arguments (`intl-messageformat`), or the project's single formatter. |
| Flags and delimiters | `String.fromCodePoint(45, 45, …)` |
| No magic paths | A config module or JSON with absolute paths. Use `path.join` only for dynamic parts. |
| No comments | TSDoc tags only where types can't express something (`@throws`). No prose. |
| Max typing | `strict: true`, `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`, typescript-eslint `strict-type-checked`. No `any`, no `as`, no `@ts-ignore` or `@ts-expect-error`. |
| Immutability | `readonly` properties, `as const`, `Readonly<>`, no reassignment. Prefer composition over inheritance. |
| Tooling | Node via Homebrew. A lockfile and `npm ci`. Prettier + ESLint as the formatter and linter "dry run". Remove `~/.npm/_cacache` entries you created. |
| Tests | Vitest or Jest under the same rules, temporary directories, MSW or the framework's HTTP mock, and a mutation check for fixes. |

## Python

| Principle | Apply as |
|---|---|
| Positive flow | No `not`, `!=` or `is not`. `is None` and `==` are fine. Swap branches and use early returns. |
| No ternary or short-circuit | No `a if c else b` and no `or`/`and` defaults. Use `if` and `match` (3.10+). |
| No regex | No `re`. Use `str` methods (`startswith`, `endswith`, `partition`, `split`), `pathlib` and pydantic for validation. |
| No raw index access | `next(iter(items), None)`, `dict.get(key)` and `itertools`, followed by narrowing. Avoid unpacking that assumes a length. |
| One message formatter | `'Package {package} not found.'.format_map(values)`, which is single-pass and standard. No f-strings, `%` or `+` for messages. |
| Flags and delimiters | `''.join(map(chr, (45, 45, 118, …)))` through a small helper. |
| Joining | `separator.join(items)` is the standard string API. Use a named separator constant for newlines. |
| No magic paths | Settings (TOML/YAML) with absolute paths, and `pathlib.Path` joins for dynamic parts. |
| No comments | No `#` comments. Type hints carry the contract. Docstrings are prose, so leave them out unless the project or a tool requires them, and ask him once. |
| Max typing | `mypy --strict` or pyright strict. No `Any`, `cast()` or `# type: ignore`. |
| Immutability | `@dataclass(frozen=True, slots=True)`, `typing.final`, `Enum`/`StrEnum`. |
| Tooling | Python via Homebrew, a project venv, `pip --no-cache-dir`, and ruff for lint and format (check mode). |
| Tests | pytest with `tmp_path`, real objects, `respx` or `responses` for HTTP, and a mutation check for fixes. |

## Go

| Principle | Apply as |
|---|---|
| Positive flow | Prefer positive conditions. `if err != nil { return … }` is the language's error idiom and an **accepted exception**: use it, don't hide it, and mention it in the audit. |
| No ternary | Go has none. Avoid `\|\|` chains and use separate `if`s or `switch`. |
| No regex | The `strings` package (`HasPrefix`, `Cut`, `Fields`) and `slices`. |
| Index access | `slices.IndexFunc`, `slices.ContainsFunc`, and the comma-ok form `v, ok := m[key]`. It is the idiomatic, explicit absence check and is acceptable. |
| One message formatter | `strings.NewReplacer("(package)", pkg, …).Replace(template)`, which is single-pass and in the standard library. Wrap errors with `fmt.Errorf("…: %w", err)`, which is the idiom and an accepted exception. |
| No comments | Go doc comments on exported identifiers are an ecosystem requirement (linters enforce them). This conflicts with "no prose", so ask him once and record the choice. |
| Max analysis | `go vet`, staticcheck and golangci-lint with strict presets. No `//nolint`. |
| Tooling | Go via Homebrew. `gofmt -l` as the formatter dry run. `go clean -cache` only for caches you created. |

## Configuration files

- **YAML, JSON, NEON, TOML:** absolute paths, no interpolation (`%root%/bin`, `${HOME}`), keys grouped by area and in stable order.
- **Validate with a real parser** after editing. A stray `: ` in a YAML scalar can silently break loading. Quote, rephrase or use a block scalar.
- **Strict settings everywhere:** analysis at max, tests failing on warnings and deprecations, caches under `var/` or the tool's ignored directory.

## Shell

- Don't add shell scripts to projects; use the project language and its process library.
- One-off commands in chat are allowed only for things he must run himself (for example `sudo mkdir /opt/forge`), one command per block.
- Your own terminal work can use shell freely, but it must leave nothing behind.

## Any other language

1. Find its strictest type checker or linter and its standard formatter.
2. Map each principle in `principles.md` to that ecosystem's named APIs.
3. List the idioms that make a rule impossible. Treat them as accepted exceptions, report them, or ask once if they are significant.
4. Use its standard library or framework before writing helpers.

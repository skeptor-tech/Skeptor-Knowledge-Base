# GitHub Code Search Cheat Sheet

## Query Basics

- Bare terms match file content and paths; use spaces to require multiple terms
  (implicit `AND`).
- Wrap phrases in quotes for exact matches, including whitespace:
  `"sparse index"`.
- Escape double quotes (`\"`) and backslashes (`\\`) when they appear in the
  literal text.
- Space-separate every query component (terms, qualifiers, regexes,
  parentheses, boolean keywords) so the parser interprets them correctly.

## Boolean Logic

- `term1 term2` ≙ `term1 AND term2`.
- `OR` finds files matching either operand: `sparse OR index`.
- `NOT` excludes matches: `"fatal error" NOT path:__testing__`.
- Parentheses group complex expressions:
  `(language:ruby OR language:python) AND NOT path:"/tests/"`.

## Key Qualifiers

- `repo:`
  - Purpose: Restrict to specific repositories.
  - Example: `repo:github-linguist/linguist`
  - Notes: Use full `owner/name`; combine with `OR` for multiple repos.
- `org:` / `user:`
  - Purpose: Limit to a specific organization or user.
  - Example: `org:github`
  - Notes: Requires the full org or user name; no partial matches.
- `language:`
  - Purpose: Filter by language (supports `OR`).
  - Example: `language:ruby OR language:cpp`
  - Notes: Language names follow Linguist's `languages.yml`.
- `path:`
  - Purpose: Match terms in file paths.
  - Example: `path:unit_tests`
  - Notes: Supports regex (`/.../`), glob (`*`, `?`, `**`); quote literal matches
    such as `path:"file?"`.
- `symbol:`
  - Purpose: Find symbol definitions.
  - Example: `language:go symbol:WithContext`
  - Notes: Accepts regex; focuses on definitions via Tree-sitter support.
- `content:`
  - Purpose: Match file content only.
  - Example: `content:README.md`
  - Notes: Ignores path matches.
- `is:`
  - Purpose: Filter repository properties.
  - Example: `path:/^LICENSE$/ is:vendored`
  - Notes: Values include `archived`, `fork`, `vendored`, `generated`; combine
    with `NOT` to exclude.

### Path Glob Tips

- Globs are unanchored unless prefixed with `/`: `path:/src/*.js`.
- `*` skips any run of characters except `/`; use `**` to cross directories
  (`path:/src/**/*.js`).
- Use `?` for single-character wildcards (`path:*.a?c`).

## Regular Expressions

- Wrap regexes in slashes: `/sparse.*index/`.
- Escape literal slashes: `/^App\/src\//`.
- Regex escapes like `\n`, `\t`, `\x{HHHH}` are supported.
- Most regex features work, but look-around assertions are not supported.

## Case Sensitivity

- Searches are case-insensitive by default.
- Force case-sensitive matching via regex flags, e.g. `/(?-i)True/` to require
  uppercase `True`.

## Quick Reminders

- Combine quoted strings with qualifiers when needed:
  `path:git language:"protocol buffers"`.
- Regular expressions and quoted strings can appear in most qualifiers.
- If parsing fails, code search treats the fragment as a literal; adjust
  spacing or quoting to clarify intent.

## gh utility

The `gh` utility is a command-line tool allowing you to search on GitHub.

Example usage:

```bash
gh search code "knowledge base filename:AGENTS.md" --limit 20 --json repository,path,url,textMatches
```

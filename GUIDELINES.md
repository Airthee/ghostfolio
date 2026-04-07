# Project Guidelines

## Code Style

### Alphabetical ordering

Always respect alphabetical order for:

- Object construction: `{ alpha, beta, gamma }` not `{ gamma, alpha, beta }`
- Destructuring: `const { a, b, c } = obj` not `const { c, a, b } = obj`
- Function parameters (when objects): `function foo({ a, b, c })` not `function foo({ c, a, b })`
- Imports: keep import specifiers and import statements in alphabetical order
- Constructor parameters: injected dependencies must be in alphabetical order

This applies to object literals, imports destructuring, and any key-value structure.

### Constructor-injected dependencies must be `private readonly`

Use `private readonly` for all constructor-injected dependencies:

```ts
// Good
constructor(private readonly destroyRef: DestroyRef) {}

// Bad
constructor(private destroyRef: DestroyRef) {}
```

### Use `isNumber()` instead of truthy checks for numeric values

When guarding numeric parameters that could legitimately be `0`, use `isNumber()` instead of a truthy check:

```ts
// Good
if (isNumber(skip)) {
  params.append('skip', skip.toString());
}

// Bad — fails when skip is 0
if (skip) {
  params.append('skip', skip.toString());
}
```

### Prefer destructuring in subscribe/callback parameters

When subscribing to observables or using callbacks with typed objects, destructure the parameters directly in the signature:

```ts
// Good
.subscribe(({ dataSource, symbol }: MyParams) => { ... })

// Bad
.subscribe((params: MyParams) => { const { dataSource, symbol } = params; ... })
```

## Project Structure

### Interfaces go in dedicated files

Interfaces should be placed in a dedicated `interfaces/interfaces.ts` file within the relevant module directory, not inline in component files.

## Translations (i18n)

### Mark translated strings with `state="translated"`

In `.xlf` translation files, when a string has been translated, change `state="new"` to `state="translated"` on the `<target>` element.

## Git Conventions

### Commit message format

Commit messages follow the pattern: `Type/description (#PR)`

**Types:**

- `Feature/` — new functionality (e.g. `Feature/add copy-to-clipboard button to user detail dialog`)
- `Task/` — maintenance, refactoring, upgrades, improvements (e.g. `Task/upgrade stripe to version 20.4.1`)
- `Bugfix/` — bug fixes (e.g. `Bugfix/fix Storybook story of Symbol Autocomplete component`)
- `Release` — release commits, format: `Release X.Y.Z`

**Rules:**

- Start the description with a **lowercase verb** (add, eliminate, improve, upgrade, fix, implement, refactor, remove, update, enable, support...)
- No period at the end
- PR number is appended automatically by GitHub on merge — do not add it manually
- Do not add a `Co-Authored-By` line

### Branch naming

Use the format: `type/short-description-with-hyphens`

Examples: `feature/add-type-filter-on-activities`, `task/upgrade-stripe`, `bugfix/fix-redis-race-condition`

## PR Discipline

### Keep changes focused

Do not reorder methods, change unrelated APIs, or refactor code outside the scope of the PR. Each PR should only contain changes relevant to its stated purpose.

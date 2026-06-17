# @kopandante/biome-config

Shared Biome configuration for **all Next.js projects**. One source of truth for the house
lint/format standard plus enforcement of the project conventions:

- **`noAlert: error`** — no `alert`/`confirm`/`prompt`; use a shadcn Dialog / Sonner toast.
- **`noRestrictedElements: warn`** — no native `<button>/<input>/<select>/<textarea>/<dialog>`;
  use the shadcn primitives. (`components/ui/**` is exempted — that is where the primitives live.)
- House base: tab indent, double quotes, `recommended` + curated a11y/perf/suspicious warns,
  CSS ignored by the formatter.

> **Hardcoded colors** are intentionally **not** a Biome rule: Biome 2.4 has no color rule and its
> GritQL plugin layer does not emit diagnostics for string-content matches. Colors are enforced
> globally by the `content-guard` hook in `~/.claude` (covers every editor write, every repo).
> This package is the **linter** layer; the hook is the **global** layer.

## Adopt in an existing project

```sh
bun add -D github:kopandante/biome-config   # or the npm name once published
```

Then make the project's `biome.json` extend it (local keys override the shared ones):

```jsonc
{
	"$schema": "https://biomejs.dev/schemas/2.4.2/schema.json",
	"extends": ["@kopandante/biome-config/biome.json"],
	// project-specific overrides go here
}
```

`extends` merges least-relevant-first, so anything the project sets wins over the shared base.

## New projects

Every new Next.js project must start with the two lines above (`bun add -D` + `extends`).
Do not copy the rules inline — extend, so the standard stays in one place and cannot drift.

## Versioning

Bump `version` on any rule change; consumers pick it up on `bun update`. A rule that would turn
existing code red ships at `warn` first (migration), then is promoted to `error` once the fleet is clean.

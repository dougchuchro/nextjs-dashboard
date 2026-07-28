## Next.js App Router Course - Starter

This is the starter template for the Next.js App Router Course. It contains the starting code for the dashboard application.

For more information, see the [course curriculum](https://nextjs.org/learn) on the Next.js Website.

## Local environment setup

Notes on getting this project running on this machine (pnpm 11, Node 24).

### Package manager

This project uses **pnpm**. Install dependencies with:

```bash
pnpm install
```

### Allowing native build scripts (bcrypt & sharp)

pnpm 10+ blocks package build scripts by default. If you see
`ERR_PNPM_IGNORED_BUILDS: bcrypt, sharp` (it can even block `pnpm dev`, which runs a
dependency check first), approve the build scripts once:

```bash
pnpm approve-builds --all
```

This runs the scripts and records the approval in `pnpm-workspace.yaml`:

```yaml
allowBuilds:
  bcrypt: true
  sharp: true
onlyBuiltDependencies:
  - bcrypt
  - sharp
```

- **sharp** — used by Next.js for image optimization. Ships prebuilt platform binaries.
- **bcrypt** — used for password hashing in the authentication chapter. On Node 24 it
  fetches a prebuilt N-API (`napi-v3`) binary, which is Node-version-agnostic.

Note: the allow-list moved out of `package.json` (the old `pnpm.onlyBuiltDependencies`
field is no longer read) into `pnpm-workspace.yaml`.

### Running the app

```bash
pnpm dev
```

Then open [http://localhost:3000](http://localhost:3000).

## Gotchas

### Stale type errors pointing at `.next/`

Next.js generates route types into `.next/dev/types/`, and `tsconfig.json` includes that
directory. Those generated files can fall out of sync with the source — typically after
adding a new route segment or layout while the dev server is running. `tsc` then reports
errors in generated code that look alarming but have nothing to do with your source:

```
.next/dev/types/validator.ts(25,44): error TS2344:
  Type '"/dashboard"' is not assignable to type '"/"'
```

Here the generated types still described a world where `/` was the only route with a
layout, because they predated `app/dashboard/layout.tsx`.

**Fix:** regenerate them — run `pnpm build`, or restart the dev server. If that clears the
error with no source changes, it was stale, not a real bug.

**Rule of thumb:** a type error whose file path starts with `.next/` is almost never your
code. Regenerate before investigating.

### `@tailwind` flagged as "Unknown at rule" in VS Code

Editor-only warning from VS Code's built-in CSS validator, which doesn't know Tailwind's
at-rules. The build is unaffected. Installing the Tailwind CSS IntelliSense extension does
*not* silence it, since that validator runs independently. To hide it, add to
`.vscode/settings.json`:

```json
{
  "css.lint.unknownAtRules": "ignore"
}
```

### `baseUrl` in `tsconfig.json`

Commented out deliberately. It's unnecessary as of TypeScript 4.1 — the `"@/*"` mapping in
`paths` resolves relative to `tsconfig.json` without it — and it gave project-root folders
resolution priority over `node_modules`, letting a local directory silently shadow an npm
package of the same name.

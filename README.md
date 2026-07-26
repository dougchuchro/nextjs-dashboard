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

pnpm 10+ blocks package build scripts by default, and moved the allow-list out of
`package.json` into `pnpm-workspace.yaml`. This project's `pnpm-workspace.yaml` lists the
packages whose build scripts are permitted:

```yaml
onlyBuiltDependencies:
  - bcrypt
  - sharp
```

- **sharp** — used by Next.js for image optimization. Ships prebuilt platform binaries.
- **bcrypt** — used for password hashing in the authentication chapter.

### bcrypt native binary on Node 24

`bcrypt@5.1.1` may not build automatically via `pnpm rebuild`. If `require('bcrypt')` fails
with a missing `bcrypt_lib.node`, fetch the prebuilt N-API binary directly:

```bash
cd node_modules/.pnpm/bcrypt@5.1.1/node_modules/bcrypt && npx node-pre-gyp install --fallback-to-build && cd -
```

The `napi-v3` prebuilt binary is Node-version-agnostic, so this works on Node 24.

### Running the app

```bash
pnpm dev
```

Then open [http://localhost:3000](http://localhost:3000).

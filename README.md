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

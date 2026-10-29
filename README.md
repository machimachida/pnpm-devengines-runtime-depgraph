# pnpm devEngines.runtime vs. GitHub dependency graph

Minimal reproduction: GitHub's dependency graph does not pick up `pnpm-lock.yaml`
when the lockfile contains a `node@runtime:<version>` entry, which pnpm writes
when `package.json` sets `devEngines.runtime`.

| Directory          | `devEngines.runtime` | Lockfile                                  |
| ------------------ | -------------------- | ----------------------------------------- |
| `with-runtime/`    | yes                  | single YAML document, has `node@runtime`  |
| `without-runtime/` | no                   | single YAML document, no `node@runtime`   |

Both lockfiles were written by `pnpm install --lockfile-only` with pnpm 11.11.0.
The only dependency is `debug` (which depends on `ms`).

Check which manifests the dependency graph recognises:

```sh
gh api graphql -H "Accept: application/vnd.github.hawkgirl-preview+json" \
  -f query='{repository(owner:"OWNER",name:"REPO"){dependencyGraphManifests(first:20){nodes{filename blobPath}}}}'
```

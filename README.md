# Nexus package index — staging

The **staging** index of Nexus-Lang packages: where the official index's automation,
its mirror and the tools (`nxpkg`, `nxapp`) are tried end to end before the official
index opens. **It may be reset at any time**: do not depend on it.

It is read over HTTPS, file by file, at <https://index-staging.nx-lang.org/>:

```sh
nxapp info  zlog --index https://index-staging.nx-lang.org/
nxapp install zlog --index https://index-staging.nx-lang.org/
```

or, in `~/.nexus/config.toml`:

```toml
[index.staging]
url  = "https://github.com/nxlang/index-staging.git"
http = "https://index-staging.nx-lang.org/"
require-signature = true
```

## Layout

- `index.toml`: the index's name and rules (`require-signature`: every built package is
  signed).
- one file per package, `<name>.toml`: its owners, its signing keys, its versions (each
  with its public git source, commit and `nxsum2` checksum, and its built packages).

Packages are added by pull request (`nxpkg publish`); the checks run
`nxpkg index check`. Only packages with public sources are accepted.

## Rules

- A published version is never deleted, only yanked.
- The first publisher of a name becomes its owner; a new name, and a change of owners or
  signing keys, is reviewed by a person.
- Reserved: the standard library's module names, `std`, `nexus`, and names one edit away
  from an existing one unless a person accepts it.

Security: see [SECURITY.md](SECURITY.md).

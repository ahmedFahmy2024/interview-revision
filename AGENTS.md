# Agent instructions

## Dependency source code

Use `opensrc` to inspect dependency source code when implementation details are needed. Resolve the package source with `opensrc path <package>` and use the returned path with tools such as `rg` or `cat`:

```bash
rg "pattern" $(opensrc path <package>)
cat $(opensrc path <package>)/path/to/file
```

Use the version resolved from this repository's installed dependencies or lockfile. If a specific version is needed and the repository does not pin it, use `opensrc path --cwd <directory-with-lockfile> <package>` with a lockfile that pins that version.

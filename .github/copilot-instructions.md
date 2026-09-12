# Repository overview

This is a controller repository for CodeQL variant-analysis work. It contains independent CodeQL packs rather than an application or shared root workspace:

- `codeql-custom-queries-java`, `codeql-custom-queries-python`, and `codeql-custom-queries-actions` are executable query packs (`library: false`). Each pack imports its language library and owns its dependency manifest and lock file.
- `.github\codeql\extensions\kafka-java` is a Java model-extension library pack (`library: true`), not a query pack. Model files belong under its declared `models/**/*.yml` data-extension path.
- There is no root `codeql-workspace.yml`; run CodeQL commands against a specific pack or query path.

## Build and validation commands

The CodeQL CLI is required but is not vendored by this repository. Run commands from the repository root unless noted.

Install or refresh dependencies for one pack:

```powershell
codeql pack install .\codeql-custom-queries-java
codeql pack install .\codeql-custom-queries-python
codeql pack install .\codeql-custom-queries-actions
```

Compile a single query (the closest available equivalent to a single test):

```powershell
codeql query compile .\codeql-custom-queries-java\example.ql
```

Compile every query in one pack:

```powershell
codeql query compile .\codeql-custom-queries-java
```

Run a query against a previously created database:

```powershell
codeql query run --database <database-path> .\codeql-custom-queries-java\example.ql
```

Replace the pack path with the Python or Actions pack as appropriate. There are currently no QLTest fixtures, lint scripts, or repository-wide build/test commands.

## Query and pack conventions

- Keep each query in the pack matching its imported language: `import java`, `import python`, or `import actions`.
- Preserve the standard query metadata block. At minimum, executable queries should define `@name`, `@kind`, `@problem.severity`, and a unique language-prefixed `@id` such as `java/...`, `python/...`, or `actions/...`.
- Update the relevant `codeql-pack.yml` when changing pack dependencies. Keep the generated `codeql-pack.lock.yml` synchronized by rerunning `codeql pack install` in that pack.
- The query packs use the namespace pattern `getting-started/codeql-extra-queries-<language>`, `library: false`, and `warnOnImplicitThis: false`; follow that shape unless intentionally changing pack behavior.
- Kafka framework modeling belongs in `.github\codeql\extensions\kafka-java\models` as YAML data extensions. Keep the extension pack targeted at `codeql/java-all`; do not place executable `.ql` queries in that library pack.
- The checked-in `example*.ql` files are generated hello-world scaffolds. When turning them into real analyses, replace the metadata and selection logic rather than copying the sample identifiers unchanged.

## Editor workflow

The workspace recommends the `github.vscode-codeql` extension. Use the extension's database selection, query execution, result viewing, and AST navigation for interactive variant analysis; keep CLI commands above reproducible for dependency and compile checks.

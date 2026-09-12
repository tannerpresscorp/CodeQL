# CodeQL
Controller repository for variant analysts.

## Getting started with variant analysis

Use this repository as a lightweight controller while you begin CodeQL variant analysis work.

1. **Set up a CodeQL database**
   - Install the CodeQL CLI and the VS Code CodeQL extension.
   - Create a database for your target project:
     - `codeql database create <db-path> --language=<language> --source-root=<repo-path>`
   - In VS Code, add/open that database in the CodeQL extension.

2. **Inspect the code structure**
   - Open a source file from the loaded database.
   - Run **`CodeQL: View AST`** from the Command Palette to inspect parsed structure.

3. **Start modeling**
   - Begin with framework entry points (for example, controllers/handlers).
   - Identify source, sink, and sanitizer candidates.
   - Add or refine model definitions, then re-run your query against the database.

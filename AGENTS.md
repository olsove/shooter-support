# Repository Guidelines

## Project Structure & Module Organisation
This repository is currently a clean initial Git repository with no application files committed yet. Keep the top level tidy and group code by purpose as the project grows:

- `src/` for application code
- `tests/` for automated tests
- `assets/` for static files such as images or fixtures
- `docs/` for design notes, ADRs, and contributor-facing documentation

Prefer small, focused modules and keep test files close to the feature area they cover.

## Build, Test, and Development Commands
There is no build tooling configured yet. Add project-specific commands to a single entry point once tooling is introduced, for example:

- `make test` to run the full test suite
- `make lint` to run formatting and static checks
- `make dev` to start the local development environment

If you introduce another toolchain such as `npm`, `pytest`, or `cargo`, document the exact commands in `README.md` and keep them consistent with this file.

## Coding Style & Naming Conventions
Use the formatter and linter native to the language you add. Until a language-specific standard exists:

- use 4 spaces for indentation unless the ecosystem strongly prefers otherwise
- use descriptive file and module names
- prefer `snake_case` for file names and variables where applicable
- prefer `PascalCase` for types and classes

Keep functions short, avoid broad drive-by edits, and follow existing patterns before introducing new ones.

## Testing Guidelines
Add automated tests with every non-trivial feature or bug fix. Place tests under `tests/` or the language’s standard test location, and name them after the behaviour they cover, such as `test_auth_login.py` or `user-service.spec.ts`.

Run the smallest relevant test set locally before opening a pull request.

## Commit & Pull Request Guidelines
Use `[feature|fix|doc|chore]/<name>` branches; never push to `main`.
There is no existing history yet, so start with Conventional Commits:

- `feat(api): add match history endpoint`
- `fix(ui): handle empty state correctly`
- `doc(readme): markdown lint fixes`

Keep commits logically small. Pull requests should explain what changed, why it changed, how it was tested, and any risks or rollback steps. Link related issues when available and include screenshots for visible UI changes.

## Security & Configuration Tips
Never commit secrets. Store local configuration in ignored files such as `.env.local`, and provide safe examples in files such as `.env.example` once configuration is added.

## Architectural Decision Records (ADR)
- The local folder for ADRs is docs/adr
- Follow the README.md when creating a new ADR
- Use the template in docs/adr/README.md when creating a new ADR
- Always start out with the Proposed status when creating a new ADR
- Update the README.md in root with a link to the new ADR, it in the section "Key Documentation"

# Repository Guidelines

## Purpose & Project Structure

This repository maintains personal Codex plugins for requirements analysis, project analysis, design, and coding. Use this layout when adding plugins:

- `.agents/plugins/marketplace.json`: repository plugin catalog.
- `plugins/<plugin-name>/plugin.json`: independently installable plugin manifest.
- `plugins/<plugin-name>/skills/<skill-name>/SKILL.md`: skill instructions.
- Skill-local `references/`, `scripts/`, and `assets/`: documentation, executable helpers, and templates.
- `plugins/<plugin-name>/tests/`: prompt scenarios, fixtures, and script tests.

Group related skills into focused plugins; keep packages self-contained.

## Codex Compatibility

Follow [OpenAI's packaging guide](https://developers.openai.com/plugins/build/plugins). New packages use root `plugin.json` with `name`, semantic `version`, and `description`; put OpenAI-specific fields under `extensions.com.openai`. Use `.codex-plugin/plugin.json` only for compatibility.

Give the marketplace a stable `name`. Each `plugins[]` entry must include its plugin name, local source, `policy.installation`, `policy.authentication`, and `category`. Use `source: {"source": "local", "path": "./plugins/<plugin-name>"}`; paths resolve from the repository root. Plugin resource paths resolve from the plugin root; keep them inside the package and `./`-prefixed. Update catalog entries when adding or moving plugins.

## Build, Test & Development Commands

No build system, formatter, linter, or test runner is configured. From the repository root:

- `git diff --check`: check tracked changes for whitespace errors.
- `python3 -m json.tool .agents/plugins/marketplace.json`: check catalog JSON syntax once created; repeat for manifests.
- `codex plugin marketplace add .`: register this source after creating its catalog.
- `codex plugin marketplace list`: inspect registered sources.

JSON parsing does not validate schemas or paths.

## Coding Style & Skill Authoring

Use UTF-8, LF, two-space JSON/YAML indentation, and lowercase kebab-case identifiers. Preserve exact `SKILL.md` capitalization. Require YAML frontmatter with `name` and `description`. Describe activation conditions, inputs, steps, outputs, and missing-input handling. Keep instructions concise, link supporting files, and respect explicit user instructions. Follow [OpenAI's skills guide](https://developers.openai.com/plugins/build/skills).

## Testing Guidelines

No coverage threshold exists. Verify catalog/manifest consistency, referenced files, and frontmatter. Install changed plugins and test in a fresh Codex chat: direct and indirect activation, unrelated requests, incomplete inputs, and edge cases. Record prompts and expected results in `tests/<skill-name>.md`; test executable helpers separately.

## Commit & Pull Request Guidelines

Use Conventional Commits, e.g. `feat(design): add wireframe skill`. Keep changes focused. PRs should describe behavior, affected plugins, validation results, and linked issues when applicable. Include output examples for workflow changes and bump plugin versions for releases.

## Security & Configuration

Keep credentials, private work data, and machine-specific paths out of packages. Document required environment variables and external dependencies.

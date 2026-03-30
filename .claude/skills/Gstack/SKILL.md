```markdown
# Gstack Development Patterns

> Auto-generated skill from repository analysis

## Overview

This skill teaches the core development patterns and workflows used in the Gstack TypeScript codebase. Gstack is a modular system for skill generation and management, supporting multiple AI hosts (Claude, Codex, Factory Droid). The repository emphasizes strong conventions for file structure, code style, commit hygiene, and workflow automation. This guide covers coding conventions, workflow instructions, testing patterns, and common developer commands.

## Coding Conventions

### File Naming

- Use **camelCase** for file names.
  - Example: `genSkillDocs.ts`, `addSkillField.ts`

### Import Style

- Use **relative imports** for internal modules.
  ```typescript
  import { generateSkillDocs } from './genSkillDocs';
  import { resolveField } from '../resolvers/fieldResolver';
  ```

### Export Style

- Use **named exports** exclusively.
  ```typescript
  // Good
  export function generateSkillDocs() { ... }
  export const SKILL_FIELDS = [ ... ];

  // Avoid default exports
  // export default function() { ... }
  ```

### Commit Messages

- Follow **Conventional Commits**:
  - Prefixes: `feat`, `fix`, `docs`
  - Example: `feat: add support for Factory Droid host`
  - Average commit message length: ~71 characters

## Workflows

### skill-template-edit-and-regenerate
**Trigger:** When a SKILL.md.tmpl template or resolver is changed, or when adding a new skill  
**Command:** `/regenerate-skills`

1. Edit one or more `SKILL.md.tmpl` files or scripts in `scripts/resolvers/*.ts`.
2. Run the skill doc generator:
   ```sh
   bun run gen:skill-docs
   # or
   node scripts/gen-skill-docs.ts
   ```
3. Regenerate `SKILL.md` files for each skill directory (e.g., `office-hours/SKILL.md`).
4. Regenerate Codex/Factory files if needed (`.agents/skills/*/SKILL.md`, `.factory/skills/*/SKILL.md`).
5. Commit all regenerated `SKILL.md` and related files.

---

### version-bump-and-changelog-update
**Trigger:** When a feature, fix, or release is completed and needs to be recorded  
**Command:** `/bump-version`

1. Edit the `VERSION` file to increment the version (e.g., `v0.13.5.0` → `v0.13.6.0`).
2. Update `CHANGELOG.md` with a new entry at the top describing the changes.
3. Optionally, sync the `package.json` version field.
4. Commit `VERSION`, `CHANGELOG.md`, and `package.json`.

---

### security-audit-remediation
**Trigger:** When a security audit or review identifies vulnerabilities  
**Command:** `/security-fix`

1. Edit code to address security issues (e.g., path validation, auth, escaping, sandboxing).
2. Update or add tests to cover the remediations.
3. Update documentation to reflect new security practices.
4. Bump version and update changelog.

---

### add-or-update-skill-template-field-or-section
**Trigger:** When introducing a new feature or directive to all skills  
**Command:** `/add-skill-field`

1. Edit `scripts/resolvers/*.ts` to add new resolver or logic.
2. Update all relevant `SKILL.md.tmpl` files to include the new placeholder or section.
3. Run `scripts/gen-skill-docs.ts` to regenerate `SKILL.md` files.
4. Commit changes to templates, resolvers, and all regenerated `SKILL.md` files.

---

### test-suite-expansion-and-regression-guard
**Trigger:** When new features are added or bugs are fixed, especially for security or workflow logic  
**Command:** `/add-tests`

1. Write or update test files to cover new or changed behavior.
2. Add regression tests for known issues (e.g., zsh glob, codex cwd, security edge cases).
3. Commit test files and any related code changes.

---

### cross-host-skill-generation-and-factory-support
**Trigger:** When adding a new host or updating host-specific logic for skills  
**Command:** `/add-host-support`

1. Edit `scripts/gen-skill-docs.ts` and `scripts/resolvers/types.ts` to support the new host.
2. Update setup script to generate skills for the new host.
3. Regenerate skill docs for all hosts (`.agents/skills`, `.factory/skills`, etc.).
4. Update or add tests for host-specific output.
5. Commit all related changes.

---

### gitignore-and-remove-generated-or-build-files
**Trigger:** When a build artifact or generated output should not be tracked in git  
**Command:** `/ignore-build-output`

1. Edit `.gitignore` to add new generated directory (e.g., `.factory/`).
2. Remove files from git tracking:
   ```sh
   git rm --cached <generated directory>/*
   ```
3. Commit `.gitignore` and removal changes.

---

### sync-version-between-files
**Trigger:** When a version bump occurs and version fields must be kept in sync  
**Command:** `/sync-version`

1. Edit `VERSION` file.
2. Edit `package.json` version field to match.
3. Optionally update `SKILL.md` or other metadata if version is embedded.
4. Commit all version updates.

---

## Testing Patterns

- **Framework:** [jest](https://jestjs.io/)
- **Test File Pattern:** `*.test.ts` (e.g., `genSkillDocs.test.ts`)
- **Helpers:** Place shared logic in `test/helpers/*.ts`.
- **Example Test:**
  ```typescript
  import { generateSkillDocs } from '../scripts/genSkillDocs';

  describe('generateSkillDocs', () => {
    it('should generate SKILL.md for all skills', () => {
      // Arrange
      // ...setup code...

      // Act
      generateSkillDocs();

      // Assert
      // ...expect SKILL.md files to exist...
    });
  });
  ```

## Commands

| Command               | Purpose                                                        |
|-----------------------|----------------------------------------------------------------|
| /regenerate-skills    | Regenerate all SKILL.md files after template/resolver changes  |
| /bump-version         | Bump project version and update changelog                      |
| /security-fix         | Apply security remediations and update docs/tests              |
| /add-skill-field      | Add or update a field/section in all skill templates           |
| /add-tests            | Add or update tests for new features, bugfixes, or regressions |
| /add-host-support     | Add or update support for a new AI host                        |
| /ignore-build-output  | Add generated directories to .gitignore and remove from git    |
| /sync-version         | Sync version numbers across VERSION, package.json, SKILL.md    |
```

---
name: dev-setup
description: "Automated setup of developer-experience tooling: ESLint, changesets, husky, lint-staged, GitHub workflows, templates, and commitizen."
---

# dev-setup Skill

## Purpose

Set up best-practice developer tooling in a JavaScript/TypeScript project. This skill detects the package manager, checks for existing configurations, copies asset files, edits `package.json`, and installs dependencies.

## Arguments

- `--force` — Skip all interactive prompts. Use predefined overwrite/merge rules:
  - **Asset files**: overwrite unconditionally.
  - **`package.json` scripts**: merge new scripts; overwrite existing keys with skill defaults.
  - **`package.json` devDependencies**: merge new dependencies; if a version conflict exists, keep the higher semver version. If unparseable, overwrite with skill default.
  - **`package.json` lint-staged**: overwrite with skill default.

## Execution Workflow

### Detect Environment

1. Locate the project root by finding the nearest `package.json` upward from the current working directory.
2. If no `package.json` is found, abort with: "No package.json found. Run this skill from the project root directory."
3. Detect the package manager by checking lockfiles in this priority:
   - `pnpm-lock.yaml` → **pnpm**
   - `yarn.lock` → **yarn**
   - `package-lock.json` → **npm**
   - Default to **pnpm** if none found.
4. Read `package.json` to inspect existing `scripts`, `devDependencies`, and `lint-staged` config.

### Check Existing Configs

Scan the project for existing files that may conflict:

| Asset | Target Path |
|-------|-------------|
| ESLint config | `eslint.config.js` |
| CI workflow | `.github/workflows/ci.yml` |
| Release workflow | `.github/workflows/release.yml` |
| Snapshot release workflow | `.github/workflows/snapshot-release.yml` |
| PR template | `.github/PULL_REQUEST_TEMPLATE.md` |
| Bug report template | `.github/ISSUE_TEMPLATE/bug_report.md` |
| Feature request template | `.github/ISSUE_TEMPLATE/feature_request.md` |
| Changeset config | `.changeset/config.json` |
| Husky pre-commit | `.husky/pre-commit` |
| Husky pre-merge | `.husky/pre-merge` |
| Commitizen config | `.czrc` |
| lint-staged | `package.json` → `lint-staged` key |

Build a conflict report listing every item that already exists.

### Prompt User on Conflicts

- If `--force` is active, apply the force strategies from the Arguments section without prompting.
- If no conflicts exist, proceed automatically.
- If conflicts exist and `--force` is not active, present the conflict report to the user.
- For each conflict, ask: **overwrite**, **skip**, or **keep existing**.

### Copy Assets

For each item approved (or with no conflict):

Copy from `assets/<category>/` to the target path relative to the project root:

- `assets/eslint/eslint.config.js` → `eslint.config.js`
- `assets/workflows/ci.yml` → `.github/workflows/ci.yml`
- `assets/workflows/release.yml` → `.github/workflows/release.yml`
- `assets/workflows/snapshot-release.yml` → `.github/workflows/snapshot-release.yml`
- `assets/templates/PULL_REQUEST_TEMPLATE.md` → `.github/PULL_REQUEST_TEMPLATE.md`
- `assets/templates/ISSUE_TEMPLATE/bug_report.md` → `.github/ISSUE_TEMPLATE/bug_report.md`
- `assets/templates/ISSUE_TEMPLATE/feature_request.md` → `.github/ISSUE_TEMPLATE/feature_request.md`
- `assets/changeset/config.json` → `.changeset/config.json`
- `assets/changeset/README.md` → `.changeset/README.md`
- `assets/husky/pre-commit` → `.husky/pre-commit`
- `assets/husky/pre-merge` → `.husky/pre-merge`
- `assets/cz-emoji/.czrc` → `.czrc`

Ensure target directories exist before copying.

### Edit package.json

Add the following keys **only if they do not already exist or if overwrite was approved**:

**Scripts:**
```json
{
  "ci:version": "pnpm changeset version",
  "ci:publish": "pnpm run build && pnpm changeset publish",
  "ci:snapshot": "pnpm changeset version --snapshot snapshot",
  "ci:prerelease": "pnpm run build && pnpm changeset publish --no-git-tag --snapshot",
  "lint:fix": "eslint . --fix",
  "prepare": "husky install"
}
```

**lint-staged:**
```json
{
  "lint-staged": {
    "**/**/*.{js,ts,tsx,json,yml,yaml,md}": ["eslint --fix"]
  }
}
```

**devDependencies:**
```json
{
  "typescript": "^4.6.4",
  "eslint": "^8.56.0",
  "@aiou/eslint-config": "^1.2.1",
  "lint-staged": "^13.1.0",
  "@changesets/cli": "^2.26.0",
  "husky": "^8.0.3",
  "cz-emoji": "^1.3.1"
}
```

If `--force` is active:
- For scripts: overwrite existing keys with skill defaults.
- For devDependencies: keep the higher semver version when conflicting. If unparseable, use skill default.
- For lint-staged: overwrite unconditionally.

### Run Package Install

Execute the detected package manager's install command:
- **pnpm**: `pnpm install`
- **yarn**: `yarn install`
- **npm**: `npm install`

If install fails, capture stderr, display a concise error, and advise the user to run install manually.

### Verification

List everything that was created, modified, or skipped in a final summary. Confirm successful completion.

## Error Handling

- **No package.json**: Abort immediately with clear message.
- **File copy failures** (e.g., permission denied): Continue with remaining steps. Report failure in summary.
- **Install failures**: Surface stderr. Advise manual install. Do not retry.
- **Partial failures**: Always complete the full workflow and report what succeeded vs. failed.

## Rollback

No automatic rollback. The skill is additive. Overwrites require explicit user consent (or `--force`).

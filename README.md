# neo-skills

Collection of reusable AI skills for automated JavaScript/TypeScript project setup.

## What is this?

`neo-skills` is a curated set of AI-executable skills that bootstrap best-practice developer tooling into your projects. Instead of manually configuring ESLint, CI/CD workflows, changesets, git hooks, and commit conventions, these skills automate the entire setup.

## Available Skills

### `dev-setup`

Automated setup of developer-experience tooling:

- **ESLint** - Modern flat config with `@aiou/eslint-config`
- **GitHub Workflows** - CI, release, and snapshot release automation
- **Changesets** - Version management and changelog generation
- **Husky** - Git hooks for pre-commit and pre-merge checks
- **lint-staged** - Run linters on git staged files
- **Commitizen** - Conventional commits with emoji support
- **Templates** - Pull request and issue templates
- **Dotfiles** - `.czrc` and `.node-version` configuration

The skill detects your package manager (pnpm or bun), checks for existing configurations, and intelligently merges or prompts for conflicts.

## Usage

Run any skill directly with npx:

```bash
npx skills dev-setup
```

### Options

- `--force` - Skip all interactive prompts and apply defaults

## How It Works

1. **Detect Environment** - Finds `package.json` and identifies package manager
2. **Check Existing Configs** - Scans for conflicting files
3. **Prompt on Conflicts** - Asks whether to overwrite, skip, or keep existing (unless `--force`)
4. **Copy Assets** - Copies configuration files to appropriate locations
5. **Edit package.json** - Adds scripts, devDependencies, and configurations
6. **Install Dependencies** - Runs package manager install
7. **Verify** - Reports everything that was created, modified, or skipped

## Project Structure

```
skills/
  dev-setup/
    SKILL.md              # Skill definition and workflow
    assets/
      eslint/             # ESLint configuration
      workflows/          # GitHub Actions (pnpm & bun variants)
      templates/          # PR and issue templates
      changeset/          # Changeset configuration
      husky/              # Git hook scripts
      lint-staged/        # lint-staged configuration
      dotfiles/           # .czrc, .node-version
```

## License

MIT

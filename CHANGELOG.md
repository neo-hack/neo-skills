# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.4.1] - 2026-05-03

### Fixed

- **dev-setup**: Fixed changeset CLI usage across all workflow files.
  - Updated `@changesets/cli` from `^2.26.0` to `^2.31.0`.
  - Fixed `config.json` schema version to match CLI version (`@changesets/config@3.1.4`).
  - Removed incorrect `changeset pre enter snapshot` from snapshot workflows.
  - Fixed `publish --snapshot` to correct `publish --tag snapshot`.
  - Corrected `--no-git-tag` flag usage.

## [0.4.0] - 2026-05-03

### Changed

- **dev-setup**: Upgraded `@aiou/eslint-config` from `^3.0.2` to `^3.1.0`.
- **dev-setup**: Upgraded `typescript` from `^4.6.4` to `^5.9.3`.

## [0.3.1] - 2026-04-26

### Changed

- **dev-setup**: Upgraded `@aiou/eslint-config` from `^3.0.1` to `^3.0.2`.

## [0.3.0] - 2026-04-26

### Changed

- **dev-setup**: Upgraded `@aiou/eslint-config` from `^2.2.0` to `^3.0.1`.
- **dev-setup**: Renamed ESLint config asset from `eslint.config.js` (CJS) to `eslint.config.mjs` (ESM) with top-level `await`.

## [0.2.0] - 2026-04-26

### Changed

- **dev-setup**: Moved lint-staged configuration from inline `package.json` to standalone `.lintstagedrc` file (`assets/lint-staged/lintstagedrc`).
- **dev-setup**: Consolidated dotfiles (`.czrc`, `.node-version`) into `assets/dotfiles/` directory. Renamed asset files to remove leading dots (`cz-rc`, `node-version`) so they remain visible on disk after download.

### Fixed

- **dev-setup**: Added `chmod +x` step for husky scripts (`.husky/pre-commit`, `.husky/pre-merge`) after copying to ensure they are executable.

## [0.1.0] - 2026-04-26

### Added

- Initial release with `dev-setup` skill for automated developer tooling setup (ESLint, changesets, husky, lint-staged, GitHub workflows, templates, and commitizen).

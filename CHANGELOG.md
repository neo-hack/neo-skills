# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.3.0] - 2026-04-26

### Changed

- **dev-setup**: Upgraded `@aiou/eslint-config` from `^2.2.0` to `^3.0.0`.
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

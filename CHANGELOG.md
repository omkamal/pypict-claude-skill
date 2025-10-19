# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Planned
- Additional real-world examples (e-commerce, API testing, mobile apps)
- PICT syntax reference documentation
- Helper scripts for PICT model generation
- Integration with test management tools
- Support for higher-order combinatorial testing (3-way, 4-way)

## [1.0.0] - 2025-10-19

### Added
- Initial release of PICT Test Designer skill
- Core functionality for test case design using PICT methodology
- Comprehensive ATM system testing example
- Installation guide for Claude Code CLI and Desktop
- MIT License with proper attributions
- Contributing guidelines
- Documentation structure (README, SKILL.md, examples)
- GitHub Actions CI workflow
- Example directory with ATM specification and test plan

### Features
- Automated parameter identification from requirements
- PICT model generation with constraints
- Expected output determination
- Pairwise test case generation
- Support for multiple testing domains
- Comprehensive documentation and examples

### Credits
- Built on Microsoft PICT
- Uses pypict Python bindings by Kenichi Maehashi
- Designed for Claude AI by Anthropic

## Version History

### Versioning Scheme

- **Major version (X.0.0)**: Incompatible API changes or major feature additions
- **Minor version (0.X.0)**: New features in a backward-compatible manner
- **Patch version (0.0.X)**: Backward-compatible bug fixes

### Release Types

- **[Unreleased]**: Changes in development but not yet released
- **[Version]**: Released version with date

### Change Categories

- **Added**: New features
- **Changed**: Changes in existing functionality
- **Deprecated**: Soon-to-be removed features
- **Removed**: Removed features
- **Fixed**: Bug fixes
- **Security**: Security vulnerability fixes

---

## How to Contribute to Changelog

When submitting a pull request, add your changes to the [Unreleased] section under the appropriate category (Added, Changed, Fixed, etc.).

Example:
```markdown
## [Unreleased]

### Added
- New example for mobile app testing (#42)

### Fixed
- Typo in installation instructions (#38)
```

The maintainers will move items from [Unreleased] to a versioned release when publishing a new version.

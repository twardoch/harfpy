# Changelog

All notable changes to this fork are documented here. This project follows
[Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- Modern PEP 621 packaging via `pyproject.toml` (hatchling + hatch-vcs), making
  the module installable with `pip install git+…`. The upstream `setup.py` is
  retained for reference.
- `README.md` with provenance, install instructions, and a verified shaping
  example (the previous `README` was only an "archived, moved elsewhere" notice).
- `COPYING` with the full LGPL v2.1 text referenced by the module header.
- GitHub Actions CI that installs the system HarfBuzz library and runs an import
  and shaping smoke test on Linux and macOS.
- `.gitignore` and `TODO.md`.
- Project icon at `docs/assets/icon.png`.

### Notes
- The binding code in `harfbuzz.py` is unchanged upstream work by Lawrence
  D'Oliveiro; this fork only adds scaffolding and documentation.

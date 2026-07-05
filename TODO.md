# TODO

Bigger ideas beyond the scaffolding pass. This is a fork; the binding in
`harfbuzz.py` is upstream code, so most substantive work should flow to or from
the canonical repo at <https://gitlab.com/ldo/harfpy>.

- [ ] Sync with upstream: diff `harfbuzz.py` against the current GitLab version
      and pull in any fixes for newer HarfBuzz ABI changes.
- [ ] Publish to PyPI as `harfpy` once provenance/ownership with upstream is
      clarified (coordinate with Lawrence D'Oliveiro; respect LGPL).
- [ ] Bundle the library discovery logic so macOS Homebrew users don't need
      `DYLD_LIBRARY_PATH` (e.g. try `ctypes.util.find_library` fallbacks).
- [ ] Add a small pytest suite (shape a fixture font, assert glyph count and
      advances) rather than relying only on the CI smoke test.
- [ ] Evaluate whether to deprecate in favour of the maintainers' `uharfbuzz`
      and document the recommendation prominently if so.
- [ ] Optional: type stubs (`harfbuzz.pyi`) for the public API subset.

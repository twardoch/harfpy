# HarfPy — Python bindings for HarfBuzz

HarfPy is a pure-Python [`ctypes`](https://docs.python.org/3/library/ctypes.html)
binding for [HarfBuzz](https://harfbuzz.github.io/), the OpenType text-shaping
engine used by Firefox, Chrome, LibreOffice, Android, and most of the modern
type stack. It turns a run of Unicode text plus a font into the sequence of
positioned glyphs needed to render that text correctly, including ligatures,
kerning, mark attachment, and complex-script reordering.

There is no compiled extension: HarfPy loads the system HarfBuzz shared library
at import time and calls into it directly, so it stays in step with whatever
HarfBuzz version your OS provides.

## Provenance

This repository is a maintained mirror/fork of Lawrence D'Oliveiro's original
**harfpy**. Lawrence retired the GitHub copy in 2023 and moved development to
GitLab; this fork preserves the last GitHub state, restores the `harfbuzz.py`
module and `setup.py` that the archival notice had removed, and adds modern
packaging, CI, and documentation on top. The binding code in `harfbuzz.py` is
unchanged upstream work.

- Upstream (canonical): <https://gitlab.com/ldo/harfpy>
- Original author: Lawrence D'Oliveiro `<ldo@geek-central.gen.nz>`
- License: LGPL v2.1 or later (see [`COPYING`](COPYING))

### When to use HarfPy vs. alternatives

If you are starting a new project and just need HarfBuzz from Python, the
HarfBuzz maintainers' own [`uharfbuzz`](https://github.com/harfbuzz/uharfbuzz)
binding is the mainstream choice — it ships prebuilt wheels and bundles the
library. Reach for HarfPy when you specifically want a dependency-free `ctypes`
binding that links against the system HarfBuzz, or when you are already using
Lawrence D'Oliveiro's companion bindings (`python_freetype`, Qahirah, PyBidi),
which HarfPy integrates with directly.

## Installation

HarfPy needs the HarfBuzz shared library present on your system.

```bash
# macOS
brew install harfbuzz

# Debian / Ubuntu
sudo apt-get install libharfbuzz0b

# Fedora
sudo dnf install harfbuzz
```

Then install the Python module:

```bash
pip install git+https://github.com/twardoch/harfpy
```

On macOS, Homebrew installs the library outside the default loader path, so you
may need to point `ctypes` at it:

```bash
export DYLD_LIBRARY_PATH="$(brew --prefix)/lib:$DYLD_LIBRARY_PATH"
```

## Usage

A minimal shaping example — turn a string into glyphs and their positions:

```python
import harfbuzz as hb

HB = hb.HARFBUZZ

# Load a font into a HarfBuzz face/font via a blob of its raw bytes.
data = open("/path/to/font.otf", "rb").read()
blob = hb.Blob.create_for_array(data, HB.MEMORY_MODE_READONLY)
face = hb.Face.create(blob, 0, False)  # (blob, face_index, autoscale)
font = hb.Font.create(face)

# Build and configure a buffer.
buf = hb.Buffer.create()
buf.add_str("Hello, world!")
buf.guess_segment_properties()  # infer script, language, and direction

# Shape, then read off the positioned glyphs.
hb.shape(font, buf)
for info, pos in zip(buf.glyph_infos, buf.glyph_positions):
    print(info.codepoint, info.cluster, pos.x_advance, pos.y_advance)
```

If you have Lawrence D'Oliveiro's `python_freetype` installed, HarfPy can also
create a font directly from a FreeType face — see the upstream examples.

Check the HarfBuzz version HarfPy is bound to:

```python
import harfbuzz as hb
print(hb.version_string())
```

The full API mirrors the HarfBuzz C API closely; see the
[HarfBuzz manual](https://harfbuzz.github.io/) for the underlying concepts.

## License

LGPL v2.1 or later, matching upstream. See [`COPYING`](COPYING).

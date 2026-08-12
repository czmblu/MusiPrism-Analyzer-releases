# What is inside MusiPrism Analyzer, and under what terms

The ready-to-run packages carry a complete working environment: an interpreter,
libraries, trained models and two command-line programs. None of that is ours.
Each keeps its own licence, and those licences are not affected by the one on
MusiPrism Analyzer itself.

They are listed worst-obligation first: the two that place a condition on this
program, then the ones that ask us to make their source reachable, then the rest.

## ffmpeg and ffprobe — GPL-3.0-or-later

`bin/ffmpeg.exe` and `bin/ffprobe.exe` are the *full* builds published by Gyan
Doshi, configured with `--enable-gpl --enable-version3` (read out of the
binaries, not assumed). They are therefore covered by the **GNU General Public
License, version 3 or later**, not by the LGPL that a default ffmpeg build uses.
Neither carries `--enable-nonfree`, which would have made them impossible to pass
on at all.

MusiPrism Analyzer runs them as separate programs — it starts them and reads
what they write — and does not link against them, so shipping them together is
an aggregate: their licence applies to them, ours to the rest. What the GPL does
require is that whoever receives those binaries can get their source.

- Licence: <https://www.gnu.org/licenses/gpl-3.0.html>
- Source and build scripts for these exact builds:
  <https://github.com/GyanD/codexffmpeg> and <https://www.gyan.dev/ffmpeg/builds/>
- Upstream source: <https://ffmpeg.org/download.html>

## The genre model — CC BY-NC-SA 4.0

`mtg-upf/discogs-maest-30s-pw-73e-ts`, from the Music Technology Group at
Universitat Pompeu Fabra, is released under **Creative Commons
Attribution-NonCommercial-ShareAlike 4.0**.

**Non-commercial** is the part to read twice: with this model inside, the
package may not be used commercially — by anyone, including us. Recognising the
genre is the only feature that depends on it; everything else would work without
it.

- Model: <https://huggingface.co/mtg-upf/discogs-maest-30s-pw-73e-ts>
- Licence: <https://creativecommons.org/licenses/by-nc-sa/4.0/>

## The LGPL libraries, and where their source is

Four components are under the **Lesser GPL**. That licence lets a program like
this one use them and be distributed with them, on two conditions: say they are
there, and let whoever receives them get their source and put a modified version
in their place. This section is the first condition; the links are the second.

Three of them arrive as loadable modules that sit in the package as separate
files, so replacing one with your own build is a matter of overwriting the file.
`fpcalc.exe` is a program of its own, run and never linked to.

| What | File | Licence | Source |
|---|---|---|---|
| **Chromaprint**, and the **FFmpeg** built into it | `bin/fpcalc.exe` | LGPL-2.1-or-later | <https://github.com/acoustid/chromaprint> · <https://ffmpeg.org/download.html> |
| **LAME**, through `lameenc` | `lameenc.cp312-win_amd64.pyd` | LGPL-3.0-or-later (LAME: LGPL-2.0-or-later) | <https://github.com/chrisstaite/lameenc> · <https://lame.sourceforge.io/> |
| **libsoxr**, through `python-soxr` | `soxr/soxr_ext.pyd` | LGPL-2.1-or-later | <https://github.com/dofuuz/python-soxr> · <https://sourceforge.net/projects/soxr/> |
| **libsndfile**, under `soundfile` | `_soundfile_data/libsndfile_x64.dll` | LGPL-2.1-or-later | <https://github.com/libsndfile/libsndfile> |

Two of those need a word each. The FFmpeg inside `fpcalc.exe` is **built into
it**, not loaded beside it — that is how the official Chromaprint tool is
published — and it is an LGPL build, without `--enable-gpl`. And the Python
packages that wrap LAME and libsoxr are BSD-licensed code of their own, but the
compiled module they ship carries the LGPL library inside it, so the module is
what the licence follows, not the Python source.

`libsndfile` in turn contains FLAC, Ogg, Vorbis and Opus, which are BSD-licensed,
and libmpg123, which is LGPL-2.1; its own source archive carries all of them.

## PyTorch, and what PyTorch brings with it

`torch` and `torchaudio` are **BSD-3-Clause**, but the wheels carry compiled
libraries that are not.

- **The NVIDIA edition only** ships cuBLAS, cuDNN, cuFFT, cuRAND, cuSPARSE,
  nvrtc and nvJitLink — about 1.7 GB — under NVIDIA's own licences, which permit
  redistribution as part of an application and set their own conditions:
  <https://docs.nvidia.com/cuda/eula/> and
  <https://docs.nvidia.com/deeplearning/cudnn/sla/>. The processor edition
  contains none of them.
- **Both editions** ship `libiomp5md.dll`, the Intel OpenMP runtime, © Intel
  Corporation.

PyTorch collects the notices of everything it bundles in `LICENSE` inside its own
`torch-*.dist-info` folder, which travels in the package: it is five thousand
lines and it is the authoritative list for that part.

## Everything else

| Component | Licence |
|---|---|
| Python 3.12 (embeddable distribution) | PSF License |
| Demucs, and the `htdemucs` / `htdemucs_ft` / `htdemucs_6s` weights | MIT |
| transformers, huggingface_hub, safetensors, tokenizers | Apache-2.0 |
| librosa (ISC), numpy, scipy, scikit-learn, soundfile (the Python part) | BSD-3-Clause / ISC |
| FastAPI, Starlette, uvicorn, pydantic | MIT / BSD-3-Clause |
| pywebview, and pythonnet / clr_loader / bottle underneath it | BSD-3-Clause / MIT |
| basic-pitch, and its bundled ONNX model | Apache-2.0 |
| onnxruntime, pretty_midi, mir_eval, mido, resampy | MIT / ISC |
| numba, llvmlite | BSD-2-Clause, Apache-2.0 with LLVM exception |
| certifi, tqdm | MPL-2.0 (tqdm also MIT) — source at their own projects |
| 7-Zip (`7z.exe`, `7z.dll`, inside the installer only) | LGPL-2.1 — source at <https://www.7-zip.org/> |

MPL-2.0 is copyleft per *file*: it asks that the source of those files stay
available, which it is, and it puts no condition on the program around them.

The services it queries over the network — AcoustID, MusicBrainz, LRCLIB,
Deezer — are not redistributed and keep their own terms of use.

If something in this list is wrong or missing, it is a mistake and not a claim:
write to papeoalessio@gmail.com and it gets corrected.

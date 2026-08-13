# What is inside MusiPrism Analyzer, and under what terms

The ready-to-run packages carry a complete working environment: an interpreter,
libraries, trained models and two command-line programs. None of that is ours.
Each keeps its own licence, and those licences are not affected by the one on
MusiPrism Analyzer itself.

They are listed worst-obligation first: the one that places a condition on this
program, then the ones that ask us to make their source reachable, then the rest.

Until 1.0.11 there were two. The genre model was **CC BY-NC-SA 4.0**, and its
non-commercial clause covered the whole package, us included; it has been
replaced by one built here, and that section now describes what took its place.

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

## The genre model — ours, and what it is made of

The genre is worked out by two pieces, and neither carries a non-commercial
clause. This replaced `mtg-upf/discogs-maest-30s-pw-73e-ts` (MAEST) in 1.0.12,
which was CC BY-NC-SA 4.0 and made the whole package unusable commercially by
anybody, ourselves included.

**The ear** is `MIT/ast-finetuned-audioset-10-10-0.4593` — the Audio Spectrogram
Transformer, from MIT, **BSD-3-Clause**. It is used unchanged and untrained: it
turns the sound into 768 numbers and is never asked what genre it is.

- Model: <https://huggingface.co/MIT/ast-finetuned-audioset-10-10-0.4593>
- Licence: <https://opensource.org/license/bsd-3-clause>

**The answer** is a single linear layer on top of those numbers, trained here.
It is 200 kB, it sits in `app/analysis/modello_genere/`, and it is ours to give
away under the same terms as the rest of the program.

**What it was taught on** is the part that needs saying out loud. The tracks come
from the **Free Music Archive**, and only those whose own licence allows
commercial use: **CC0**, **Public Domain Mark**, **CC BY** and **CC BY-SA**.
Everything NonCommercial was left out — training on it would have put back the
very clause we removed — and so was everything NoDerivatives, which is a
narrower question but not one worth arguing about for a few hundred tracks.
Of the 106,574 tracks in the archive, 11,154 passed that filter and 10,613 were
used.

CC BY and CC BY-SA ask for attribution. The music itself is not redistributed —
what ships is a layer of numbers — but the tracks are named all the same, by
their Free Music Archive identifiers, in
`app/analysis/modello_genere/brani-usati.txt`, which travels inside the package.
The list is also reproducible from scratch with `tools/genere/elenco_brani.py`.

- Archive: <https://freemusicarchive.org/>
- The dataset those files come from: <https://github.com/mdeff/fma>
- Licences: <https://creativecommons.org/licenses/>

## The LGPL libraries, and where their source is

Three components are under the **Lesser GPL**. That licence lets a program like
this one use them and be distributed with them, on two conditions: say they are
there, and let whoever receives them get their source and put a modified version
in their place. This section is the first condition; the links are the second.

All three arrive as loadable modules that sit in the package as separate files,
so replacing one with your own build is a matter of overwriting the file.

| What | File | Licence | Source |
|---|---|---|---|
| **LAME**, through `lameenc` | `lameenc.cp312-win_amd64.pyd` | LGPL-3.0-or-later (LAME: LGPL-2.0-or-later) | <https://github.com/chrisstaite/lameenc> · <https://lame.sourceforge.io/> |
| **libsoxr**, through `python-soxr` | `soxr/soxr_ext.pyd` | LGPL-2.1-or-later | <https://github.com/dofuuz/python-soxr> · <https://sourceforge.net/projects/soxr/> |
| **libsndfile**, under `soundfile` | `_soundfile_data/libsndfile_x64.dll` | LGPL-2.1-or-later | <https://github.com/libsndfile/libsndfile> |

There was a fourth until 1.0.12: **Chromaprint**, with an LGPL build of FFmpeg
compiled into `fpcalc.exe`. It computed the acoustic fingerprint for AcoustID,
which is gone — artist and title are read from the file's own tags now — so the
executable is gone with it.

The Python packages that wrap LAME and libsoxr are BSD-licensed code of their
own, but the compiled module they ship carries the LGPL library inside it, so
the module is what the licence follows, not the Python source.

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

## The services it talks to

Nothing here is redistributed — these are queried over the network while the app
runs — and none of them asks for a key.

**ListenBrainz** provides the similar tracks and similar artists. It is run by
the MetaBrainz Foundation, and its data is published openly. It replaced
**Deezer** in 1.0.12: Deezer's terms allow its API **only for a non-commercial
purpose and in a non-commercial environment**, which was the second independent
reason this package could not be used commercially. That reason is gone; the
genre model went in the same version, so **nothing in this package is
non-commercial any more**. The 30-second previews went with Deezer —
ListenBrainz collects listens, not music.

**MusicBrainz**, from the same foundation, turns a name into a catalogue entry:
it is what album, year and the identifiers behind the recommendations come from.
It wants requests to carry an application name, a version and a contact that
works, so that whoever asks too much can be warned rather than cut off.
MusiPrism sends its name, its version and the address of its project page, and
keeps to one request a second.

The covers are from the **Cover Art Archive**, hosted by the Internet Archive.
Each image belongs to whoever made it; they are shown, never stored.

**AcoustID** was here until 1.0.12 and needed a key of your own. It is gone.
**LRCLIB** needs no key. The lyrics it returns are the work of their authors and
publishers: MusiPrism displays them while you listen and does not keep them.

If something in this list is wrong or missing, it is a mistake and not a claim:
write to papeoalessio@gmail.com and it gets corrected.

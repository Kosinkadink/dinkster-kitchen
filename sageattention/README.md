# Managed SageAttention wheels

This directory builds the `dinkster-kitchen` distribution from the pinned
SageAttention 2.2.0 source. The distribution provides the unchanged
`sageattention` import package as a binary build with embedded provenance. It
does not package this repository's own Python library.

`source.json` pins the source archive, upstream commit, Apache-2.0 license,
Torch and CUDA versions, build tools, and packaging revision.
`prepare_source.py` verifies the archive before extracting it and applies
`packaging.patch`. Each wheel includes the upstream license, attribution, and
`sageattention/_dinkster_kitchen_provenance.json`. The pinned upstream source
has no NOTICE file, so the packaging patch adds the distribution notice.

The binary build supports Python 3.12, Torch 2.13.0, CUDA 13.0, Linux x86_64,
and Windows x86_64. Its native extensions cover SM80, SM86, SM89, SM90, and
SM120. Linux uses Triton 3.7.1; Windows uses
`triton-windows==3.7.1.post27`.

Install the published build with its matching CUDA-enabled Torch package:

```bash
python -m pip install torch==2.13.0 --index-url https://download.pytorch.org/whl/cu130
python -m pip install dinkster-kitchen==2.2.0.post1
```

To install a downloaded release wheel into an environment that already has
the pinned runtime dependencies:

```bash
python -m pip install --no-deps ./dinkster_kitchen-2.2.0.post1-*.whl
```

Prepare the verified source tree without modifying the checkout:

```bash
python sageattention/prepare_source.py sageattention/build/source
```

Pass `--archive` to use an already-downloaded source archive. The workflow
builds and verifies the manylinux x86_64 and win_amd64 wheels.

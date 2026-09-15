# Data boundary and privacy rules

This repository contains two independent data paths:

- `dataset/` is the 20-image Task A training/evaluation workflow. Keep its official split and filenames unchanged.
- `data/images/` is a 10-image cabin practice pack for learning visibility and privacy decisions. Do not use it for training, evaluation, or as a substitute for Task A.

The cabin pack contains real and consented-scan derivatives. Every cabin face is covered with an opaque mask and its five facial keypoints are `v=0`; this is a task rule, not a claim that the image is anonymous. Do not try to remove a mask, recover a face, add raw source material, or add personal information.

Attribution, license links, source selection and output hashes are in `data/image-manifest.csv`, `data/source-selection.json` and `THIRD_PARTY_NOTICES.md`. Keep those files with any copy of the cabin pack. The pack is classroom/noncommercial only because one source is CC BY-NC 4.0.

Before changing or redistributing an image, run:

```bash
python3 scripts/audit-data-pack.py
```

The audit checks the exact ten active files, their hashes, image metadata and declared data boundary. A passing audit does not itself establish that a person is anonymous or authorize a new use.

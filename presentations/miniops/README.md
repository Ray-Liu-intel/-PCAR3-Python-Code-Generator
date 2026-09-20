# MiniOps management presentation deliverables

This directory contains the generated bilingual PowerPoint decks and the reproducible generator used to create them.

## Files

- `MiniOps_Management_CN.pptx`
- `MiniOps_Management_EN.pptx`
- `make_miniops_ppt.py`
- `requirements.txt`

## Download links

These repository-relative links resolve to the exact generated files on the branch you are currently viewing:

- [MiniOps_Management_CN.pptx](./MiniOps_Management_CN.pptx)
- [MiniOps_Management_EN.pptx](./MiniOps_Management_EN.pptx)
- [make_miniops_ppt.py](./make_miniops_ppt.py)

If GitHub opens the file page instead of downloading directly, use **Download raw file** or **Save link as...** from that page.

## Local regeneration

From the repository root:

```bash
python3 -m pip install -r presentations/miniops/requirements.txt
python3 presentations/miniops/make_miniops_ppt.py
```

Optional examples:

```bash
python3 presentations/miniops/make_miniops_ppt.py --language cn
python3 presentations/miniops/make_miniops_ppt.py --language en --output-dir /tmp/miniops-ppt
```

The generator writes both `.pptx` files into this directory and then reopens them with `python-pptx` to verify:

- valid package/open succeeds
- expected filename and non-empty output file
- expected deck title in core properties and first slide title
- exactly six slides per deck
- non-empty speaker notes on every slide
- an installed font is available from the generator's preferred font list for each deck, or generation stops with a clear error

## Scope and validation notes

- The existing HTML application in this repository is intentionally unchanged.
- The decks use editable PowerPoint text boxes, tables, and shapes only; no external or unlicensed images are embedded.
- Public GitHub repository URLs are used in notes as sources.
- No slide rendering tool was available in the agent environment during generation, so the validation performed here covers package integrity and note content rather than rendered-image inspection.

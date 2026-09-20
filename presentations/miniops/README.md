# MiniOps management presentation deliverables

This directory contains the generated bilingual PowerPoint decks and the reproducible generator used to create them.

## Files

- `MiniOps_Management_CN.pptx`
- `MiniOps_Management_EN.pptx`
- `make_miniops_ppt.py`
- `requirements.txt`

## Current output-branch direct download links

These exact raw URLs point to the generated files on the current output branch `copilot/create-miniops-presentation`:

- Chinese deck: <https://raw.githubusercontent.com/Ray-Liu-intel/-PCAR3-Python-Code-Generator/copilot/create-miniops-presentation/presentations/miniops/MiniOps_Management_CN.pptx>
- English deck: <https://raw.githubusercontent.com/Ray-Liu-intel/-PCAR3-Python-Code-Generator/copilot/create-miniops-presentation/presentations/miniops/MiniOps_Management_EN.pptx>
- Generator: <https://raw.githubusercontent.com/Ray-Liu-intel/-PCAR3-Python-Code-Generator/copilot/create-miniops-presentation/presentations/miniops/make_miniops_ppt.py>

If GitHub serves the PowerPoint files in-browser, use **Save link as...** to download them directly.

## Stable repository paths

- `presentations/miniops/MiniOps_Management_CN.pptx`
- `presentations/miniops/MiniOps_Management_EN.pptx`
- `presentations/miniops/make_miniops_ppt.py`

After merge to the default branch, the stable raw URLs are expected to be:

- <https://raw.githubusercontent.com/Ray-Liu-intel/-PCAR3-Python-Code-Generator/main/presentations/miniops/MiniOps_Management_CN.pptx>
- <https://raw.githubusercontent.com/Ray-Liu-intel/-PCAR3-Python-Code-Generator/main/presentations/miniops/MiniOps_Management_EN.pptx>
- <https://raw.githubusercontent.com/Ray-Liu-intel/-PCAR3-Python-Code-Generator/main/presentations/miniops/make_miniops_ppt.py>

## Local regeneration

From the repository root:

```bash
python3 -m pip install -r presentations/miniops/requirements.txt
python3 presentations/miniops/make_miniops_ppt.py
```

The generator writes both `.pptx` files into this directory and then reopens them with `python-pptx` to verify:

- valid package/open succeeds
- exactly six slides per deck
- non-empty speaker notes on every slide

## Scope and validation notes

- The existing HTML application in this repository is intentionally unchanged.
- The decks use editable PowerPoint text boxes, tables, and shapes only; no external or unlicensed images are embedded.
- Public GitHub repository URLs are used in notes as sources.
- No slide rendering tool was available in the agent environment during generation, so the validation performed here covers package integrity and note content rather than rendered-image inspection.

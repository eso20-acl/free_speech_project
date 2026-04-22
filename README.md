# Free Speech Link Compiler (TXT → DOCX)

This project takes a **text file full of links** and turns it into **one Word document (`.docx`)** that contains the text from each link.

It’s meant for pages like FreeSpeechProject entries (it tries to grab the title, “first posted / last updated”, and the main article text).

## What you need (one-time setup)

- **Python 3.10+** (Python 3.11 is great)
- Internet connection (it downloads the pages)

### Install the Python packages (Mac-friendly)

On many Macs, you can’t install packages “globally” with pip. The easiest fix is to use a **virtual environment** (a private Python just for this folder).

```bash
cd "free_speech_compiler_clean"
python3 -m venv .venv
./.venv/bin/python -m pip install -r requirements.txt
```

## Your input file (the links)

Make a plain text file like `links.txt` where each line is a URL:

```txt
https://example.com/page1
https://example.com/page2
```

Rules:
- Blank lines are ignored.
- Lines starting with `#` are ignored (use those for notes).
- Duplicate URLs are removed automatically.

## Run it (copy/paste)

From this folder, run:

```bash
./.venv/bin/python compile_links_to_docx.py --input links.txt --output output.docx
```

### Optional knobs (if a site is slow or picky)

- **Slow down requests** (be nicer to websites):

```bash
./.venv/bin/python compile_links_to_docx.py --input links.txt --output output.docx --delay 1.0
```

- **Give each page more time to load**:

```bash
./.venv/bin/python compile_links_to_docx.py --input links.txt --output output.docx --timeout 60
```

## Where the results go (and how to “store information after”)

The script saves whatever path you give `--output`.

A simple way to stay organized is to use an `outputs/` folder and name files by date. (The script will create the output folder automatically if it doesn’t exist.)

```bash
./.venv/bin/python compile_links_to_docx.py --input links.txt --output "outputs/combined_$(date +%Y-%m-%d).docx"
```

Recommended workflow:
- Keep your inputs in `inputs/` (like `inputs/links_2026-04-22.txt`)
- Keep your Word exports in `outputs/`
- Keep a quick log in `outputs/NOTES.md` (example below)

Example `outputs/NOTES.md`:

```md
## 2026-04-22
- Input: inputs/links_2026-04-22.txt
- Output: outputs/combined_2026-04-22.docx
- What changed: added 3 new links, updated delay to 1.0
```

## What the script does (in normal words)

`compile_links_to_docx.py`:
- Reads URLs from your `--input` text file
- Downloads each webpage
- Extracts readable text (headings, paragraphs, list items)
- Writes everything into a single `.docx`
- Adds a page break between links
- If a link fails, it still writes an error page so you can see which one broke

## Troubleshooting

- **“No URLs found in input file.”**
  - Make sure your file has real `http://` or `https://` links (one per line).

- **It says (FAILED) for some links**
  - That site might block automated requests, require login, or be temporarily down.
  - Try `--delay 1.0` and/or `--timeout 60`.

- **`ModuleNotFoundError: ...`**
  - Re-run the install step:

```bash
./.venv/bin/python -m pip install -r requirements.txt
```

## Saving changes to GitHub (so you don’t lose work)

### If this folder is NOT a git repo yet (first time only)

1) In this folder:

```bash
git init
git add .
git commit -m "Initial commit"
```

2) Create an empty repo on GitHub (website), then connect it:

```bash
git branch -M main
git remote add origin YOUR_GITHUB_REPO_SSH_OR_HTTPS_URL
git push -u origin main
```

### If it’s ALREADY on GitHub (the normal “update” loop)

Any time you change code / inputs / README:

```bash
git status
git add -A
git commit -m "Update link compiler"
git push
```

Tip: `git status` is your “what changed?” button.


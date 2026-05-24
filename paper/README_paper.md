# PROFE 2026 — Matching subtask paper

Files:
- `main.tex` — the paper (CEUR-WS `ceurart` template, single column).
- `refs.bib` — bibliography.
- `figures/level_distribution.pdf` — CEFR-level distribution figure (referenced by the paper).

## How to compile (recommended: Overleaf)

1. Go to Overleaf → New Project → **Templates** → search **"CEURART"**.
2. In that project, replace `main.tex` with this `main.tex`, upload `refs.bib`,
   and upload the `figures/` folder.
3. Compile (pdfLaTeX). Overleaf already ships `ceurart.cls` and the CEUR bib style.

Local compile (if you install a full TeX Live + `ceurart.cls` from
<https://github.com/yamadharma/ceurart>):

```
pdflatex main
bibtex   main
pdflatex main
pdflatex main
```

## Placeholders to fill before submitting (search for `%% TODO`)

- [ ] **Team name** in the title (`[TEAM]`).
- [ ] **Author names** and (optional) ORCIDs.
- [ ] **Affiliation / university** (`\address`).
- [ ] **Author emails** (one is pre-filled: davidbojaca1@gmail.com).
- [ ] **Conference venue/city** in `\conference{...}`.
- [ ] **Code repository URL** in Section "Reproducibility".
- [ ] **Exact multimodal score** in Table 2 (the `$<0.6667$` row + footnote).
      We only know it was below the text-only system; insert the real
      Codabench number if you kept it.
- [ ] **Confirm the 24/27 dev figure** (Section "Results"). This was the
      observed result on the labelled example exercises; verify before final.
- [ ] Update the three `%% TODO` citations in `refs.bib` (PROFE 2026 task
      overview and IberLEF 2026 overview) once the official references are
      published.

## Key numbers used (all verified from the data, except where flagged)

| Item | Value | Source |
|------|-------|--------|
| Matching exercises (test) | 189 | `matching_dataset.json` |
| Exams | 126 | computed |
| Questions (matching points) | 1,394 | computed |
| Avg options per exercise (set1) | 8.88 (3–11) | computed |
| Avg questions per exercise (set2) | 7.38 (6–10) | computed |
| Exercises with any image | 71 (37.6%) | computed |
| Exercises with image among options (set1) | 27 (14.3%) | computed |
| Exercises with explicit distractors | 13 | computed |
| ChatGPT baseline (matching) | 0.51 | task.txt / organisers |
| **Our text-only accuracy** | **0.6667** | Codabench |
| Dev on labelled examples | 24/27 (88.9%) | observed — **confirm** |

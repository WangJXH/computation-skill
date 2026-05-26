---
name: paper-writing
description: Scaffold and assist with academic paper writing for computational modeling and mathematics. Handles project init (copy Springer LaTeX template), outline planning, and prose drafting with anti-AI guard. Use when starting a new paper, structuring sections, or drafting content for Springer-format computational science papers.
origin: user
---

# Paper Writing — Computational Modeling & Mathematics

## When to Activate

- Starting a new academic paper project
- Planning section structure before writing
- Drafting or improving prose for a computational modeling / mathematics paper
- Formatting tables, figures, or equations in LaTeX

## Modes

Detect the user's intent and activate the right mode. Ask if unclear.

| Mode | Trigger |
|------|---------|
| `init` | "start a new paper", "scaffold", "create paper project" |
| `outline` | "outline", "structure", "plan sections" |
| `write` | "draft", "write", "help me write", "improve this text" |

---

## Mode: init

**Goal**: Scaffold a new paper project from the user's Springer template.

### Steps

1. Ask:
   - Paper title
   - Authors (comma-separated)
   - Journal name
   - Target directory path

2. Copy the full template folder:
   ```bash
   cp -r ~/.claude/skills/paper-writing/template/. <target_dir>
   ```

3. Create figure directory:
   ```bash
   mkdir -p <target_dir>/figure
   ```

4. Update `main.tex` in place:
   - Replace `\title{...}` with the paper title
   - Replace `\author{...}` with the authors
   - Replace `\journalname{}` with the journal name
   - Replace `\titlerunning{...}` with a short title

5. Print build instructions:
   ```
   Compile order:
     1. pdflatex main
     2. makeindex main.nlo -s nomencl.ist -o main.nls
     3. bibtex main
     4. pdflatex main
     5. pdflatex main

   Nomenclature rebuild (when nomenclature changes):
     makeindex main.nlo -s nomencl.ist -o main.nls
   ```

### Project structure after init

```
<target_dir>/
├── main.tex
├── paper.bib
├── svjour3.cls
├── svglov3.clo
├── spbasic.bst
├── spmpsci.bst
├── spphys.bst
└── figure/
    └── (PDF figures go here)
```

### Figure inclusion snippet

```latex
\begin{figure}[H]
    \centering
    \includegraphics[width=0.8\textwidth]{figure/my_plot.pdf}
    \caption{...}
    \label{fig:...}
\end{figure}
```

### Table snippet (house style)

```latex
\begin{table}[H]
    \centering
    \caption{...}
    \label{tab:...}
    \fbox{
        \resizebox{0.95\textwidth}{!}{
            \begin{tabular}{l l l l l}
                \multicolumn{5}{c}{\bf Table title}         \\[0.5ex]
                \hline\hline                                 \\[-2ex]
                Col1 & Col2 & Col3 & Col4 & Col5            \\[0.5ex]
                \hline                                       \\[-1.5ex]
                ...  & ...  & ...  & ...  & ...              \\[0.5ex]
                \hline
            \end{tabular}
        }
    }
\end{table}
```

---

## Mode: outline

**Goal**: Build a section structure before any writing begins.

### Process

1. Ask: what is the main contribution of this paper?
2. Ask: what results/simulations are available?
3. Propose a section structure. User approves or adjusts before any prose is drafted.

### Standard structure

```
1. Introduction
2. Mathematical Model
3. Numerical Examples
4. Conclusion
Appendix (optional)
```

---

## Mode: write

**Goal**: Draft or improve prose for a computational modeling / mathematics paper.

### Common rules for this field

- Every quantitative claim traces to a table or figure in the paper
- Error norms must be specified explicitly: L², H¹, or L∞
- Convergence rates must be stated numerically, not qualitatively
- Solver and preconditioner names are always spelled out
- Never write "good agreement" — use actual norms or percentages
- Notation is defined on first use
- Figure captions are self-contained
- Missing citations are flagged as `[CITE]`, never invented

### Anti-AI guard

Before delivering any prose, remove:

| Banned pattern | Action |
|---|---|
| "delve into" | replace with "examine", "discuss", "present" |
| "crucial", "pivotal", "paramount" | state why it matters concretely |
| "In today's rapidly evolving landscape" | delete, start with the fact |
| "It is worth noting that" | delete, state the fact directly |
| "This paper aims to" | replace with "This paper presents / develops / proposes" |
| Throat-clearing opener | start with the concrete claim or result |
| Uniform paragraph rhythm | vary sentence and paragraph length deliberately |

---

## Conventions Reference

| Item | Convention |
|---|---|
| Journal class | `svjour3` (Springer) |
| Bibliography style | `spmpsci` |
| Table border | `\fbox` + `\resizebox{0.95\textwidth}{!}` |
| Table rules | `\hline\hline` top, `\hline` header sep, `\hline` bottom |
| Row spacing | `\\[0.5ex]`, `\\[-2ex]` after hlines |
| Figures | PDF files in `figure/`, included via `\includegraphics` |
| Nomenclature rebuild | `makeindex main.nlo -s nomencl.ist -o main.nls` |
| Missing citations | flag as `[CITE]` |

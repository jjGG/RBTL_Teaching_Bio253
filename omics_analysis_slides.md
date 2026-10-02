# Omics Data Analysis via Agentic AI Workflows
## Course Slide Deck & Instructor Lecture Guide

![FGCZ Omics Agentic Workflow Banner](./fgcz_omics_agentic_banner_1790927902020.jpg)

**Host**: Functional Genomics Center Zurich (FGCZ)  
**Institutions**: University of Zurich (UZH) & ETH Zurich  
**Interactive Slide Deck**: [omics_analysis_slides.html](./omics_analysis_slides.html)

---

## Slide 1: Welcome & Overview

### Slide Content
* **Topic**: End-to-End Omics Analysis Using AI Agent Workspaces
* **The Promise**: Transform tabular experimental data (counts, metadata, annotations) into publication-ready differential analyses, visualizations, and scientific reports with agentic coding.
* **Key Learning Goals**:
  1. Structuring an omics project workspace for an AI agent.
  2. Crafting unambiguous, reproducible prompt specifications.
  3. The Autonomous Execution Loop: Reading files, coding, self-debugging, and reporting.
  4. Preventing biological and statistical hallucinations.

> **Instructor Lecture Notes:**
> * Clarify that life sciences datasets are naturally multi-file (expression matrices, clinical/sample metadata, feature annotations).
> * The goal of this session is not just to ask an LLM questions, but to treat the LLM as an autonomous bioinformatician working inside our local directory.

---

## Slide 2: The Multi-File Workspace Pattern

### Directory Tree Architecture
```
my_omics_project/
├── data/
│   ├── counts_matrix.tsv          # Genes/Proteins (rows) × Samples (columns)
│   ├── sample_metadata.csv        # SampleID, Condition, Batch, Replicate
│   └── gene_annotations.tsv       # Ensembl ID, Gene Symbol, Pathway/Biotype
├── prompts/
│   └── analysis_prompt.md         # Full AI instructions & statistical thresholds
├── scripts/
│   └── run_diff_expression.py    # AI generates and executes this script
├── results/
│   ├── pca_plot.png               # Quality control & variance decomposition
│   ├── volcano_plot.png           # Significance vs effect size
│   ├── expression_heatmap.png     # Clustered top DEGs
│   └── differential_results.tsv   # Full statistical output table
├── report.qmd                     # Quarto reproducible document
└── .gitignore                     # Excludes large binaries, virtualenvs, caches
```

### Three Rules for Omics Workspaces
1. **Never combine data and metadata in one file**: Agents parse clean, normalized CSV/TSV tables with standard headers much more reliably.
2. **Include a Data Dictionary**: A short 5-line markdown comment explaining the columns prevents false assumptions about sample group labels.
3. **Keep Raw Sequences Out of the Context Window**: Never place multi-gigabyte `.fastq.gz`, `.bam`, or `.raw` files directly in the root workspace without `.gitignore`.

> **Instructor Lecture Notes:**
> * Highlight that 80% of agentic coding failures stem from messy directories. If the AI doesn't know which column is the experimental condition, it will guess or hallucinate.

---

## Slide 3: Anatomy of an Effective Omics Agent Prompt

### The Master Prompt Template
```markdown
You are a senior bioinformatician analyzing an RNA-seq / Proteomics dataset.

Context & Files:
- Counts: `data/counts_matrix.tsv` (raw integer counts / normalized intensities)
- Metadata: `data/sample_metadata.csv` (SampleID, Condition: Control vs Treated, Batch)
- Annotations: `data/gene_annotations.tsv`

Tasks:
1. Load data and verify that sample names in counts match metadata rows exactly.
2. Perform QC:
   - Compute log-transformed CPM / variance stabilizing transformation (VST).
   - Generate a PCA plot (PC1 vs PC2) colored by Condition and shaped by Batch. Save to `results/pca_plot.png`.
3. Differential Expression Analysis:
   - Fit model: `~ Batch + Condition` using DESeq2 / PyDESeq2.
   - Contrast: Treated vs Control.
   - Filter DEGs: Adjusted p-value (FDR) < 0.05 and |log2FoldChange| >= 1.0.
   - Save full table to `results/differential_results.tsv`.
4. Visualization & Synthesis:
   - Generate a Volcano plot with top 10 significant genes labeled.
   - Generate a hierarchical clustered heatmap of the top 50 DEGs.
   - Write an executive summary of findings to `report.md`.

Execution Constraint:
Write all executable code into `scripts/analyze.py`. Run the script in the terminal.
If errors occur (e.g. package missing, index mismatch), read the terminal traceback,
fix the script, and rerun until exit code 0 is achieved.
```

> **Instructor Lecture Notes:**
> * Contrast this prompt with a lazy prompt like *"Do differential expression for me"*.
> * Point out the four essential pillars:
>   1. Explicit file locations.
>   2. Explicit statistical thresholds ($p_{\text{adj}} < 0.05$, $|\log_2 \text{FC}| \ge 1$).
>   3. Explicit model covariates (accounting for Batch).
>   4. Self-executing terminal constraint.

---

## Slide 4: The Autonomous Execution Loop

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          THE AGENT EXECUTION LOOP                           │
└─────────────────────────────────────────────────────────────────────────────┘
       │
       ▼
[01. Inspect]       ➔ Checks `head -n 5` of tables, column names, matrix dimensions.
       │
       ▼
[02. Plan & Write]  ➔ Authors `scripts/analyze.py` using appropriate scientific libraries.
       │
       ▼
[03. Run in Shell]  ➔ Executes `python3 scripts/analyze.py` via tool calls.
       │
       ├───► If Error: Inspects traceback (e.g. `KeyError: 'condition'`) 
       │               ➔ Edits script and retries until exit code 0.
       ▼
[04. Synthesize]    ➔ Checks outputs in `results/`, verifies figure creation,
                      and drafts `report.md` summarizing biological significance.
```

> **Instructor Lecture Notes:**
> * Explain the shift in the student's responsibility: The student does not spend 45 minutes fixing syntax errors or package imports. Instead, the student reviews the statistical outputs and evaluates biological plausibility.

---

## Slide 5: Automated Scientific Reporting

### Deliverables Expected from an Agentic Workflow:
1. **Figures**:
   * **PCA Plot**: Does variance separate primarily along PC1 by biological condition, or by batch?
   * **Volcano Plot**: Are up- and down-regulated genes balanced? Are expected marker genes prominent?
   * **Heatmap**: Does unsupervised hierarchical clustering group biological replicates together?
2. **Structured Tables**:
   * Filtered lists of up-regulated and down-regulated candidates with log2FC, p-value, and FDR.
3. **Executable Quarto / Markdown Report**:
   * Methods section describing library versions and normalization algorithms used.
   * Executive summary interpreting key biological pathways (e.g., cell cycle, immune response, apoptosis).

> **Instructor Lecture Notes:**
> * Encourage students to require the agent to generate Quarto (`.qmd`) or RMarkdown (`.Rmd`) files, ensuring full computational reproducibility.

---

## Slide 6: Critical Bioinformatic Guardrails

| Common Pitfall | Risk / Consequence | Agent Guardrail Prompt Instruction |
| :--- | :--- | :--- |
| **Unadjusted p-values** | Thousands of false positive genes reported | *"Filter strictly on Benjamini-Hochberg adjusted p-value (FDR < 0.05), never raw p-value."* |
| **Sample order mismatch** | Inverted sample labels, wrong condition contrasts | *"Assert that count column names exactly match metadata sample IDs in the same order."* |
| **Missing Batch in design** | Batch variance masks true biological signal | *"Include `~ Batch + Condition` in the linear model design matrix."* |
| **Gigabyte file ingestion** | Triggers agent token limits & rate limit lockouts | *"Keep large raw data out of context; write streaming Python/R scripts to process rows."* |

---

## Slide 7: Live Lab Protocol (90-Minute Practical)

1. **Step 1: Setup Workspace (10 min)**
   * Clone the template repo: `git clone https://github.com/fgcz/lab-omics-agentic`
   * Open the directory in **Google Antigravity**.
2. **Step 2: Inspect Inputs (15 min)**
   * Check `data/counts_matrix.tsv` and verify sample naming in `data/sample_metadata.csv`.
3. **Step 3: Execute Agentic Run (30 min)**
   * Feed the structured prompt to Antigravity's agent.
   * Watch the agent write code, execute the script in terminal, and self-correct any package issues.
4. **Step 4: Scientific Review & Discussion (35 min)**
   * Open `results/pca_plot.png` and `results/volcano_plot.png`.
   * Compare results with your lab partner: Did different prompts produce identical candidate genes?

---

## Slide 8: Key Takeaways

* **Directory Hygiene is Mandatory**: A clean structure ensures the AI produces reliable code.
* **You are the Scientist, AI is the Coder**: The human validates biological assumptions and statistical thresholds; the agent eliminates boilerplate coding friction.
* **Reproducibility by Design**: Every agent run must leave behind standalone, documented scripts and reports that can run without the AI.

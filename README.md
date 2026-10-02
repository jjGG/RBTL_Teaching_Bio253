# RBTL Teaching - BIO253 & FGCZ Course Materials
## Agentic Coding in Higher Education & Life Sciences

Course materials, interactive slide presentations, and lecture guides developed for teaching agentic coding, omics data analysis, and scientific GitHub workflows.

**Institutions**: Functional Genomics Center Zurich (FGCZ) • University of Zurich (UZH) • ETH Zurich  
**Course Context**: BIO253 / Computational Biology & Life Sciences  

---

## 📚 Included Slide Decks & Guides

### 1. Introduction to Agentic Coding & Free Tier Reality
* **Interactive Presentation**: [`agentic_coding_slides.html`](./agentic_coding_slides.html)
* **Markdown Deck & Notes**: [`slides_and_lecture_notes.md`](./slides_and_lecture_notes.md)
* **Banner**: `fgcz_agentic_coding_banner_1790891166206.jpg`
* **Summary**:
  * The transition from chatbots (1.0) and copilots (2.0) to autonomous coding agents (3.0).
  * Why **Claude Code has no free tier** and requires paid subscriptions or API credits.
  * How students can use the **Google Antigravity Free Individual Tier** without paying.
  * Context window capacity ($1\text{M}$–$2\text{M}$ tokens in Gemini vs. $200\text{k}$ in Claude).
  * Rate limits, weekly quota resets vs. 5-hour rolling windows, and free backup API keys via [Google AI Studio](https://aistudio.google.com).

### 2. Omics Data Analysis via Agentic AI Workflows
* **Interactive Presentation**: [`omics_analysis_slides.html`](./omics_analysis_slides.html)
* **Markdown Deck & Notes**: [`omics_analysis_slides.md`](./omics_analysis_slides.md)
* **Banner**: `fgcz_omics_agentic_banner_1790927902020.jpg`
* **Summary**:
  * Structuring multi-file biological workspaces (`data/`, `scripts/`, `results/`, `report.qmd`).
  * Crafting unambiguous prompts specifying statistical thresholds ($p_{\text{adj}} < 0.05$, $|\log_2 \text{FC}| \ge 1$), batch effect covariates, and execution constraints.
  * The autonomous agent loop: inspecting file headers, authoring Python/R scripts, terminal execution, self-debugging, and generating PCA/Volcano/Heatmap plots.
  * Guardrails against bioinformatic hallucinations and data size pitfalls.

### 3. Collaborative GitHub Workflows & Student Group Projects
* **Interactive Presentation**: [`github_groupwork_slides.html`](./github_groupwork_slides.html)
* **Markdown Deck & Notes**: [`github_groupwork_slides.md`](./github_groupwork_slides.md)
* **Banner**: `fgcz_github_groupwork_banner_1790927922039.jpg`
* **Summary**:
  * Best practices for team research projects on GitHub (3–5 students).
  * Standardizing repo layout (`environment.yml`, `README.md`, `src/`, `data/README.md`).
  * The Feature Branch lifecycle (`main` protection, feature branches, pull requests, and peer reviews).
  * Integrating AI agents into Git without compromising code review.
  * **End-of-Course Archival**: Tagging submissions (`hs26-final`), creating cohort reference branches (`cohort/hs2026-final`), and archiving repositories for future years.

---

## 🚀 How to View the Slides

Each interactive slide deck is a self-contained HTML file styled with modern academic themes and keyboard navigation.

To view in your browser:
* Double-click any of the `.html` files in Finder or your IDE file explorer.
* Or run in your terminal:
  ```bash
  # Deck 1: Agentic Coding & Free Tier Landscape
  open agentic_coding_slides.html

  # Deck 2: Omics Analysis Workflows
  open omics_analysis_slides.html

  # Deck 3: GitHub Group Work & Archival
  open github_groupwork_slides.html
  ```

### Controls Inside Presentations
* <kbd>→</kbd> / <kbd>Space</kbd> / <kbd>PageDown</kbd>: Next slide
* <kbd>←</kbd> / <kbd>PageUp</kbd>: Previous slide
* <kbd>N</kbd> or click **Speaker Notes**: Toggle instructor talking points and lecture goals on any slide.

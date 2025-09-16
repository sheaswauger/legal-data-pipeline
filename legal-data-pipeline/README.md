# Legal Data Pipeline – School Safety & Gun Regulation

This repository contains a reproducible research pipeline for collecting, processing, and analyzing **U.S. state laws** related to **school safety** and **gun regulation**.

> Course: *Applied Computational Policy Analysis: Building Legal Data Pipelines for Research* (PUAD 8890 – Section 902, Independent Study, Fall 2025)  
> Instructor: **Prof. Deserai Anderson Crow**  
> Student: **Shea Swauger**

## Project Goals

- Programmatically access legislative texts and metadata (e.g., Open States, LegiScan).
- Build a reproducible pipeline (Snakemake) to fetch, clean, enrich, and bundle a multi-state corpus.
- Document assumptions, limitations, and transformations for transparency.
- Prepare data for downstream NLP (topic modeling, classification).

## Quickstart

### 1) Clone and set up Python
```bash
# clone (after you create a GitHub repo and push this scaffold)
git clone <YOUR-REPO-URL> legal-data-pipeline
cd legal-data-pipeline

# create and activate a virtual environment (choose one)
python3 -m venv .venv && source .venv/bin/activate
# or: conda env create -f environment.yml && conda activate legal-data-pipeline
pip install -r requirements.txt
```

### 2) Configure secrets
Copy `.env.example` → `.env` and fill in any API keys (e.g., Open States, LegiScan).
Never commit secrets; `.env` is in `.gitignore`.

### 3) Test a tiny retrieval
```bash
python -m src.legal_pipeline.retrieve --provider openstates --state CO --limit 10 --out data/raw/co_sample.jsonl
```

### 4) Run the workflow (optional preview)
```bash
# Dry-run to see planned steps
snakemake -n

# Execute the default pipeline
snakemake --cores 1
```

## Repository Layout

```
src/                # Python package ("legal_pipeline/")
data/               # data/raw + data/processed (gitignored by default except placeholders)
docs/               # documentation, diagrams
deliverables/       # course deliverables (tracker, meeting notes, etc.)
.github/workflows/  # CI for lint/tests
scripts/            # CLI helpers / utilities
tests/              # unit tests
```

## Deliverables

- **OSF Project** (link here): <ADD_OSF_LINK>
- **Data Dictionary**: `docs/data_dictionary.md`
- **Workflow Diagram**: `docs/workflow.mmd` (Mermaid)
- **Limitations & Bias Memo**: `docs/limitations.md`
- **License**: `LICENSE` (default MIT; adjust as needed)
- **Syllabus/Tracker**: `deliverables/`

## Usage Examples

```bash
# Fetch bills
python -m src.legal_pipeline.retrieve --provider openstates --state CO --session 2024 --limit 50 --out data/raw/co_2024.jsonl

# Parse/normalize raw JSON into tabular form
python scripts/normalize_json.py data/raw/co_2024.jsonl data/processed/co_2024_bills.parquet

# Enrich with tags
python scripts/tag_metadata.py data/processed/co_2024_bills.parquet data/processed/co_2024_bills_tagged.parquet
```

## Reproducibility

- Deterministic steps are orchestrated with **Snakemake**.
- Environments are declared via `requirements.txt` / `environment.yml`.
- Data files are versioned to the extent licensing allows; otherwise retrieval steps are documented.
- See `docs/repro_checklist.md`.

## Citation & Ethics

- If publishing, cite data sources and consider licensing constraints.
- See `docs/ethics_bias.md` and `docs/limitations.md`.

---

© 2025 Shea Swauger. See `LICENSE` for terms.

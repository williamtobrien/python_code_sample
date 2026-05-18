# Grant Applicant to OpenAlex Author Matching

This repository contains a Python coding sample adapted from a research data project at the University of Chicago Knowledge Lab. The project studies whether the funding of research proposals leads to later publications that reflect the proposed research agenda.

The coding sample focuses on one part of that pipeline: matching grant applicants to OpenAlex author IDs.

## Main Notebook

The primary file is:

[`author_matching_coding_sample.ipynb`](author_matching_coding_sample.ipynb)

The notebook is self-contained and uses synthetic application data to protect confidential grant records. 

## Matching Approach

The matching logic is intentionally conservative. The goal is to avoid false positive author matches, even if that means leaving some applicants unmatched.

The notebook uses two evidence sources:

1. **ORCID match:** match applicant ORCID values to OpenAlex author ORCID values.
2. **Reference-based match:** match cited publication titles to OpenAlex works, then disambiguate the matched work's author list.

Reference-based matches are only accepted when the applicant can be identified clearly among the authors of the cited work. The disambiguation logic checks:

1. last-name agreement,
2. first-initial agreement,
3. full first-name agreement when multiple candidates remain.

If the evidence is missing or ambiguous, the applicant is left unmatched.

## What the Notebook Demonstrates

- Data cleaning and normalization for names, ORCIDs, and publication titles.
- Entity resolution across grant application records and OpenAlex author/work records.
- Conservative author disambiguation using nested OpenAlex authorship data.
- Explicit source prioritization: `orcid > refs_publication`.
- Validation cases that show both successful matches and intentional non-matches.

## Repository Structure

```text
.
├── author_matching_coding_sample.ipynb   # Main coding-sample notebook
├── src/author_matcher/                   # Optional reusable Python module
├── tests/                                # Unit tests for matching helpers
├── pyproject.toml                        # Package metadata and dependencies
└── README.md
```

## Setup

Create an environment with Python 3.10 or later, then install the package in editable mode:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
```

Launch the notebook:

```bash
jupyter notebook author_matching_coding_sample.ipynb
```

If Jupyter is not already installed in your environment, install it first:

```bash
pip install notebook
```

## Optional Tests

The notebook is the main presentation artifact, but the helper logic is also available under `src/` with tests:

```bash
pytest
```

## Data Note

The original grant application data are confidential. The notebook therefore uses synthetic application records and simplified OpenAlex-like records to make the matching behavior reviewable without exposing private data.

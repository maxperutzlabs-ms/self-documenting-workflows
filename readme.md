# Self-Documenting Proteomics Workflows

*Max Perutz Labs Mass Spectrometry Facility*

This is the companion repository for our poster, *"Self-Documenting by Design: Continuity in Proteomics Analysis Across Users and Time."*

Mass spectrometry projects live far longer than a single analysis phase. In our core facility, we routinely face three operational hurdles:

1) **Long time gaps:** Returning to a complex dataset months or years later to support our users in finalizing a manuscript or for additional analysis of the initial dataset.
2) **Operator transitions:** Transferring ongoing or completed projects between team members without losing context.
3) **Information fragmentation:** Relying on memory or scattered notes for search parameters, custom reference databases, and analysis decisions.

Rather than relying on active, manual hand-overs, we embed self-documentation directly into our project architecture. By standardizing folder structures, sequence database tracking, post-processing notebooks, and reporting any project can be revisited or resumed by any team member at any point.

Here we share some ofour facility's internal guidelines, conventions, and tools. These represent our current practices, offered as a starting point for discussion and adaptation rather than rigid universal rules.

We invite you to open an issue or start a discussion to share how your facility or research group manages project continuity.

## Overview

> (!) Work In Progress

- [Project Directory Structure](#project-structure): Standardized folder layouts and naming logic
- Input Files and Search: FASTA management and raw file conventions
- Data Analysis: Python frameworks for automated report building
- Reporting and Documentation: Core principles for reporting experimental intent
- Facility Tools: Automation scripts and utilities

## Project Structure

A consistent folder structure shared among all projects removes ambiguity, ensures long-term reproducibility, and lets any team member step into an analysis seamlessly.

```text
project-directory/
├─ data               # Data files and processing results
│  ├─ external            # Additional reference data from outside sources
│  ├─ fasta               # FASTA files used in this project
│  ├─ raw                 # Raw instrument files
│  ├─ processed           # Primary search engine output (e.g. FragPipe or Spectronaut)
│  ├─ interim             # Optional: intermediate and temporary results
│  ├─ post_processed      # Post-processed tables (e.g., filtered, imputed, normalized matrices)
│  └─ analyzed            # Downstream output of final analysis (e.g. figures, enrichment results, etc.)
├─ notebooks          # Notebooks (Jupyter notebooks, R Markdown, etc.)
├─ src                # Optional: Code shared between notebooks
├─ reports            # Final deliverables for users (HTML reports, PDFs, etc.)
├─ references         # Optional: Material too extensive for readme
└─ README.md
```

To find out more about our conventions and detailed folder-naming logic, see the [Project Structure Guidelines](docs/project-structure.md).

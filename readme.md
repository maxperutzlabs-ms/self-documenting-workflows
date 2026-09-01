# Self-Documenting Proteomics Workflows

*Max Perutz Labs Mass Spectrometry Facility*

This is the companion repository for our poster, *"Self-Documenting by Design: Continuity in Proteomics Analysis Across Users and Time."*

Mass spectrometry projects live far longer than a single analysis phase. In our core facility, we routinely face three operational hurdles:

1) **Long time gaps:** Returning to a complex dataset months or years later to support our users in finalizing a manuscript or for additional analysis of the initial dataset.
2) **Operator transitions:** Transferring ongoing or completed projects between team members without losing context.
3) **Information fragmentation:** Relying on memory or scattered notes for search parameters, custom reference databases, and analysis decisions.

Rather than relying on active, manual hand-overs, we embed self-documentation directly into our project architecture. By standardizing folder structures, sequence database tracking, post-processing notebooks, and reporting, any project can be revisited or resumed by any team member at any point.

Here we share some of **our facility's internal** guidelines, conventions, and tools. These represent our current practices, offered as a starting point for discussion and adaptation rather than rigid universal rules.

We invite you to open an issue or start a discussion to share how your facility or research group manages project continuity.

## Overview

- [Project Directory Structure](#project-structure): Standardized folder layouts and naming logic
- [Input Files and Search](#input-files-and-search): FASTA management and raw file conventions
- [Data Analysis](#data-analysis): Notebook based workflows for post-processing, quality control, and reporting
- [Reporting and Documentation](#reporting-and-documentation): Guidelines for writing reports and documentation

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

To simplify setup, our **Project-management GUI tool** provides a one-click workflow to automatically generate the standard directory structure according to our conventions.

## Input Files and Search

Maintaining consistency across projects, transparency, and long-term reproducibility depends on strict control over protein sequence databases, raw data tracking, and standardized processing parameters.

### FASTA Database Management

We maintain a centralized collection of reference FASTA files to ensure consistency across search runs:

- **Acquisition & Naming**: Standardized guidelines govern database retrieval, versioning, and naming conventions.
- **Custom & Contaminant Sequences**: Guidelines exist for formatting custom sequence headers. A centralized facility contaminants database is maintained directly on [GitHub](https://github.com/maxperutzlabs-ms/perutz-ms-contaminants).

Refer to the [FASTA guidelines](docs/fasta-guidelines.md) for database acquisition protocols, header conventions, and validation steps.

### Raw File Metadata & Archival

To avoid redundant storage of raw data while maintaining complete traceability:

- **Server Archival**: Instrument .raw files are backed up to a central server, with checksums recorded and validated after transfer.
- **Independent Archiving**: Project folders are archived independently of the primary raw file repository. To avoid raw data duplication, raw files are omitted from the project archive.
- **Metadata Table**: An automatically generated metadata table is placed in `data/raw/`, recording the exact raw files used in the project and their order of acquisition.

### Processing Workflows & Search Engines

Default parameter files for supported search engines (e.g., FragPipe, Spectronaut) are stored in a central repository location:

- **Standard Settings**: All data processing must use these centralized default workflow configurations as a baseline.
- **Minimized Documentation**: By relying on standardized workflows, we reduce the burden of explicit documentation. Operators only need to document *deviations* from facility defaults in the project `README.md`.

### Project-management GUI tool

To simplify project setup, our project-management GUI tool provides one-click workflows to:

- Retrieve and concatenate FASTA files from the centralized repository and the contaminants database from GitHub (including the automated addition of reversed decoy entries).
- Generate the required raw file metadata table within `data/raw/`.

## Data Analysis

We use standardized, template-driven Jupyter Notebooks for data analysis rather than writing custom processing code from scratch for every project or relying on standalone GUI tools.

### Template-Driven Workflow

Analysis notebooks are built around a single `config` block located at the very top of each file:

- **Simple Configuration**: For standard runs, operators only need to modify parameters in the top `config` block—making execution as straightforward as setting parameters in a GUI.
- **Full Flexibility**: Beneath the configuration block, the entire processing pipeline remains fully accessible code. Operators can intervene at any step, adjust algorithms, or append custom exploratory scripts when non-standard processing is required.
- **Available Templates**: We maintain dedicated notebook templates for common mass spectrometry workflows, including protein-level LFQ (DDA and DIA), pilot experiments, PTM site-level analysis, and TMT quantification.

### Core Facility Libraries

Our notebook templates rely on custom, in-house Python libraries to enforce data standardization, robust post-processing, quality control, and consistent output formatting:

- [**msreport**](https://github.com/hollenstein/msreport): Converts primary search engine outputs (e.g., FragPipe, Spectronaut) into standardized tables and provides core algorithms for aggregation, normalization, imputation, statistical testing, and QC visualization.
- [**xlsxreport**](https://github.com/hollenstein/xlsxreport): Programmatically transforms data matrices into well-formatted, multi-tab Excel workbooks ready for end-user delivery or use as supplementary tables.
- [**profasta**](https://github.com/hollenstein/profasta): Utility library for importing, manipulating, and validating sequence entries and headers across FASTA databases.

## Reporting and Documentation

An analysis report should enable anyone outside the project to understand the experimental intent, verify run quality, and evaluate conclusions independently without needing to contact the operator.

### Data Inspection and Reporting Workflow

1. **Initial Notes in `README.md`**: Operators document preliminary observations, rationale, and notes directly in the project `README.md` during data inspection, recording any immediate insights, questions, or hypotheses that arise.
2. **QC Evaluation & Sanity Checks**:
  - Inspect all notebook-generated QC plots. Document any unexpected deviations and investigate root causes if unclear.
  - Confirm that overall technical quality is sufficient to address the experimental question.
  - Perform experimental sanity checks (e.g., presence of expected interactors, positive controls, or quantitative trends).
3. **Excel Deliverable**: Generate the final report in `reports/` using the automatically created Excel file from `xlsxreport`, QC plots from the notebook, and a narrative summary of the analysis.

### Excel Report 

The generated Excel report serves as the primary deliverable for clients and collaborators, providing a self-contained record of the analysis. Coupling the experimental rationale and observations directly with the quantitative data tables ensures long-term context, making it easy to understand project intent when re-evaluating the data later. 

The narrative documentation in the Excel **Overview Tab** must contain the following sections:

- **Question & Experimental Design**: The core objective of the experiment and the analytical strategy used.
- **Special Procedures** *(Include only if applicable)*: Non-standard sample handling, search settings, or processing steps.
- **Results & Observations**: Primary findings, an evaluation of data quality and consistency with expected trends, and notes on technical issues. Includes main takeaways for the collaborator and optional recommendations for follow-up analyses.

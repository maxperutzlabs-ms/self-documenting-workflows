# FASTA Guidelines

These guidelines describe how we handle FASTA databases and custom FASTA entries in our facility. They represent our current practices and conventions and are shared as a starting point for discussion and adaptation rather than as universal rules.

## Reference FASTA databases

### Obtaining FASTA databases from UniProt

We use [UniProt](https://www.uniprot.org/) as the primary source for reference protein sequences.

For routine proteomics and species with a UniProt proteome database, we use the **"one protein sequence per gene (FASTA)"** download and rename the file after downloading.

We use the following naming convention, with each component separated by an underscore:

```
{release}_{proteome-id}_{organism}_{additional-information}.fasta
```

For a UniProt proteome, the components are:

- `{release}` - UniProt release, e.g. `2025_01`
- `{proteome-id}` - UniProt proteome identifier, e.g. `UP000005640`
- `{organism}` - abbreviated organism name, e.g. `H_sapiens`
- `{additional-information}` - additional information relevant to the database, e.g. `1ppg` for "one protein per gene"

The organism naming convention is:

- For commonly used species, use an abbreviated genus name, e.g. H_sapiens.
- For less commonly used species, use the full genus name, e.g. Aspergillus_niger.

**For example:**

```
2026_01_UP000005640_H_sapiens_1ppg.fasta
```

and

```
2026_01_UP000006706_Aspergillus_niger_1ppg.fasta
```

### Species without a UniProt proteome database

If no suitable UniProt proteome database exists, we use the relevant UniProtKB entries instead.

For example, if a species does not have a dedicated proteome, we search UniProtKB and download the available entries as FASTA. The query or selection criteria used to obtain the entries should be documented in a README file.

The filename itself should document the release, organism, and number or type of entries. The naming convention is:

```
{release}_{organism}_{additional-information}.fasta
```

where `{additional-information}` describes the contents, such as the number of entries.

**For example:**

```
2025_01_Spodoptera_frugiperda_28682entries.fasta
```

### FASTA databases from other sources

FASTA databases obtained from sources other than UniProt should be accompanied by documentation describing:

- The source
- The database version
- The date of retrieval
- How the contained proteins were selected

This information should also be included in the README of the project in which the database is used.

### Updating reference databases

Our current practice is to update organism-specific databases only once per year. Databases for commonly used species are updated with the first UniProt release of the year, while less commonly used species are updated as needed. 

## Custom FASTA entries

### When to use custom FASTA files

Custom FASTA files are used when project-specific protein sequences need to be added to a search database. Examples include tagged bait proteins, synthetic constructs, engineered proteins, truncated proteins, or other sequences not present in the selected reference proteome or the contaminants database.

Custom FASTA files should not use generic names such as `target.fasta`. Instead, the name should provide enough information to roughly understand what the file contains without opening it.

### Custom FASTA header format

To ensure that custom entries can be parsed reliably by various software tools, we follow the **UniProtKB FASTA header format**:

```
>db|UniqueIdentifier|EntryName ProteinName OS=OrganismName OX=OrganismIdentifier GN=GeneName PE=ProteinExistence SV=SequenceVersion
```

For synthetic proteins, we use the following template:

```
>xx|UniqueIdentifier|UniqueIdentifier_SYNTH ProteinName OS=Synthetic OX=0000 GN=GeneName PE=1 SV=1
```

Unless there is a specific reason to deviate, we use the following conventions:

- `xx` as the database identifier
- `OS=Synthetic`
- `OX=0000`
- `PE=1`
- `SV=1`
- `UniqueIdentifier_SYNTH` as the `EntryName`
- `UniqueIdentifier` as the protein identification code
- `_SYNTH` as the organism identification code

This makes synthetic entries clearly distinguishable from genuine UniProt entries while retaining a standard, machine-readable format. When creating a **new custom entry**, the following fields need to be defined:

**UniqueIdentifier**

- The identifier should be short, descriptive, and unique. It must not match an existing UniProt accession or another synthetic entry already used in our databases.
- We generally use uppercase alphanumeric characters. To reduce the risk of accidentally creating an identifier resembling a UniProt accession, we use a letter in the second position.

**ProteinName**

- The protein name should clearly describe what the entry represents. It can contain alphanumeric characters, spaces, and dashes.
- For engineered constructs, the name should provide enough information to understand the composition of the construct.

**GeneName**

- The gene name should be short and clear. It may be used to label proteins in plots and reports, so readability is more important than reproducing a long descriptive name.

**Organism (OS and OX)**

If a protein is naturally occurring in an organism but is not included in the selected reference proteome, we recommend using the official organism name and organism identifier for `OS` and `OX` rather than `Synthetic` and `0000`.

For synthetic constructs, we use `OS=Synthetic` and `OX=0000` to clearly indicate that the entry is not a naturally occurring protein.

## Examples

Synthetic protein entries:

```
>xx|CAMTAG|CAMTAG_SYNTH Calmodulin-Tag OS=Synthetic OX=0000 GN=CamTag PE=1 SV=1
```

or

```
>xx|TBIRA|TBIRA_SYNTH Turbo-BirA biotin ligase OS=Synthetic OX=0000 GN=TurboBirA PE=1 SV=1
```

For a proximity-labeling experiment, the expressed protein may be a construct such as FKBP-APEX2-ULK1. We use a short, readable `UniqueIdentifier` and `GeneName`, while documenting the full construct in `ProteinName`:

```
>xx|APEXULK1|APEXULK1_SYNTH FKBP-APEX2 fused to Homo sapiens ULK1 OS=Synthetic OX=0000 GN=APEXULK1 PE=1 SV=1
```

The protein name therefore provides additional documentation of what the sequence represents, while the identifier and gene name remain convenient for use in analysis and reporting.

## Related resources

- [UniProt FASTA header documentation](https://www.uniprot.org/help/fasta-headers)
- [UniProt accession number documentation](https://www.uniprot.org/help/accession_numbers)

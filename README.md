# Coxsackievirus A16 Nextstrain Analysis

This repository provides a comprehensive Nextstrain analysis of Coxsackievirus A16. You can choose to perform either a **VP1 run (>=600 base pairs)** or a **whole genome run (>=6400 base pairs)**. \
Live Nextstrain build can be found under [VP1](https://nextstrain.org/groups/hodcroftlab/coxsackievirus/A16/vp1) or [whole-genome](https://nextstrain.org/groups/hodcroftlab/coxsackievirus/A16/whole-genome).

For those unfamiliar with Nextstrain or needing installation guidance, please refer to the [Nextstrain documentation](https://docs.nextstrain.org/en/latest/).

### Enhancing the Analysis
Most of the data for this analysis can be obtained from [NCBI Virus](https://www.ncbi.nlm.nih.gov/labs/virus/vssi/#/). Instructions for downloading sequences are provided at the end of this README under [Sequences](#sequences). 


> [!IMPORTANT] 
> - All GitHub Actions "monthly runs" and their automatically generated commits live in the automatic-update branch.
>- Inspect scheduled workflows or the generated commits on that branch: switch to `automatic-update` locally or view `.github/workflows/` on that branch in the GitHub UI.
>- If you are modifying scheduled automation, open PRs targeting `automatic-update` (unless you explicitly want changes applied to the default branch immediately).

## Repository Organization
This repository includes the following directories and files:

- `ingest`: Contains Python scripts and the `snakefile` for automatic downloading of CVA16 sequences and metadata.
- `scripts`: Custom Python scripts called by the `snakefile`.
- `snakefile`: The entire computational pipeline, managed using Snakemake. Snakemake documentation can be found [here](https://snakemake.readthedocs.io/en/stable/).
- `vp1`: Sequences and configuration files for the **VP1 run**.
- `whole_genome`: Sequences and configuration files for the **whole genome run**.

### Configuration Files
The `config`, `vp1/config`, and `whole_genome/config` directories contain necessary configuration files:
- `colors.tsv`: Color scheme
- `geo_regions.tsv`: Geographical locations
- `lat_longs.tsv`: Latitude data
- `dropped_strains.txt`: Dropped strains
- `clades_genome.tsv`: Virus clade assignments
- `reference_sequence.gb`: Reference sequence
- `auspice_config.json`: Auspice configuration file

The reference sequence used is [G-10, accession number U05876](https://www.ncbi.nlm.nih.gov/nuccore/U05876).

## Quickstart

### Setup

#### Nextstrain Environment
Install the Nextstrain environment by following [these instructions](https://docs.nextstrain.org/en/latest/guides/install/local-installation.html).


1. Generate reference files (one-time / when updating references)
   - Use helper to fetch GenBank-based reference files used by ingest:
     ```
     python3 ingest/generate_from_genbank.py --reference "U05876.1" --output-dir "whole_genome/config/"
     ```
   - For Enteroviruses, you'll get the CDS usually with: [0];[product];[2].
   - Output appears in `data/references/` and is consumed by `ingest` rules.
   - Check `data/references/pathogen.json` attributes for correctness.

2. Run ingest (produces data/metadata.tsv and data/sequences.fasta)
   - Make vendored scripts executable if needed:
     ```
     chmod +x ./ingest/vendored/* ./ingest/bin/*
     ```
   - Run ingest via its Snakefile or via the main Snakefile:
     ```
     cd ingest
     snakemake all --cores 1
     ```
     or
     ```
     snakemake all --cores 1
     ```
     (See targets below for building specific outputs.)

3. Build specific Nextstrain outputs
   - VP1 (example target name — adapt to repo targets):
     ```
     snakemake  auspice/coxsackievirus_A16_vp1.json --cores 9
     ```
   - Whole genome:
     ```
     snakemake  auspice/coxsackievirus_A16_whole-genome.json --cores 9
     ```

4. Visualize locally with Auspice
   ```
   auspice view --datasetDir auspice
   ```
   - To run multiple views, change PORT:
     ```
     export PORT=4001
     ```

## Notes & tips
- Snakemake targets: inspect the Snakefile to find exact `auspice/` output filenames used in this repo.
- `generate_from_genbank.py` requires specifying feature/product indices used by the script — double-check the script CLI help if you see parsing errors.
- Keep `automatic-update` branch separate for scheduled workflows to avoid accidental merges from automation commits.

### Sequences
- Manual: NCBI Virus (search `CVA16` or Taxid `31704`): https://www.ncbi.nlm.nih.gov/labs/virus/vssi/#/
- Automated: use the `ingest` pipeline (see repository `ingest/` & vendored helpers).

### Vendored scripts
- Vendoring is managed via `git subrepo`. To update vendored ingest scripts:
  - Install git-subrepo per its docs and follow instructions in `ingest/vendored/README.md`.

## Contributing & maintenance
- For changes to scheduled automated updates: PR -> `automatic-update`.
- For code/pipeline/data changes: open PRs against the default branch (or discuss in issues first for large changes).
- For questions: open an [issue](https://github.com/hodcroftlab/coxsackievirus_a16/issues) or contact the maintainers.

## References
- Nextstrain docs: https://docs.nextstrain.org/en/latest/
- Live build: https://nextstrain.org/groups/hodcroftlab/coxsackievirus/A16/vp1
- Reference sequence (G-10, U05876): https://www.ncbi.nlm.nih.gov/nuccore/U05876

## Contact
- https://eve-lab.org/
- For data/pipeline queries: nadia.neuner-jehle[at]swisstph.ch